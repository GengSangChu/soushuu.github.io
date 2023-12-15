---
layout:     post
title:      thinkphpORM
subtitle:   ORM流程分析
date:       2023-12-15
author:     soushuuu
header-img: img/the-first.png
catalog: false
tags:
    - thinkPHP
---

ORM底层其实调用的是查询构造器。我们一并分析，开始。

首先，数据库映射中，一个表就是一个Model，定义如下

~~~php
<?php
namespace app\model;

use think\Model;

class User extends Model
{
}
~~~

定义了Uer的model后，我们就可以在控制器中使用了：

~~~php
$user = User::find(1);
~~~

我们就从这一句，分析ORM的运行机制。

根据上面的定义，我们可以立马知道是调用 Model 中的 find方法（因为 User中没有定义），接着看。

实际上，我们并没有在Model中找到find方法，但是类中却定义了 __callStatic：

~~~php
public static function __callStatic($method, $args)
    {
        if (isset(static::$macro[static::class][$method])) {
            return call_user_func_array(static::$macro[static::class][$method]->bindTo(null, static::class), $args);
        }

        $model = new static();

        return call_user_func_array([$model->db(), $method], $args);
    }
~~~

这下我们知道了，其实调用的是 $model->db() 中的 find，看下 db 方法：

~~~php
public function db($scope = []): Query
    {
        /** @var Query $query */
        $query = self::$db->connect($this->connection)
            ->name($this->name . $this->suffix)
            ->pk($this->pk);

        if (!empty($this->table)) {
            $query->table($this->table . $this->suffix);
        }

        $query->model($this)
            ->json($this->json, $this->jsonAssoc)
            ->setFieldType(array_merge($this->schema, $this->jsonType))
            ->setKey($this->getKey())
            ->lazyFields($this->lazyFields);

        // 软删除
        if (property_exists($this, 'withTrashed') && !$this->withTrashed) {
            $this->withNoTrashed($query);
        }

        // 全局作用域
        if (is_array($scope)) {
            $globalScope = array_diff($this->globalScope, $scope);
            $query->scope($globalScope);
        }

        // 返回当前模型的数据库查询对象
        return $query;
    }
~~~

处理步骤比较多，通过调用db属性的connect方法获取一个 query 对象，并且做了一些设置，后面细看具体设置了什么。

现在问题是db属性没有默认值，上面的调用流程上也没有设置，在那呢？这得看回[thinkphp主流程]({{site.baseurl}}/2023/12/14/thinkphp主流程/)。在这里，初始化了ModelService：

~~~php
public function boot()
    {
        Model::setDb($this->app->db);
        Model::setEvent($this->app->event);
        Model::setInvoker([$this->app, 'invoke']);
        Model::maker(function (Model $model) {
            $config = $this->app->config;

            $isAutoWriteTimestamp = $model->getAutoWriteTimestamp();

            if (is_null($isAutoWriteTimestamp)) {
                // 自动写入时间戳
                $model->isAutoWriteTimestamp($config->get('database.auto_timestamp', 'timestamp'));
            }

            $dateFormat = $model->getDateFormat();

            if (is_null($dateFormat)) {
                // 设置时间戳格式
                $model->setDateFormat($config->get('database.datetime_format', 'Y-m-d H:i:s'));
            }

            $timeField = $config->get('database.datetime_field');
            if (!empty($timeField)) {
                [$createTime, $updateTime] = explode(',', $timeField);
                $model->setTimeField($createTime, $updateTime);
            }
        });
    }
~~~

这下我们知道了，调用的是Db类的connect方法：

~~~php
/**
     * 创建/切换数据库连接查询.
     *
     * @param string|null $name  连接配置标识
     * @param bool        $force 强制重新连接
     *
     * @return ConnectionInterface
     */
    public function connect(string $name = null, bool $force = false)
    {
        return $this->instance($name, $force);
    }

protected function instance(string $name = null, bool $force = false): ConnectionInterface
    {
        if (empty($name)) {
            $name = $this->getConfig('default', 'mysql');
        }

        if ($force || !isset($this->instance[$name])) {
            $this->instance[$name] = $this->createConnection($name);
        }

        return $this->instance[$name];
    }
~~~

可以看出使用了配置文件中的 mysql 连接配置。看它的调用链：

~~~php
DbManager::connect
    ->DbManager::instance
        ->DbManager::instance
            ->\think\db\connector\Mysql::__construct
               (即think\db\Connection::__construct)
               ->\think\db\builder\Mysql::__construct
                 (即think\db\BaseBuilder::__construct)
~~~

之所以是调用\think\db\builder\Mysql 这个连接器的 builder 跟之所以调用 \think\db\connector\Mysql 一样是可以通过配置文件设置的。

所以上面 `self::$db->connect($this->connection)` 这段代码其实调用的是 `\think\db\connector\Mysql`实例。

接着是调用了这个实例 name 方法：

~~~php
public function name($name)
    {
        return $this->newQuery()->name($name);
    }

