---
layout:     post
title:      workerman runAll
subtitle:   workerman runAll分析
date:       2023-12-15
author:     soushuuu
header-img: img/the-first.png
catalog: false
tags:
    - workerman
---

workerman在进行一系列设置后，最终都会执行 worker::runAll() 方法，今天就来分析一下这个方法的具体执行流程。

现在先看下 worker 的构造方法：

* 构造workerid
* 构造上下文context
* 将当前对象加入到 workers 数组
* 将当前workerid绑定到 pidMap 数组
* 设置 socket 连接协议
* 创建资源流上下文

~~~php
public function __construct(string $socketName = null, array $socketContext = [])
    {
        // Save all worker instances.
        $this->workerId = spl_object_hash($this);
        $this->context = new stdClass();
        static::$workers[$this->workerId] = $this;
        static::$pidMap[$this->workerId] = [];

        // Context for socket.
        if ($socketName) {
            $this->socketName = $socketName;
            if (!isset($socketContext['socket']['backlog'])) {
                $socketContext['socket']['backlog'] = static::DEFAULT_BACKLOG;
            }
            // stream_context_create — 创建资源流上下文
            $this->socketContext = stream_context_create($socketContext);
        }

        // Try to turn reusePort on.
        /*if (\DIRECTORY_SEPARATOR === '/'  // if linux
            && $socketName
            && \version_compare(php_uname('r'), '3.9', 'ge') // if kernel >=3.9
            && \strtolower(\php_uname('s')) !== 'darwin' // if not Mac OS
            && strpos($socketName,'unix') !== 0 // if not unix socket
            && strpos($socketName,'udp') !== 0) { // if not udp socket

            $address = \parse_url($socketName);
            if (isset($address['host']) && isset($address['port'])) {
                try {
                    \set_error_handler(function(){});
                    // If address not in use, turn reusePort on automatically.
                    $server = stream_socket_server("tcp://{$address['host']}:{$address['port']}");
                    if ($server) {
                        $this->reusePort = true;
                        fclose($server);
                    }
                    \restore_error_handler();
                } catch (\Throwable $e) {}
            }
        }*/
    }
~~~

然后再看runAll：

~~~php
public static function runAll(): void
    {
        static::checkSapiEnv();
        static::init();
        static::parseCommand();
        static::lock();
        static::daemonize();
        static::initWorkers();
        static::installSignal();
        static::saveMasterPid();
        static::lock(LOCK_UN);
        static::displayUI();
        static::forkWorkers();
        static::resetStd();
        static::monitorWorkers();
    }
~~~

第一个方法仅仅是验证SAPI是否cli，否则不允许执行。作为常驻内存的一个PHP网络应用，也的确不适合使用FPM。

~~~php

protected static function checkSapiEnv(): void
    {
        // Only for cli.
        if (PHP_SAPI !== 'cli') {
            exit("Only run in command line mode \n");
        }
    }

~~~


接下来的 init 方法则进行了一系列设置：
* 错误处理函数：set_error_handler
* 设置 debug 回溯
* 设置进程的pid文件
* 设置日志log文件
* 设置运行状态
* 设置全局静态变量数组中的start_timestamp
* 设置进程title
* 将每个启动的worker进程通过id添加到当前worker进程（即其他worker为当前worker的从属worker）
* 初始化定时器Timer（设置SIGALRM信号处理函数）

~~~php

protected static function init(): void
    {
        set_error_handler(function ($code, $msg, $file, $line) {
            static::safeEcho("$msg in file $file on line $line\n");
        });

        // Start file.
        $backtrace = debug_backtrace(DEBUG_BACKTRACE_IGNORE_ARGS);
        static::$startFile = end($backtrace)['file'];

        $uniquePrefix = str_replace('/', '_', static::$startFile);

        // Pid file.
        if (empty(static::$pidFile)) {
            static::$pidFile = __DIR__ . "/../$uniquePrefix.pid";
        }

        // Log file.
        if (empty(static::$logFile)) {
            static::$logFile = __DIR__ . '/../../workerman.log';
        }

        if (!is_file(static::$logFile)) {
            // if /runtime/logs  default folder not exists
            if (!is_dir(dirname(static::$logFile))) {
                @mkdir(dirname(static::$logFile), 0777, true);
            }
            touch(static::$logFile);
            chmod(static::$logFile, 0644);
        }

        // State.
        static::$status = static::STATUS_STARTING;

        // For statistics.
        static::$globalStatistics['start_timestamp'] = time();

        // Process title.
        static::setProcessTitle('WorkerMan: master process  start_file=' . static::$startFile);

        // Init data for worker id.
        static::initId();

        // Timer init.
        Timer::init();
    }

