### 安装
brew install redis

### 开启服务 默认地址127.0.0.1：6379
nohup redis-server

### 客户端连接
  redis-cli
  
### redis 对大小写不敏感，但是一般约定指令集用大写

### 获取所有的key 键
  keys *

### 设置一个key
  set ldl.key1 abcckfjlka

### 获取一个key对应的值
  get ldl.key1  // abcckfjlka

### redis的字符串类似于JavaScript中的Number 和String，可以进行递增、递减
  set online.users 0
  incr online.users //1
  decr online.users //0

### redis -> 哈希
  {
    'profile.1' : { "name": "liu","last":"delong"}
    ,'profile.2' : { "name": "meng","last":"yongbo"}
  }
  
  HSET 用法 ：profile.1 name liu ->设置
  hgetall 用法: hgetall profile.1 ->获取
  hdel 用法：hdel profile last ->删除键
  hexists 用法：hexists profile.1 last ->获取字段是否存在， 不存在返回0，存在返回1
  
### redis -> 列表
  增加：rpush /lpush
  获取：rrange /lrange
  redis中的列表相当于JavaScript中的字符串数组
  RPUSH profile.1.jobs "医生"
  RPUSH profile.1.jobs "护士"
  
  lrange profile.1.jobs 0 -1 当第二个参数是-1 返回所有

### redis -> 数据集
  数据集处于列表和哈希之间。
  增加：>sadd mydemo akdl
  获取：>smembers mydemo 
        //akdl
  以相同值再次调用
  >sadd mydemo akdl 
  >smembers mydemo //akdl
  >sadd mydemo akdllll
  >smembers mydemo 
    //akdl
    //adkllll
  
  移除 srem 
  >srem mydemo "akdl"

### 有序数据集
  有序数据集拥有所有数据集的特性，不过是有序的，在redis中，使用有序数据集的情况较少，属于高级用法
  
### Redis 和 Nodejs
  使用Redis存储用户session
  
  why：
    若把用户session数据存储在node进程中，会有两大缺点：
    1.应用成语无法享受多线程带来的好处
    2.每次重启应用，会导致session丢失
    
### node 使用Redis
  dependencies ：{
    "redis": "2.6.0-2"
  } 当前最新版本 2.6.0-2
  
  连接
  var redis = require('redis');
  
  client = redis.createClient(6379, '127.0.0.1');

  client.on('error', function (err) {
    console.log('Error ' + err);
  });
  
  client.set('my key' ,'my value' ,function (err) {
    //...
  }); //客户端将所有redis命令都用函数的方式提供出来，其他smembers,hexists 等命令用法使用相同方式
  
  
### 总结
  Redis 是一个生机盎然的数据库，它既可以用作普通的数据库，也可以作为一种粘合剂来协调多个小程序。
  node本身虽然可以将session存储在内存中，不过将数据分离出来，交个另一个进程，通过简单的tcp来实现连接操作
  ，就可以获得灵活独立的好处，不管程序有没有运行，数据都在那儿O(∩_∩)O~~
  
  
  
