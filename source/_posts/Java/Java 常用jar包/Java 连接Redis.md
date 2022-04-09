---
title: Jedis连接Redis
date: 2022-2-11 14:56:43
tags:
  - Java jar包
categories: Java
---

## 概述
Java 连接Redis 的驱动程序

## 使用步骤
1. 导入驱动jar包
2. 创建连接对象
3. 使用连接对象操作redis
4. 关闭连接(释放资源)


## 连接Redis
``` Java
import redis.clients.jedis.Jedis;
 
public class RedisJava {
    public static void main(String[] args) {
        //连接本地的 Redis 服务
        Jedis jedis = new Jedis("localhost");
        // 如果 Redis 服务设置了密码，需要下面这行，没有就不需要
        // jedis.auth("123456"); 
        System.out.println("连接成功");
        //查看服务是否运行
        System.out.println("服务正在运行: "+jedis.ping());
    }
}
```

## 操作数据结构
这里使用junit测试redis
``` Java
import org.junit.After;
import org.junit.Assert;
import org.junit.Before;
import org.junit.Test;
import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisPool;
import redis.clients.jedis.JedisPoolConfig;

import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Set;

/**
 * @author zrh
 * @date 2022/2/28
 * @apiNote
 */
public class TestRedis {
    private Jedis jedis;

    @Before
    public void testBefore() {
        jedis = new Jedis("127.0.0.1", 6379);
    }

    @After
    public void testAfter() {
        jedis.close();
    }

    /**
     * 操作String类型
     */
    @Test
    public void testString() {
        //  存储数据
        jedis.set("stringKey", "stringValue");
        //  获取数据
        System.out.println(jedis.get("stringKey"));
        //  删除数据
        jedis.del("stringKey");
        System.out.println(jedis.get("stringKey"));
    }

    /**
     * 操作Hash类型
     */
    @Test
    public void testHash(){
        //  存储数据,单个添加
        jedis.hset("hashKey1", "hashField1", "hashValue1");
        jedis.hset("hashKey1", "hashField2", "hashValue2");
        //  获取数据,单个获取
        System.out.println(jedis.hget("hashKey1", "hashField1"));
        System.out.println(jedis.hget("hashKey1", "hashField2"));

        //  存储数据,添加整个map集合
        Map<String,String> map = new HashMap<>();
        map.put("hashField1","hashValue1");
        map.put("hashField2","hashValue2");
        jedis.hmset("hashKey2", map);

        //  获取整个hash,返回值为HashMap类型
        System.out.println(jedis.hgetAll("hashKey2"));

        //  删除数据
        jedis.hdel("hashKey2", "hashField1");
        System.out.println(jedis.hgetAll("hashKey2"));
    }
    /**
     * 操作List类型
     */
    @Test
    public void testList(){
        //  lpush在首部位置添加内容
        jedis.lpush("listKey","listValue1","listValue2");
        //  rpush在末尾位置添加内容
        jedis.rpush("listKey","listValue3","listValue4");

        //  获取list全部数据,返回值为List类型
        List<String> listKey = jedis.lrange("listKey", 0, -1);
        System.out.println(listKey);

        //  删除首部位置元素
        jedis.lpop("listKey");
        //  删除末尾位置元素
        jedis.rpop("listKey");
    }

    /**
     * 操作list person对象; 以序列化的方式存取
     */
    @Test
    public void testList(){
        
        List<Person> list=new ArrayList<Person>();
        list.add(new Person(1, "张三"));
        list.add(new Person(2, "李四"));
        
        jedis.set("key3".getBytes(), SerializeUtil.serialize(list));

        byte[] personlistbyte = jedis.get(("key3").getBytes());
        List<Person> list1 = (List<Person>) SerializeUtil.unserialize(personlistbyte);
        System.out.println(list1);

        jedis.close();
    }

    /**
     * 操作set类型
     */
    @Test
    public void testSet(){
        //  添加数据
        jedis.sadd("setKey","setMember1","setMember2");

        //  获取数据;返回值为Set类型
        Set<String> setKey = jedis.smembers("setKey");

        //  删除数据
        jedis.srem("setKey","setMember1");
        jedis.srem("setKey");
    }

    /**
     * 操作sortedset类型
     */
    @Test
    public void testSortedSet(){
        //  添加数据
        jedis.zadd("sortedsetKey",5.2,"sortedsortMember1");

        //  获取全部数据;返回值为Set类型
        Set<String> sortedsetKey = jedis.zrange("sortedsetKey", 0, -1);

        //  删除数据
        jedis.zrem("sortedsetKey");
    }

    /**
     * 使用连接池
     */

    @Test
    public  void testJedisPool(){
        //0.创建配置对象
        JedisPoolConfig config = new JedisPoolConfig();
        config.setMaxTotal(50);
        config.setMaxIdle(10);
        //1.创建连接池对象
        JedisPool jedisPool = new JedisPool(config,"localhost",6379);
        //2.获取连接
        Jedis jedis = jedisPool.getResource();

        //3.使用
        jedis.set("name","hehe");

        //4.归还连接
        jedis.close();
    }
}
```

## 连接池工具类
配置文件
```properties
host=127.0.0.1
port=6379
maxTotal=50
maxIdle=10
```
工具类
``` Java
import redis.clients.jedis.JedisPool;
import redis.clients.jedis.JedisPoolConfig;

import java.io.IOException;
import java.io.InputStream;
import java.util.Properties;

/**
 * @author zrh
 * @date 2022/3/1
 * @apiNote
 */
public class JedisUtils {
private static JedisPool jedisPool;
    static {
        InputStream inputStream = JedisUtils.class.getClassLoader().getResourceAsStream("jedis.properties");
        Properties properties = new Properties();
        try {
            properties.load(inputStream);
            inputStream.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
        JedisPoolConfig jedisPoolConfig = new JedisPoolConfig();
        jedisPoolConfig.setMaxIdle(Integer.parseInt(properties.getProperty("maxIdle")));
        jedisPoolConfig.setMaxTotal(Integer.parseInt(properties.getProperty("maxTotal")));
        jedisPool = new JedisPool(jedisPoolConfig,properties.getProperty("host"),Integer.parseInt(properties.getProperty("port")));
    }

    public static JedisPool getJedisPool() {
        return jedisPool;
    }
    public static void close(){
        jedisPool.close();
    }
}
```

测试类
``` Java
public class TestRedis {
    //工具类测试
    @Test
    public void testJedisPoolUtil(){
        Jedis jedis = JedisUtils.getJedisPool().getResource();
        jedis.set("name","lisi");
        String name = jedis.get("name");
        System.out.println(name);
        //释放连接
        JedisUtils.close();
    }
}
```