protected static function initId(): void
    {
        foreach (static::$workers as $workerId => $worker) {
            $newIdMap = [];
            $worker->count = max($worker->count, 1);
            for ($key = 0; $key < $worker->count; $key++) {
                $newIdMap[$key] = static::$idMap[$workerId][$key] ?? 0;
            }
            static::$idMap[$workerId] = $newIdMap;
        }
    }

public static function init(EventInterface $event = null): void
    {
        if ($event) {
            self::$event = $event;
            return;
        }
        if (function_exists('pcntl_signal')) {
            pcntl_signal(SIGALRM, ['\Workerman\Timer', 'signalHandle'], false);
        }
    }

~~~

接下来是解析输入的命令行，可以看到支持6个命令：

* start
* stop
* restart
* reload
* status
* connections

还支持两种运行模式：

* -d （守护进程）
* -g （平滑重启、优雅停止）

~~~php

protected static function parseCommand(): void
    {
        if (DIRECTORY_SEPARATOR !== '/') {
            return;
        }

        // Check argv;
        $startFile = basename(static::$startFile);
        $usage = "Usage: php yourfile <command> [mode]\nCommands: \nstart\t\tStart worker in USER mode.\n\t\tUse mode -d to start in DAEMON mode.\nstop\t\tStop worker.\n\t\tUse mode -g to stop gracefully.\nrestart\t\tRestart workers.\n\t\tUse mode -d to start in DAEMON mode.\n\t\tUse mode -g to stop gracefully.\nreload\t\tReload codes.\n\t\tUse mode -g to reload gracefully.\nstatus\t\tGet worker status.\n\t\tUse mode -d to show live status.\nconnections\tGet worker connections.\n";
        $availableCommands = [
            'start',
            'stop',
            'restart',
            'reload',
            'status',
            'connections',
        ];
        $availableMode = [
            '-d',
            '-g'
        ];
        $command = $mode = '';
        foreach (static::getArgv() as $value) {
            if (!$command && in_array($value, $availableCommands)) {
                $command = $value;
            }
            if (!$mode && in_array($value, $availableMode)) {
                $mode = $value;
            }
        }

        if (!$command) {
            exit($usage);
        }

        // Start command.
        $modeStr = '';
        if ($command === 'start') {
            if ($mode === '-d' || static::$daemonize) {
                $modeStr = 'in DAEMON mode';
            } else {
                $modeStr = 'in USER mode';
            }
        }
        static::log("Workerman[$startFile] $command $modeStr");

        // Get master process PID.
        $masterPid = is_file(static::$pidFile) ? (int)file_get_contents(static::$pidFile) : 0;
        // Master is still alive?
        if (static::checkMasterIsAlive($masterPid)) {
            if ($command === 'start') {
                static::log("Workerman[$startFile] already running");
                exit;
            }
        } elseif ($command !== 'start' && $command !== 'restart') {
            static::log("Workerman[$startFile] not run");
            exit;
        }

        $statisticsFile = static::$statusFile ?: __DIR__ . "/../workerman-$masterPid.$command";

        // execute command.
        switch ($command) {
            case 'start':
                if ($mode === '-d') {
                    static::$daemonize = true;
                }
                break;
            case 'status':
                while (1) {
                    if (is_file($statisticsFile)) {
                        @unlink($statisticsFile);
                    }
                    // Master process will send SIGIOT signal to all child processes.
                    posix_kill($masterPid, SIGIOT);
                    // Sleep 1 second.
                    sleep(1);
                    // Clear terminal.
                    if ($mode === '-d') {
                        static::safeEcho("\33[H\33[2J\33(B\33[m", true);
                    }
                    // Echo status data.
                    static::safeEcho(static::formatStatusData($statisticsFile));
                    if ($mode !== '-d') {
                        @unlink($statisticsFile);
                        exit(0);
                    }
                    static::safeEcho("\nPress Ctrl+C to quit.\n\n");
                }
            case 'connections':
                if (is_file($statisticsFile) && is_writable($statisticsFile)) {
                    unlink($statisticsFile);
                }
                // Master process will send SIGIO signal to all child processes.
                posix_kill($masterPid, SIGIO);
                // Waiting a moment.
                usleep(500000);
                // Display statistics data from a disk file.
                if (is_readable($statisticsFile)) {
                    readfile($statisticsFile);
                }
                exit(0);
            case 'restart':
            case 'stop':
                if ($mode === '-g') {
                    static::$gracefulStop = true;
                    $sig = SIGQUIT;
                    static::log("Workerman[$startFile] is gracefully stopping ...");
                } else {
                    static::$gracefulStop = false;
                    $sig = SIGINT;
                    static::log("Workerman[$startFile] is stopping ...");
                }
                // Send stop signal to master process.
                $masterPid && posix_kill($masterPid, $sig);
                // Timeout.
                $timeout = static::$stopTimeout + 3;
                $startTime = time();
                // Check master process is still alive?
                while (1) {
                    $masterIsAlive = $masterPid && posix_kill($masterPid, 0);
                    if ($masterIsAlive) {
                        // Timeout?
                        if (!static::$gracefulStop && time() - $startTime >= $timeout) {
                            static::log("Workerman[$startFile] stop fail");
                            exit;
                        }
                        // Waiting a moment.
                        usleep(10000);
                        continue;
                    }
                    // Stop success.
                    static::log("Workerman[$startFile] stop success");
                    if ($command === 'stop') {
                        exit(0);
                    }
                    if ($mode === '-d') {
                        static::$daemonize = true;
                    }
                    break;
                }
                break;
            case 'reload':
                if ($mode === '-g') {
                    $sig = SIGUSR2;
                } else {
                    $sig = SIGUSR1;
                }
                posix_kill($masterPid, $sig);
                exit;
            default :
                static::safeEcho('Unknown command: ' . $command . "\n");
                exit($usage);
        }
    }

