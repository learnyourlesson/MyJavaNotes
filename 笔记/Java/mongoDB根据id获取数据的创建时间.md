# MongoDb根据id获取数据的创建时间

MongoDb 创建id是根据时间来创建的，我们可以从id中反向把创建时间解析出来，获取id从第10位开始后8位，然后根据16进制解析为int，尾部加上'000'转换为毫秒值得到时间戳，最后转换为时间保存进数据'createTime'字段。

```javascript
db.getCollection('表名').find({"_id" : ObjectId("5fbe13b49da3000694c0b41f")}).forEach(function(item){
    var _str = item._id.toString().substr(10,8);
    var _date = new Date(Number(parseInt(_str, 16).toString()+'000'))
    item.createTime = _date;
    db.表名.save(item)
    })
```

