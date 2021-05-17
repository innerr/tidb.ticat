# tidb.ticat
A [ticat](https://github.com/innerr/ticat) mods repo:
for TiDB

## This doc is WIP

## Target
* Improve the experience and efficiency in non-production scenarios
    * TiDB developing, integration testing, benchmark, POC, etc
* More details: [why-ticat](./doc/why-ticat.md)

## How ticat can achieve that?
* Human friendly
    * Organize job flow with (shell) commands
    * All commands are highly compacted, support fuzzy input, hands on in no time
* Scenario-centered
    * Focus on get things done smoothly in a scenario
* Feature-rich
    * Large amount of modules
        * Components can be easily written in any language
        * ..or from any existing utility by wrapping it up
    * Components' interacting form high-level features
* Write once, run anywhere
    * Save or edit flow easily
    * Share modules and flows easily
* An example: [autotune + benchmark](./doc/usage-draft/benchmark.md)
* More details: [how-ticat-works](./doc/how-ticat-works.md)

## Apply this repo by running:
```bash
ticat hub.add innerr/tidb.ticat
```

## Progress
```
-----  Scenarios
-----      Benchmark
-----      Integration testing
-----      (TBD)
-----  Components
-----      Tiup cluster operating
-----      Ti.sh cluster operating
-----      Cluster raw backup
-----      Jitter detecting
-----      Simple auto config tuning
-----      Workloads: TPCC
-----      Workloads: sysbench
-----      Workloads: ycsb
-----      (TBD)
```

[ticat.hub] (this mark is for bots)
* [git@github.com:innerr/cluster.tidb.ticat](https://github.com/innerr/cluster.tidb.ticat): TiDB cluster management mods
* [git@github.com:innerr/bench.tidb.ticat](https://github.com/innerr/bench.tidb.ticat): TiDB benchmark mods