~~~

接下来是获取一个独占锁，然后执行若干写入操作后释放锁：

~~~php

protected static function lock(int $flag = LOCK_EX): void
    {
        static $fd;
        if (DIRECTORY_SEPARATOR !== '/') {
            return;
        }
        $lockFile = static::$pidFile . '.lock';
        $fd = $fd ?: fopen($lockFile, 'a+');
        if ($fd) {
            flock($fd, $flag);
            if ($flag === LOCK_UN) {
                fclose($fd);
                $fd = null;
                clearstatcache();
                if (is_file($lockFile)) {
                    unlink($lockFile);
                }
            }
        }
    }

~~~

获取独占锁后，首先是判断是否为守护进程开启，是则 fork 出一个子进程然后exit掉父进程：

~~~php
protected static function daemonize(): void
    {
        if (!static::$daemonize || DIRECTORY_SEPARATOR !== '/') {
            return;
        }
        umask(0);
        $pid = pcntl_fork();
        if (-1 === $pid) {
            throw new RuntimeException('Fork fail');
        } elseif ($pid > 0) {
            exit(0);
        }
        if (-1 === posix_setsid()) {
            throw new RuntimeException("Setsid fail");
        }
        // Fork again avoid SVR4 system regain the control of terminal.
        $pid = pcntl_fork();
        if (-1 === $pid) {
            throw new RuntimeException("Fork fail");
        } elseif (0 !== $pid) {
            exit(0);
        }
    }
~~~

接着是初始化所有 worker 实例：

* 设置状态文件

然后给每个worker：

* 设置名称
* 设置进程对应的用户名
* 设置上下文 context 的 Socket 名称
* 设置上下文 context 的成功状态
* 设置显示的UI
* 如果没有监听端口复用，则注册一个监听器

~~~php

protected static function initWorkers(): void
    {
        if (DIRECTORY_SEPARATOR !== '/') {
            return;
        }
        static::$statisticsFile = static::$statusFile ?: __DIR__ . '/../workerman-' . posix_getpid() . '.status';
        foreach (static::$workers as $worker) {
            // Worker name.
            if (empty($worker->name)) {
                $worker->name = 'none';
            }

            // Get unix user of the worker process.
            if (empty($worker->user)) {
                $worker->user = static::getCurrentUser();
            } else {
                if (posix_getuid() !== 0 && $worker->user !== static::getCurrentUser()) {
                    static::log('Warning: You must have the root privileges to change uid and gid.');
                }
            }

            // Socket name.
            $worker->context->statusSocket = $worker->getSocketName();

            // Status name.
            $worker->context->statusState = '<g> [OK] </g>';

            // Get column mapping for UI
            foreach (static::getUiColumns() as $columnName => $prop) {
                !isset($worker->$prop) && !isset($worker->context->$prop) && $worker->context->$prop = 'NNNN';
                $propLength = strlen((string)($worker->$prop ?? $worker->context->$prop));
                $key = 'max' . ucfirst(strtolower($columnName)) . 'NameLength';
                static::$$key = max(static::$$key, $propLength);
            }

            // Listen.
            if (!$worker->reusePort) {
                $worker->listen();
            }
        }
    }

