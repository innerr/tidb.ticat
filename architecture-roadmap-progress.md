## How to land TiDB on ticat
There are some clean edges of TiDB sub systems:
* Deploying, input `tiup yaml config`, output `cluster name`
* TiDB cluster related toolset, input `cluster name`, include start/stop/scale-in/jitter-scanning, etc
* Workloads, input `mysql address`
* ...

## 

# Modules progress
```
Deploy                                 - write: tidb.cluster. read: tidb.tiup.yaml
    By template
    Auto deploy
    Link to manually deployed

Cluster events                         - read: tidb.cluster
    Start                              - write: mysql.host|port
    Stop
    Scale in
    Scale out
    Yaml changes
    Scheduling
        Testing dbg
    Injecting
        Barriers
            CPU occupied
            Nearly OOM => OOM
            Slow disk
        Errors
            Corrupted disk block
            Network error
            Failed TiKV instance
    Physical data loading
        BR
        Physical backup|restore

Scanning                               - read: tidb.cluster
    Jitter
        QPS
        Latency
            Long tail
    Hardware resource usage
        CPU
        Mem
        Network
    Balance
        Instance
        Regions
        Read|write requests
    Error

Workloads                              - read: mysql.host|port
    Tests
        TiDB tests
        TiPocket
        Autogen tests
        CI tests
    Baseline workloads                 - read: tidb.meta.host|port
        Big range deleting
    Benchmarks                         - read: tidb.meta.host|port
        TPCC
        TPCH
        Sysbench
        YCSB
    Customized workloads
        Wide table inserting
```

## Scenario progress
```
**---  New playground
*----  Benchmark
-----  Integration testing
-----  (TBD)
```
