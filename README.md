RedisShake is mainly used to synchronize data from one redis database to another.<br>
Thanks to the Douyu's WSD team for the support. <br>

* [中文文档](https://yq.aliyun.com/articles/691794)

# 雪球MurmurHash改版，支持目标集群的client sharding逻辑
新增Java版本redis-cluster SDK的 [MurmurHash](http://git.snowballfinance.com/lib/redis-cluster) golang实现，配置新增target的sentinel地址、集群名、shard名，修改restore、sync到target的代码逻辑  
例如，同步数据源到stock-indicator-core这个新集群的shard 01~16 下，详细参考 `redis-shake.conf`：  
```
target.address = 10.10.26.7:26377  
target.cluster = stock-indicator-core  
target.shards = shard01,shard02,shard03,shard04,shard05,shard06,shard07,shard08,shard09,shard10,shard11,shard12,shard13,shard14,shard15,shard16  
```

# Redis-Shake
---
Redis-shake is developed and maintained by NoSQL Team in Alibaba-Cloud Database department.<br>
Redis-shake has made some improvements based on [redis-port](https://github.com/CodisLabs/redis-port), including bug fixes, performance improvements and feature enhancements.<br>

# Main Functions
---
The type can be one of the followings:<br>

* **decode**: Decode dumped payload to human readable format (hex-encoding).
* **restore**: Restore RDB file to target redis.
* **dump**: Dump RDB file from souce redis.
* **sync**: Sync data from source redis to target redis by `sync` or `psync` command. Including full synchronization and incremental synchronization.
* **rump**: Sync data from source redis to target redis by `scan` command. Only support full synchronization. Plus, RedisShake also supports fetching data from given keys in the input file when `scan` command is not supported on the source side.

Please check out the `conf/redis-shake.conf` to see the detailed parameters description.<br>

# Verification
---
User can use [RedisFullCheck](https://github.com/alibaba/RedisFullCheck) to verify correctness.<br>

# Metric
---
Redis-shake offers metrics through restful api and log file.<br>

* restful api: `curl 127.0.0.1:9320/metric`.
* log: the metric info will be printed in the log periodically if enable.

# Code branch rules
---
Version rules: a.b.c.

*  a: major version
*  b: minor version. even number means stable version.
*  c: bugfix version

| branch name | rules |
| - | :- |
| master | master branch, do not allowed push code. store the latest stable version. develop branch will merge into this branch once new version created.|
| **develop**(main branch) | develop branch. all the bellowing branches fork from this. |
| feature-\* | new feature branch. forked from develop branch and then merge back after finish developing, testing, and code review. |
| bugfix-\* | bugfix branch. forked from develop branch and then merge back after finish developing, testing, and code review. |
| improve-\* | improvement branch. forked from develop branch and then merge back after finish developing, testing, and code review.  |

Tag rules:<br>
Add tag when releasing: "release-v{version}-{date}". for example: "release-v1.0.2-20180628"

# Usage
---
Run `./bin/redis-shake.darwin64` or `redis-shake.linux64` which is built in OSX and Linux respectively.<br>
Or you can build redis-shake yourself according to the following steps:
*  git clone https://github.com/alibaba/RedisShake.git
*  cd RedisShake
*  export GOPATH=\`pwd\`/../..
*  cd src/sync
*  govendor sync     #please note: must install govendor first and then pull all dependencies: `go get -u github.com/kardianos/govendor`
*  cd ../../ && ./build.sh
*  ./bin/redis-shake -type=$(type_must_be_sync_dump_restore_or_decode) -conf=conf/redis-shake.conf #please note: user must modify collector.conf first to match needs.

# Shake series tool
---
We also provide some tools for synchronization in Shake series.<br>

* [MongoShake](https://github.com/aliyun/MongoShake): mongodb data synchronization tool.
* [RedisShake](https://github.com/aliyun/RedisShake): redis data synchronization tool.
* [RedisFullCheck](https://github.com/aliyun/RedisFullCheck): redis data synchronization verification tool.

Plus, we have a WeChat group so that users can join and discuss, but the group user number is limited. So please add my WeChat number: `vinllen_xingge` first, and I will add you to this group.<br>