public function listen(): void
    {
        if (!$this->socketName) {
            return;
        }

        if (!$this->mainSocket) {

            $localSocket = $this->parseSocketAddress();

            // Flag.
            $flags = $this->transport === 'udp' ? STREAM_SERVER_BIND : STREAM_SERVER_BIND | STREAM_SERVER_LISTEN;
            $errno = 0;
            $errmsg = '';
            // SO_REUSEPORT.
            if ($this->reusePort) {
                stream_context_set_option($this->socketContext, 'socket', 'so_reuseport', 1);
            }

            // Create an Internet or Unix domain server socket.
            $this->mainSocket = stream_socket_server($localSocket, $errno, $errmsg, $flags, $this->socketContext);
            if (!$this->mainSocket) {
                throw new Exception($errmsg);
            }

            if ($this->transport === 'ssl') {
                stream_socket_enable_crypto($this->mainSocket, false);
            } elseif ($this->transport === 'unix') {
                $socketFile = substr($localSocket, 7);
                if ($this->user) {
                    chown($socketFile, $this->user);
                }
                if ($this->group) {
                    chgrp($socketFile, $this->group);
                }
            }

            // Try to open keepalive for tcp and disable Nagle algorithm.
            if (function_exists('socket_import_stream') && self::BUILD_IN_TRANSPORTS[$this->transport] === 'tcp') {
                set_error_handler(function () {
                });
                $socket = socket_import_stream($this->mainSocket);
                socket_set_option($socket, SOL_SOCKET, SO_KEEPALIVE, 1);
                socket_set_option($socket, SOL_TCP, TCP_NODELAY, 1);
                restore_error_handler();
            }

            // Non blocking.
            stream_set_blocking($this->mainSocket, false);
        }

        $this->resumeAccept();
    }



public function resumeAccept(): void
    {
        // Register a listener to be notified when server socket is ready to read.
        if (static::$globalEvent && true === $this->pauseAccept && $this->mainSocket) {
            if ($this->transport !== 'udp') {
                static::$globalEvent->onReadable($this->mainSocket, [$this, 'acceptTcpConnection']);
            } else {
                static::$globalEvent->onReadable($this->mainSocket, [$this, 'acceptUdpConnection']);
            }
            $this->pauseAccept = false;
        }
    }
~~~

接下来是安装 signal handler：

~~~php
protected static function installSignal(): void
    {
        if (DIRECTORY_SEPARATOR !== '/') {
            return;
        }
        $signals = [SIGINT, SIGTERM, SIGHUP, SIGTSTP, SIGQUIT, SIGUSR1, SIGUSR2, SIGIOT, SIGIO];
        foreach ($signals as $signal) {
            pcntl_signal($signal, [static::class, 'signalHandler'], false);
        }
        // ignore
        pcntl_signal(SIGPIPE, SIG_IGN, false);
    }
~~~

接着把当前进程 pid 作为 masterpid保存起来，并且保存到文件中：

~~~php
protected static function saveMasterPid(): void
    {
        if (DIRECTORY_SEPARATOR !== '/') {
            return;
        }

        static::$masterPid = posix_getpid();
        if (false === file_put_contents(static::$pidFile, static::$masterPid)) {
            throw new RuntimeException('can not save pid to ' . static::$pidFile);
        }
    }
~~~

然后是解锁操作。

然后是显示UI，也即启动后的启动界面：

