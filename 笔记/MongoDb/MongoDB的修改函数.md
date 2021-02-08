# MongoDB的修改函数

db.getConlection(表名的字符串)  =db.表名

~.find({}) 全部查询

~.find({列名:数据}) 条件查询

~.update() 修改函数

```js
db.getCollection('taskPeople').find({}).forEach(    --先查询，然后循环执行函数
    function(item){                                 --函数
        db.getCollection('taskInfo').update(
            {"taskFollower.name":item.name},        --要修改的条件
            {$set:{"taskFollower.id":item._id}},    --$set 修改关键字 修改
            false,                                  -- 不存在是否插入，默认false
            true                                    --是否不只更新一行，默认false（只更新一行）
        )
    }
)
```


```js
db.getCollection('taskType').find({}).forEach(
    function(item){
        db.getCollection('taskType').update({"name":{$exists:false}},{$set:{"name":item._id}})
     }
    )

name不存在的行，添加name列，值为taskType表的id
$exists 存在的关键字
$unset 删除对应的列
$set 添加对应的列
```

![MongoDB的修改函数](..\img\MongoDB的修改函数.png)