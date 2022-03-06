---
title: Redis键(key)
tags:
  - Redis
categories:
  数据库
---

redis存储的是：key,value格式的数据，其中key都是字符串，value有5种不同的数据结构


## 操作
`del key名称`存在时删除key

`exists key名称`检查 key 是否存在
`expire key seconds`给 key 设置过期时间，seconds为秒值
`expireat key timestamp`给 key 设置过期时间，timestamp为UNIX时间戳
`pexpire key milliseconds`给 key 设置过期时间,milliseconds为毫秒值

7	PEXPIREAT key milliseconds-timestamp
设置 key 过期时间的时间戳(unix timestamp) 以毫秒计
8	KEYS pattern
查找所有符合给定模式( pattern)的 key 。
9	MOVE key db
将当前数据库的 key 移动到给定的数据库 db 当中。
10	PERSIST key
移除 key 的过期时间，key 将持久保持。
11	PTTL key
以毫秒为单位返回 key 的剩余的过期时间。
12	TTL key
以秒为单位，返回给定 key 的剩余生存时间(TTL, time to live)。
13	RANDOMKEY
从当前数据库中随机返回一个 key 。
14	RENAME key newkey
修改 key 的名称
15	RENAMENX key newkey
仅当 newkey 不存在时，将 key 改名为 newkey 。
16	SCAN cursor [MATCH pattern] [COUNT count]
迭代数据库中的数据库键。
17	TYPE key
返回 key 所储存的值的类型。