~~~php
protected static function displayUI(): void
    {
        $tmpArgv = static::getArgv();
        if (in_array('-q', $tmpArgv)) {
            return;
        }
        if (DIRECTORY_SEPARATOR !== '/') {
            static::safeEcho("---------------------------------------------- WORKERMAN -----------------------------------------------\r\n");
            static::safeEcho('Workerman version:'. static::VERSION. '          PHP version:'. \PHP_VERSION. "\r\n");
            static::safeEcho("----------------------------------------------- WORKERS ------------------------------------------------\r\n");
            static::safeEcho("worker                                          listen                              processes   status\r\n");
            return;
        }

        //show version
        $lineVersion = 'Workerman version:' . static::VERSION . str_pad('PHP version:', 16, ' ', STR_PAD_LEFT) . PHP_VERSION . str_pad('Event-loop:', 16, ' ', STR_PAD_LEFT) . static::getEventLoopName() . PHP_EOL;
        !defined('LINE_VERSION_LENGTH') && define('LINE_VERSION_LENGTH', strlen($lineVersion));
        $totalLength = static::getSingleLineTotalLength();
        $lineOne = '<n>' . str_pad('<w> WORKERMAN </w>', $totalLength + strlen('<w></w>'), '-', STR_PAD_BOTH) . '</n>' . PHP_EOL;
        $lineTwo = str_pad('<w> WORKERS </w>', $totalLength + strlen('<w></w>'), '-', STR_PAD_BOTH) . PHP_EOL;
        static::safeEcho($lineOne . $lineVersion . $lineTwo);

        //Show title
        $title = '';
        foreach (static::getUiColumns() as $columnName => $prop) {
            $key = 'max' . ucfirst(strtolower($columnName)) . 'NameLength';
            //just keep compatible with listen name
            $columnName === 'socket' && $columnName = 'listen';
            $title .= "<w>$columnName</w>" . str_pad('', static::$$key + static::UI_SAFE_LENGTH - strlen($columnName));
        }
        $title && static::safeEcho($title . PHP_EOL);

        //Show content
        foreach (static::$workers as $worker) {
            $content = '';
            foreach (static::getUiColumns() as $columnName => $prop) {
                $propValue = (string)($worker->$prop ?? $worker->context->$prop);
                $key = 'max' . ucfirst(strtolower($columnName)) . 'NameLength';
                preg_match_all("/(<n>|<\/n>|<w>|<\/w>|<g>|<\/g>)/i", $propValue, $matches);
                $placeHolderLength = !empty($matches) ? strlen(implode('', $matches[0])) : 0;
                $content .= str_pad($propValue, static::$$key + static::UI_SAFE_LENGTH + $placeHolderLength);
            }
            $content && static::safeEcho($content . PHP_EOL);
        }

        //Show last line
        $lineLast = str_pad('', static::getSingleLineTotalLength(), '-') . PHP_EOL;
        !empty($content) && static::safeEcho($lineLast);

        if (static::$daemonize) {
            static::safeEcho('Input "php ' . basename(static::$startFile) . ' stop" to stop. Start success.' . "\n\n");
        } else if (!empty(static::$command)) {
            static::safeEcho("Start success.\n"); // Workerman used as library
        } else {
            static::safeEcho("Press Ctrl+C to stop. Start success.\n");
        }
    }
~~~

然后是比较重要的 fork worker 进程，我们只看linux：
给每个worker（我们代码中通过new新建的worker，一般就一个，可以回去看构造函数）设置：

* 再次设置socket名称（如果名称为空）
* 设置当前主worker最大名称长度（用于UI显示，避免显示格式不美观）
* fork 出 $worker->count 个新的子进程 worker 

~~~php
protected static function forkWorkers(): void
    {
        if (DIRECTORY_SEPARATOR === '/') {
            static::forkWorkersForLinux();
        } else {
            static::forkWorkersForWindows();
        }
    }

protected static function forkWorkersForLinux(): void
    {
        foreach (static::$workers as $worker) {
            if (static::$status === static::STATUS_STARTING) {
                if (empty($worker->name)) {
                    $worker->name = $worker->getSocketName();
                }
                $workerNameLength = strlen($worker->name);
                if (static::$maxWorkerNameLength < $workerNameLength) {
                    static::$maxWorkerNameLength = $workerNameLength;
                }
            }

            while (count(static::$pidMap[$worker->workerId]) < $worker->count) {
                static::forkOneWorkerForLinux($worker);
            }
        }
    }

~~~

在`forkOneWorkerForLinux`中：

父进程：

* 将fork出的子进程id保存到`static::$_pidMap`和`static::$_idMap`

子进程：