public function newQuery()
    {
        $class = $this->getQueryClass();

        /** @var BaseQuery $query */
        $query = new $class($this);

        $timeRule = $this->db->getConfig('time_query_rule');
        if (!empty($timeRule)) {
            $query->timeRule($timeRule);
        }

        return $query;
    }

public function getQueryClass(): string
    {
        return $this->getConfig('query') ?: Query::class;
    }
~~~

至此， 可以看出是获取了一个 think\db\Query 实例，然后是调用 pk 方法，即设置主键。接下来又是若干设置：

~~~php
$query->model($this)
            ->json($this->json, $this->jsonAssoc)
            ->setFieldType(array_merge($this->schema, $this->jsonType))
            ->setKey($this->getKey())
            ->lazyFields($this->lazyFields);
~~~

* 设置model实例也即我们构造的User实例
* 设置JSON字段信息
* 设置字段类型信息
* 设置主键值
* 设置延迟写入字段

然后设置软删除，接着就调用 think\db\Query 实例的 find 方法。

~~~php
public function find($data = null)
    {
        if (!is_null($data)) {
            // AR模式分析主键条件
            $this->parsePkWhere($data);
        }

        if (empty($this->options['where']) && empty($this->options['order'])) {
            $result = [];
        } else {
            $result = $this->connection->find($this);
        }

        // 数据处理
        if (empty($result)) {
            return $this->resultToEmpty();
        }

        if (!empty($this->model)) {
            // 返回模型对象
            $this->resultToModel($result);
        } else {
            $this->result($result);
        }

        return $result;
    }
~~~

接着调用的是`$this->connection->find($this)`，也即`\think\db\connector\Mysql::find`：

~~~php
public function find(BaseQuery $query): array
    {
        // 事件回调
        try {
            $this->db->trigger('before_find', $query);
        } catch (DbEventException $e) {
            return [];
        }

        // 执行查询
        $resultSet = $this->pdoQuery($query, function ($query) {
            return $this->builder->select($query, true);
        });

        return $resultSet[0] ?? [];
    }

protected function pdoQuery(BaseQuery $query, $sql, bool $master = null): array
    {
        // 分析查询表达式
        $query->parseOptions();
        $bind = $query->getBind();

        if ($query->getOptions('cache')) {
            // 检查查询缓存
            $cacheItem = $this->parseCache($query, $query->getOptions('cache'));
            if (!$query->getOptions('force_cache')) {
                $key = $cacheItem->getKey();

                $data = $this->cache->get($key);

                if (null !== $data) {
                    return $data;
                }
            }
        }

        if ($sql instanceof Closure) {
            $sql = $sql($query);
            $bind = $query->getBind();
        }

        if (!isset($master)) {
            $master = $query->getOptions('master') ? true : false;
        }

        $procedure = $query->getOptions('procedure') ? true : in_array(strtolower(substr(trim($sql), 0, 4)), ['call', 'exec']);

        $this->getPDOStatement($sql, $bind, $master, $procedure);

        $resultSet = $this->getResult($procedure);
        $requireCache = $query->getOptions('cache_always') || !empty($resultSet);

        if (isset($cacheItem) && $requireCache) {
            // 缓存数据集
            $cacheItem->set($resultSet);
            $this->cacheData($cacheItem);
        }

        return $resultSet;
    }
~~~

在这里，调用了PDO来实现数据库查询，最后把结果集返回。

现在我们看使用查询构造器Db类的操作流程`$res = Db::table('hy_user')->find(1);`。我们使用的是 think\Facade\Db 这一个类，所以：

~~~php
public static function __callStatic($method, $params)
    {
        return call_user_func_array([static::createFacade(), $method], $params);
    }

protected static function createFacade(string $class = '', array $args = [], bool $newInstance = false)
    {
        $class = $class ?: static::class;

        $facadeClass = static::getFacadeClass();

        if ($facadeClass) {
            $class = $facadeClass;
        }

        if (static::$alwaysNewInstance) {
            $newInstance = true;
        }

        return Container::getInstance()->make($class, $args, $newInstance);
    }

protected static function getFacadeClass()
    {
        return 'think\DbManager';
    }
~~~

根据上面三个方法不难看出，我们实际调度的是：`think\DbManager->table('hy_user')->find(1);`，而 DbManager 中并没有 table 方法，而是有 __call 方法：

~~~php
public function __call($method, $args)
    {
        return call_user_func_array([$this->connect(), $method], $args);
    }
~~~

所以，又是上面的调用链：

~~~php
DbManager::connect
    ->DbManager::instance
        ->DbManager::instance
            ->\think\db\connector\Mysql::__construct
               (即think\db\Connection::__construct)
~~~

然后。调用的是`\think\db\connector\Mysql::table`：

~~~php
public function table($table)
    {
        return $this->newQuery()->table($table);
    }
~~~

但是这次没有设置模型相关属性，而是直接调用了`think\db\Query::find`。

而且我们可以注意到，通过门面模式，我们不需要自己实例化Db对象，而是直接通过静态方法的方式进行查询等操作。
