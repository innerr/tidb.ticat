# How to land TiDB on TiCat

## The edges of TiDB sub-systems
We could split TiDB system into sub-systems(or call it sub feature sets) by functional types.

There are some clean edges of TiDB sub-systems:
* Deploying, input: `tiup yaml config`, output: `cluster name`
* TiDB cluster related toolset, input: `cluster name`, include start/stop/scale-in/jitter-scanning, etc
* Workloads, input: `mysql address`
* ...

## Component subsets
For each TiDB sub-system, we will create a component subset, develop a group of **ticat** components.

Components of **ticat** use a shared-env(a key-value set) to communicate,
since a sub-system has similar function, it could use a set of fixed key-values.

Take "workloads" as an example, there would be some **ticat** components.
Each component will read env key `mysql.host`(and other keys) to know where to write data,
and write keys `bench.begin` `bench.end` to record the begin and end time.

Using this **ticat** env mechanism, we could concate components like unix-pipe.

A typical workflow will be like:
`<config-componets>... : <deploy-component> : <workload-componet> : <scanning-components>... : <report-components>`

## Essential ticat env keys
Here is the component subsets and the essential keys they read and write:
```
Config                                 - write:   tidb.cluster (name of the cluster)
                                                  tidb.version

Cluster Operating                      - read:    tidb.cluster
    Deploy                                        tidb.version
    Start
    Scale in
    ...

Workloads                              - read:    mysql.host
                                                  mysql.port
                                         write:   bench.begin
                                                  bench.end
                                                  bench.score

Scanning                               - read:    tidb.cluster
                                                  bench.begin
                                                  bench.end
```

If a TiDB cluster has more than one tidb instances,
there will be more than one mysql addresses.
So we need a set of components "Map TiDB to MySql" to determine mysql address.
```
Map TiDB to MySql                      - read:    tidb.cluster
                                         write:   mysql.host
                                                  mysql.port
```

## The modules delivering plan and progress
```
*----    Config                        - not important

-----    Cluster Operating
-----        Deploy
****-            Support local bin
*----            By template
-----            Auto deploy
*****            Link to manually deployed cluster
*****        Start
*****        Stop
*****        Destory
-----        Scale in
-----        Scale out
-----        Yaml changes
-----        Scheduling
-----            Testing dbg
-----        Injecting
-----            Barriers
-----                CPU occupied
-----                Nearly OOM => OOM
-----                Slow disk
-----                Single slow node
-----            Errors
-----                Corrupted disk block
-----                Network error
-----                Failed nodes
-----        Physical data loading
-----            BR
***--            Physical backup|restore

-----    Scanning                               - read: tidb.cluster
*----        Jitter
**---            QPS
**---            Latency
-----                Long tail
-----        Hardware resource usage
-----            CPU
-----            Mem
-----            Network
-----        Balance
-----            Instance
-----            Regions
-----            Read|write requests
-----        Error

-----    Workloads                              - read: mysql.host|port
-----        Benchmarks                         - read: tidb.meta.host|port
*****            TPCC
-----            TPCH
-----            Sysbench
-----            YCSB
-----        Customized workloads
-----            Wide table inserting
-----        Baseline workloads                 - read: tidb.meta.host|port
-----            Big range deleting
-----        Tests
-----            TiDB tests
-----            TiPocket
-----            Autogen tests
-----            CI tests

***--    Workloads multiplying
***--        Compare performance of:
*****            Versions
*****            Threads
*****                Reset or not reset data
*****            Versions * threads
*****                Reset or not reset data
*****            Threads * versions
-----            Yaml configs, for perf-auto-tune
-----            Incompatible storage versions

-----    Map TiDB to MySql
*****        Link to one-tidb cluster
-----        Proxy mode support
```