* 重定向标准IO
* 清空 `static::$pidsToRestart`和`static::$pidMap`
* 删除其它listener
* 删掉所有定时器 Timer
* 更新运行状态为`STATUS_RUNNING`
* 注册关闭函数
* 创建全局循环事件（IO多路复用）并给其设置`error handler`(后面继续分析)
* 重设信号处理
* 初始化Timer
* 设置Title、userAndGroup、当前id（workerid）
* 执行 run() （后面继续分析）
* 执行IO多路复用事件的 `run`方法（后面继续分析）

~~~php

protected static function forkOneWorkerForLinux(self $worker): void
    {
        // Get available worker id.
        $id = static::getId($worker->workerId, 0);
        $pid = pcntl_fork();
        // For master process.
        if ($pid > 0) {
            static::$pidMap[$worker->workerId][$pid] = $pid;
            static::$idMap[$worker->workerId][$id] = $pid;
        } // For child processes.
        elseif (0 === $pid) {
            srand();
            mt_srand();
            static::$gracefulStop = false;
            if (static::$status === static::STATUS_STARTING) {
                static::resetStd();
            }
            static::$pidsToRestart = static::$pidMap = [];
            // Remove other listener.
            foreach (static::$workers as $key => $oneWorker) {
                if ($oneWorker->workerId !== $worker->workerId) {
                    $oneWorker->unlisten();
                    unset(static::$workers[$key]);
                }
            }
            Timer::delAll();

            //Update process state.
            static::$status = static::STATUS_RUNNING;

            // Register shutdown function for checking errors.
            // 注册一个会在php中止时执行的函数
            register_shutdown_function(["\\Workerman\\Worker", 'checkErrors']);

            // Create a global event loop.
            if (!static::$globalEvent) {
                $eventLoopClass = static::getEventLoopName();
                static::$globalEvent = new $eventLoopClass;
                static::$globalEvent->setErrorHandler(function ($exception) {
                    static::stopAll(250, $exception);
                });
            }

            // Reinstall signal.
            static::reinstallSignal();

            // Init Timer.
            Timer::init(static::$globalEvent);

            restore_error_handler();

            static::setProcessTitle('WorkerMan: worker process  ' . $worker->name . ' ' . $worker->getSocketName());
            $worker->setUserAndGroup();
            $worker->id = $id;
            $worker->run();
            // Main loop.
            static::$globalEvent->run();
            if (static::$status !== self::STATUS_SHUTDOWN) {
                $err = new Exception('event-loop exited');
                static::log($err);
                exit(250);
            }
            exit(0);
        } else {
            throw new RuntimeException("forkOneWorker fail");
        }
    }


public function __construct()
    {
        // Avoid process exit due to no listening
        Timer::tick(100000000, function () {
        });
    }
~~~

OK，接下来重点关注：IO多路复用、`woker::run`、IO多路复用的 `run`：


底层的IO多路复用是通过调用Evens目录下的类进行相关操作的，默认使用的是`Select::class`，也可自定义类，需要实现`Workerman\Events\EventInterface`接口。
在 `Evens`目录下有几种实现，我们以swoole为例子进行分析。 
~~~php
protected static function getEventLoopName(): string
    {
        if (static::$eventLoopClass) {
            return static::$eventLoopClass;
        }

        if (class_exists(EventLoop::class)) {
            static::$eventLoopClass = Revolt::class;
            return static::$eventLoopClass;
        }

        $loopName = '';
        foreach (static::$availableEventLoops as $name => $class) {
            if (extension_loaded($name)) {
                $loopName = $name;
                break;
            }
        }

        if ($loopName) {
            static::$eventLoopClass = static::$availableEventLoops[$loopName];
        } else {
            static::$eventLoopClass = Select::class;
        }
        return static::$eventLoopClass;
    }
~~~

设置完网络IO的底层驱动后，就执行`woker::run`：

* 开启端口监听
* 调用我们编写的`onWorkerStart`回调函数

~~~php
public function run(): void
    {
        $this->listen();

        // Try to emit onWorkerStart callback.
        if ($this->onWorkerStart) {
            try {
                ($this->onWorkerStart)($this);
            } catch (Throwable $e) {
                // Avoid rapid infinite loop exit.
                sleep(1);
                static::stopAll(250, $e);
            }
        }
    }
~~~

然后是调用底层驱动的`run`：
调用了 `swoole` 的 `Event` 类的 `wait` 方法。`Event`类是`epoll`的封装，分析完 runAll 最后两个方法后我们接着分析它。

~~~php
public function run(): void
    {
        Event::wait();
    }
~~~


