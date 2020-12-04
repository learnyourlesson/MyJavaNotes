排序

db.getCollection('集合名')

.find({"列名" : "...","列名" : "..."})

.sort({"date": -1})时间倒排（1是正序）

remove({"_id" : "monKey"}) 删除id为 monkey的数据

db 查看当前所在数据库

show collections 查看当前所在数据库的所有集合

show dbs 查看所有数据库

use 数据库名  切换数据库

/usr/bin/mongod --smallfiles（使用小文件日志，因为日志空间小于3379MB报错了，有不好删除东西） -f（--config） /etc/mongodb.conf &（后台运行） 重启服务器



mongodb conf 中 bind_ip 是只能同一个网卡下的ip访问的其他的网卡下配置了会启动不了mongodb

mongdb 3 以后使用了新的用户验证方式，旧的验证方式使用连接不上，导致可视化工具连不上