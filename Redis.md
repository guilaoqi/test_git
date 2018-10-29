随着超大规模和高并发，传统的关系型数据库暴露了很多难以克服的问题。

Redis是用c语言开发的一种基于键值对的数据库。

字符串、散列类型、列表类型、集合类型、有序集合类型

```
JedisPoolConfig poolConfig=new JedisPoolConfig();
poolConfig.setMaxIdle(30);     //最大闲置个数
poolConfig.setMinIdle(minIdle);    //最小闲置个数
poolConfig.set,MaxTotal(maxTotal); //最大连接数
//1.创建一个redis的连接池
JedisPool pool =new JedisPool(poolconfig,"192.168.186.131",6379);
//2.从池子中获取redis的连接资源
Jedis jedis=pool.getResource();
//3.操作数据库
jedis.set(key,value);
//4.关闭资源
jedis.close();
pool.close();
```

```
//封装：
public class JedisPoolUtils {
    private static JedisPool pool = null;
    static {
        //从redis。properties文件读取配置数据
        InputStream in=JedisPoolUtils.class.getClassLoader().getResourceAsStream("redis.properties");
        Properties pro=new Properties();
        pro.load(in);
        JedisPoolConfig poolConfig=new poolConfig();
        poolConfig.setMaxIdle(Integer.parseInt(pro.get("redis.maxIdle")).toString());
        poolConfig.setMinIdle(Integer.parseInt(pro.get("redis.minIdle")).toString());
        poolConfig.setMaxTotal(Integer.parseInt(pro.get("redis.maxTotal")).toString());
        pool =new JedisPool(poolConfig,pro.getProperty("redis.url"),Integer.parseInt(pro.get("redis.port")).toString());
    }
    //获取Jedis资源
    public static Jedis getJedis(){
        return pool.getResource();
    }
}
```

### hash

#### 设置

hset key field value:

```
hset myhash username jack
```

hmset key field value [field2 value2 ...]

```
hmset myhash2 username rose age 21
```

#### 取值

hget key field：

```
hget myhash username
```

hmget key field1 field2 ...

```
hmget myhash uersname age addr 
```

hgetall key:

```
hgetall myhash
```

#### 删除

hdel key field [field...]

```
hdel myhash username
```

增加

hincrby key field increment //设置key中的field值增加increment

### List

#### 两端添加

lpush key value1 value2 value3...

```
lpush mylist a b c
```

rpush key value1 value2 value3



#### 查看列表

lrange key start end；(先进后出)

```
lrange mylist 0 5
```

#### 两端弹出

lpop mylist

rpop mylist

长度 llent key

#### 扩展命令

lpushx key vlaue

rpushx key value

lrem key count value //删除count个值为value的元素（count为负则从尾向头删除，count等于0则删除链表中所有等于value的值）

linsert key before|after pivot value：在pivot元素前或者后插入value这个元素

### set

sadd key values[value1、value2]

srem key members[member1、member2...]

smembers key //获取所有

sismember key member //判断是存在

### sortset

zadd key score member score2 member2

zscore key member

zcard key //获得数量

### redis 的通用keys操作

- keys pattern 获取所有与pattern匹配的key，返回与该key匹配的keys


### 消息订阅与发布

subscrib channel：订阅频道

pubscribe channel*： 

publish channel content：发布频道

### 数据持久化

1、RDB持久化（默认支持，无需配置）

在指定时间间隔内将内存中的数据集快照写入磁盘。

2、AOF持久化

该机制将以日志的形式记录服务器所处理的每一个写操作，在Redis服务器启动之初就会读取该文件来重新构建数据库，以保证启动后数据库中的数据是完整的。



redis.config里面

```
save 900 1   //修改一个key等900秒存一次
save 300 10  //修改10个key 300秒存一次
save 60 10000 //修改10000个key 60秒存一次
```