在子进程中调用了网络IO的`eventLoop`后，当`eventLoop`在某种情况下被关闭了，子进程也就结束了，即`exit`了，所以，最后两个方法只被父进程运行。

好了，回到父进程流程，接下来是`resetStd`方法：

* 重定向了当前worker的IO流

~~~php
public static function resetStd(bool $throwException = true): void
    {
        if (!static::$daemonize || DIRECTORY_SEPARATOR !== '/') {
            return;
        }
        global $STDOUT, $STDERR;
        $handle = fopen(static::$stdoutFile, "a");
        if ($handle) {
            unset($handle);
            set_error_handler(function () {
            });
            if ($STDOUT) {
                fclose($STDOUT);
            }
            if ($STDERR) {
                fclose($STDERR);
            }
            if (is_resource(STDOUT)) {
                fclose(STDOUT);
            }
            if (is_resource(STDERR)) {
                fclose(STDERR);
            }
            $STDOUT = fopen(static::$stdoutFile, "a");
            $STDERR = fopen(static::$stdoutFile, "a");
            // Fix standard output cannot redirect of PHP 8.1.8's bug
            if (function_exists('posix_isatty') && posix_isatty(2)) {
                ob_start(function ($string) {
                    file_put_contents(static::$stdoutFile, $string, FILE_APPEND);
                }, 1);
            }
            // change output stream
            static::$outputStream = null;
            static::outputStream($STDOUT);
            restore_error_handler();
            return;
        }
        if ($throwException) {
            throw new RuntimeException('Can not open stdoutFile ' . static::$stdoutFile);
        }
    }
~~~

然后是 `monitorWorkers`，在这里，父进程挂起并等待着，当子进程已经退出并且其状态未报告时返回，而如果所有子进程都关闭了，那么当前的父进程woker也关闭并且进行清理操作，如果存在`onMasterStop`方法则调用：

我们来看下子进程返回后父进程的操作：

* 调用我们写的`onWorkerExit`方法
* 清理`pidMap`中关于该子进程的数据
* 如果当前父进程不是`STATUS_SHUTDOWN`状态，则重新 fork 新的子进程


~~~php
protected static function monitorWorkers(): void
    {
        if (DIRECTORY_SEPARATOR === '/') {
            static::monitorWorkersForLinux();
        } else {
            static::monitorWorkersForWindows();
        }
    }


protected static function monitorWorkersForLinux(): void
    {
        static::$status = static::STATUS_RUNNING;
        while (1) {
            // Calls signal handlers for pending signals.
            pcntl_signal_dispatch();
            // Suspends execution of the current process until a child has exited, or until a signal is delivered
            $status = 0;
            $pid = pcntl_wait($status, WUNTRACED);
            // Calls signal handlers for pending signals again.
            pcntl_signal_dispatch();
            // If a child has already exited.
            if ($pid > 0) {
                // Find out which worker process exited.
                foreach (static::$pidMap as $workerId => $workerPidArray) {
                    if (isset($workerPidArray[$pid])) {
                        $worker = static::$workers[$workerId];
                        // Fix exit with status 2 for php8.2
                        if ($status === SIGINT && static::$status === static::STATUS_SHUTDOWN) {
                            $status = 0;
                        }
                        // Exit status.
                        if ($status !== 0) {
                            static::log("worker[$worker->name:$pid] exit with status $status");
                        }

                        // onWorkerExit
                        if (static::$onWorkerExit) {
                            try {
                                (static::$onWorkerExit)($worker, $status, $pid);
                            } catch (Throwable $exception) {
                                static::log("worker[$worker->name] onWorkerExit $exception");
                            }
                        }

                        // For Statistics.
                        if (!isset(static::$globalStatistics['worker_exit_info'][$workerId][$status])) {
                            static::$globalStatistics['worker_exit_info'][$workerId][$status] = 0;
                        }
                        ++static::$globalStatistics['worker_exit_info'][$workerId][$status];

                        // Clear process data.
                        unset(static::$pidMap[$workerId][$pid]);

                        // Mark id is available.
                        $id = static::getId($workerId, $pid);
                        static::$idMap[$workerId][$id] = 0;

                        break;
                    }
                }
                // Is still running state then fork a new worker process.
                if (static::$status !== static::STATUS_SHUTDOWN) {
                    static::forkWorkers();
                    // If reloading continue.
                    if (isset(static::$pidsToRestart[$pid])) {
                        unset(static::$pidsToRestart[$pid]);
                        static::reload();
                    }
                }
            }

            // If shutdown state and all child processes exited then master process exit.
            if (static::$status === static::STATUS_SHUTDOWN && !static::getAllWorkerPids()) {
                static::exitAndClearAll();
            }
        }
    }
