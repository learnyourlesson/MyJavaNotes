**分组查询**

```javascript
db.getCollection("auditEntry").aggregate([
    {
        "$match": {
            "date": {
                "$lt": ISODate("2019-12-26T11:20:04.708Z")
                }
            }
    },
    {
        $group: {
            _id: {
                channelPublishId: "$channelPublishId",
                channelId: '$channelId',
                projectId: '$projectId'
            },
           channel: {
                $push: {
    		channelName:'$channelName',
                    state: '$state',
                    date: '$date'
                }
            },
			counter: {
                $sum: 1
            }
        }
    },
    {
        "$match": {
            "counter": {
                $gt: 1
            }
        }
    }
])
```

```java
Aggregation aggregation = Aggregation.newAggregation(Aggregation.match(Criteria.where("date").lt(new Date())),Aggregation.group("channelPublishId", "channelId", "projectId")
                .push(new BasicDBObject("state", "$state").append("date", "$date")).as("channel")
                .count().as("counter"), Aggregation.match(Criteria.where("counter").gt(1)));

        AggregationResults<Map> auditEntryData = mongoTemplate.aggregate(aggregation, "auditEntry", Map.class);
```

主要是push中要使用BasicDBObject 对象 不能使用一般的对象（子类也可以）

**只显示固定字段和true 查询 之前所有和flase 查询上个月的数据**

```java
Query query;
        if (findFlag) {
            query = new Query(Criteria.where("date").lt(new Date()));
        } else {
            //求上个月的时间
            Calendar newCalendar = Calendar.getInstance();
            newCalendar.add(Calendar.MONTH, -1);
            newCalendar.set(Calendar.DAY_OF_MONTH, 1);
            Date lastMonth = newCalendar.getTime();
            query = new Query(Criteria.where("date").gte(lastMonth).lt(new Date()));
        }
        //过滤字段
        query.fields().include("state").include("channelId").include("date").include("projectId");
```

**使用继承MongoRepository的方式使用数据库主键类型**

```java
public interface MonthlyStatementInfoRepository extends MongoRepository<MonthlyStatementInfo, String（主键类型）> {}联和主键可以直接修改，在po中@Id标注一下就行
```

| SQL 操作/函数 | mongodb聚合操作       |
| ------------- | --------------------- |
| where         | $match                |
| group by      | $group                |
| having        | $match                |
| select        | $project              |
| order by      | $sort                 |
| limit         | $limit                |
| sum()         | $sum                  |
| count()       | $sum                  |
| join          | $lookup （v3.2 新增） |