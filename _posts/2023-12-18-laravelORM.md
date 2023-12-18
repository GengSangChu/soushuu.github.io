---
layout:     post
title:      laravelORM
subtitle:   laravelORM分析
date:       2023-12-15
author:     soushuuu
header-img: img/the-first.png
catalog: false
tags:
    - laravel
---

我们分析这一个操作的流程：

~~~php
$users = User::all();
~~~

User定义：

~~~php
class User extends Model
{
    use HasFactory;

// 表名默认加s
    protected $table = "hy_user";

}
~~~

如果不修改源码，建议设置表名，否则Model基类会根据类名自动设置默认表名。
现在我们探索all方法的定义：

~~~php

    public static function all($columns = ['*'])
    {
        return static::query()->get(
            is_array($columns) ? $columns : func_get_args()
        );
    }

    public static function query()
    {
        // \Illuminate\Database\Eloquent\Builder
        return (new static)->newQuery();
    }


    public function newQuery()
    {
        return $this->registerGlobalScopes($this->newQueryWithoutScopes());
    }


    public function newQueryWithoutScopes()
    {
        // \Illuminate\Database\Eloquent\Builder
        return $this->newModelQuery()
            ->with($this->with)
            ->withCount($this->withCount);
    }

public function newModelQuery()
    {
        // \Illuminate\Database\Eloquent\Builder
        return $this->newEloquentBuilder(
            $this->newBaseQueryBuilder()
        )->setModel($this);
    }
protected function newBaseQueryBuilder()
    {
        // \Illuminate\Database\Query\Builder
        return $this->getConnection()->query();
    }

public function getConnection()
    {
        // Illuminate\Database\MySqlConnection
        return static::resolveConnection($this->getConnectionName());
    }
~~~

上面的调用链可以看到如下的实例化链：

~~~
Illuminate\Database\Eloquent::all
->\Illuminate\Database\Eloquent\Builder
    ->\Illuminate\Database\Query\Builder
        ->Illuminate\Database\MySqlConnection
~~~

回到 all 方法后，调用 `\Illuminate\Database\Eloquent\Builder::get`：

~~~php
public function get($columns = ['*'])
    {
        // Illuminate\Database\Eloquent\Builder
        $builder = $this->applyScopes();

        // If we actually found models we will also eager load any relationships that
        // have been specified as needing to be eager loaded, which will solve the
        // n+1 query issue for the developers to avoid running a lot of queries.
        if (count($models = $builder->getModels($columns)) > 0) {
            $models = $builder->eagerLoadRelations($models);
        }
        // App\Models\User::newCollection
        return $builder->getModel()->newCollection($models);
    }

public function getModels($columns = ['*'])
    {
        // App\Models\User::hydrate
        // 操作模型关联ORM，设置关系
        return $this->model->hydrate(
            // Illuminate\Database\Query\Builder::get
            // 查询的真正逻辑
            $this->query->get($columns)->all()
        )->all();
    }

protected function hydratePivotRelation(array $models)
    {
        // To hydrate the pivot relationship, we will just gather the pivot attributes
        // and create a new Pivot model, which is basically a dynamic model that we
        // will set the attributes, table, and connections on it so it will work.
        foreach ($models as $model) {
            $model->setRelation($this->accessor, $this->newExistingPivot(
                $this->migratePivotAttributes($model)
            ));
        }
    }

public function newCollection(array $models = [])
    {
        // \Illuminate\Database\Eloquent\Collection
        return new Collection($models);
    }

public function __construct($items = [])
    {
        $this->items = $this->getArrayableItems($items);
    }
~~~

`Illuminate\Database\Query\Builder::get`方法是获取数据的真正方法：

~~~php
public function get($columns = ['*'])
    {
        return collect($this->onceWithColumns(Arr::wrap($columns), function () {
            // Illuminate\Database\Query\Processors\MySqlProcessor::processSelect
            // 用于执行 Select query
            return $this->processor->processSelect($this, $this->runSelect());
        }));
    }


protected function runSelect()
    {
        // Illuminate\Database\MySqlConnection::select
        return $this->connection->select(
            $this->toSql(), $this->getBindings(), ! $this->useWritePdo
        );
    }


// Illuminate\Database\MySqlConnection::select
public function select($query, $bindings = [], $useReadPdo = true)
    {
        return $this->run($query, $bindings, function ($query, $bindings) use ($useReadPdo) {
            if ($this->pretending()) {
                return [];
            }

            // For select statements, we'll simply execute the query and return an array
            // of the database result set. Each element in the array will be a single
            // row from the database table, and will either be an array or objects.
            $statement = $this->prepared(
                $this->getPdoForSelect($useReadPdo)->prepare($query)
            );

            $this->bindValues($statement, $this->prepareBindings($bindings));

            $statement->execute();

            return $statement->fetchAll();
        });
    }
~~~

`$this->toSql()`我们的将代码转换成sql语句，`$this->getBindings()`用于获取绑定参数。
`Illuminate\Database\MySqlConnection::run`用于调用PDO执行SQL并获取结果集。