~~~


好了，`runAll`分析结束，我们回去再看看`Swoole\Event`。

刚才我们看到调用了`wait`方法。在`wait`前，workerman调用了`Timer::tick`，其中又调用了c函数`timer_add`：

* 在这里面初始化了一个 SwooleTG.reactor , 也即 swoole::Reactor ，在这里调用了`epoll_create`创建epoll，并且开启了协程。

~~~php
static void timer_add(INTERNAL_FUNCTION_PARAMETERS, bool persistent) {
    zend_long ms;
    Function *fci = (Function *) ecalloc(1, sizeof(Function));
    TimerNode *tnode;

    ZEND_PARSE_PARAMETERS_START(2, -1)
    Z_PARAM_LONG(ms)
    Z_PARAM_FUNC(fci->fci, fci->fci_cache)
    Z_PARAM_VARIADIC('*', fci->fci.params, fci->fci.param_count)
    ZEND_PARSE_PARAMETERS_END_EX(goto _failed);

    if (UNEXPECTED(ms < SW_TIMER_MIN_MS)) {
        php_swoole_fatal_error(E_WARNING, "Timer must be greater than or equal to " ZEND_TOSTR(SW_TIMER_MIN_MS));
    _failed:
        efree(fci);
        RETURN_FALSE;
    }

    // no server || user worker || task process with async mode
    if (!sw_server() || sw_server()->is_user_worker() ||
        (sw_server()->is_task_worker() && sw_server()->task_enable_coroutine)) {
        php_swoole_check_reactor();
    }

    tnode = swoole_timer_add((long) ms, persistent, timer_callback, fci);
    if (UNEXPECTED(!tnode)) {
        php_swoole_fatal_error(E_WARNING, "add timer failed");
        goto _failed;
    }
    tnode->type = TimerNode::TYPE_PHP;
    tnode->destructor = timer_dtor;
    if (persistent) {
        if (fci->fci.param_count > 0) {
            uint32_t i;
            zval *params = (zval *) ecalloc(fci->fci.param_count + 1, sizeof(zval));
            for (i = 0; i < fci->fci.param_count; i++) {
                ZVAL_COPY(&params[i + 1], &fci->fci.params[i]);
            }
            fci->fci.params = params;
        } else {
            fci->fci.params = (zval *) emalloc(sizeof(zval));
        }
        fci->fci.param_count += 1;
        ZVAL_LONG(fci->fci.params, tnode->id);
    } else {
        sw_zend_fci_params_persist(&fci->fci);
    }
    sw_zend_fci_cache_persist(&fci->fci_cache);
    RETURN_LONG(tnode->id);
}
~~~

~~~php
void php_swoole_event_wait() {
    if (php_swoole_is_fatal_error() || !sw_reactor()) {
        return;
    }
    if (!sw_reactor()->if_exit() && !sw_reactor()->bailout) {
        // Don't disable object slot reuse while running shutdown functions:
        // https://github.com/php/php-src/commit/bd6eabd6591ae5a7c9ad75dfbe7cc575fa907eac
#if defined(EG_FLAGS_IN_SHUTDOWN) && !defined(EG_FLAGS_OBJECT_STORE_NO_REUSE)
        zend_bool in_shutdown = EG(flags) & EG_FLAGS_IN_SHUTDOWN;
        EG(flags) &= ~EG_FLAGS_IN_SHUTDOWN;
#endif
        if (sw_reactor()->wait(nullptr) < 0) {
            php_swoole_sys_error(E_ERROR, "reactor wait failed");
        }
#if defined(EG_FLAGS_IN_SHUTDOWN) && !defined(EG_FLAGS_OBJECT_STORE_NO_REUSE)
        if (in_shutdown) {
            EG(flags) |= EG_FLAGS_IN_SHUTDOWN;
        }
#endif
    }
    swoole_event_free();
}
~~~

可以看出，其实调用的是`sw_reactor()->wait(nullptr)`，也即`swoole::ReactorEpoll::wait`，其中调用 `epoll_wait`。


篇幅所限，swoole部分只能粗略带过。后面再在 swoole 相关文章中详细分析。

