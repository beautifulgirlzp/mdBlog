#### mongodb安装
````txt
1. 通过homebrew安装mongodb
    $ brew update 
    $ brew install mongodb
    如果没有homebrew还是先装一个吧，程序员必备。

2. mongodb 数据默认存在/data/db下，所以需要创建这个文件夹
    $ sudo mkdir -p /data/db
    $ sudo chown xxx /data/db
   请把xxx替换为自己当前的用户名，如果不确定可以先run $ whoami

3. 把mongodb/bin加入$PATH
    $ touch .base_profile
    $ vim .base_profile
````
##### 加入以下地址以后重启terminal
`export MONGO_PATH=/usr/local/mongodb`
`export PATH=$PATH:$MONGO_PATH/bin`



#### mongodb操作
````javascript
mongod --dbpath D:\mondodb\data\node  //设置数据库文件路径
use node //选择数据库
db.yinguit1601.find({userid: "0001"}) // 集合中查找
db.yinguit1601.insert({……}) // 集合中植入新数据
````
