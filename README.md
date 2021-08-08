# tidb.ticat
This project meant to build a unified TiDB system include all related utilities and tools, without extra costs.

From this all non-production labor works of TiDB will be greatly reduced,
hence we could efficiently build a stronger TiDB, and make a better community.

[Why we need this and how it can help](./why-we-need-this-and-how-it-can-help.md)

## Build (or download) ticat
`golang` is needed:
```
$> git clone https://github.com/innerr/ticat
$> cd ticat
$> make
```
Highly recommend to set `ticat` dir to system `$PATH`, it's handy.

## Add this repo to ticat:
```
$> ticat hub.add innerr/tidb.ticat
```

## Usage
Make sure `tiup` and `mysql client` are installed.

### Simple load and run:
Load TPCC data, then run bench, the yaml file is predefined:
```
$> ticat run.tpcc
```

Specify the yaml file and other args:
```
$> ticat run.tpcc yaml=<tiup-yaml-file-path> wh=2 thread=4
```

Show all bench results:
```
$> ticat bench.records
```

### Load once, run many times and compare results:
Load and run TPCC, compare v5.1.0 and nightly, reset data on each run:
```
$> ticat run.tpcc.versions-threads versions=v5.1.0,nightly threads=4,8
```

Same as above one, in short form, threads on top of versions:
```
$> ticat run.tpcc.t-v v=v5.1.0,nightly t=4,8
```

Similiar with above one, version on top of threads:
```
$> ticat run.tpcc.v-t v=v5.1.0,nightly t=4,8
```

Based on nightly and replace tikv(or other found modules) with local binary:
```
$> ticat run.tpcc.t-v v=nightly,nightly+./bin t=4,8
```

### Usage of ticat-pipe:
Confirm on each step:
```
$> ticat dbg.step : bench.tpcc
```

Change delay seconds on each step:
```
$> ticat dbg.delay 10 : bench.tpcc
```

Show usage of a command, use `:==` to show more info:
```
$> ticat run.tpcc.t-v :=
```

Dry run and check steps, use `:+` to show more info:
```
$> ticat run.tpcc.t-v :-
```

### Advance usage:
A simple flow is like this, config tidb and workload, then run bench:
```
$> ticat tidb.conf cluster=my-test yaml=my.yaml : tpcc.conf wh=10 : bench.prepare : bench.run
```
We can add more commands in this pipe, for example:
- sleep a while after bench.prepare to waiting for compaction
- scan jitter after bench.run
- not pass arg yaml to tidb.conf, but add an auto-deploying command in front of it

If a meta db is specified, the bench result could be write to a table:
```
$> ticat tidb.conf cluster=my-test yaml=my.yaml : tpcc.conf wh=10 : bench.prepare : bench.run : \
   bench.meta h=localhost p=4100 : bench.record
```

To bench different versions or threads, use commands in `cmp.*`:
```
$> ticat tidb.conf cluster=my-test yaml=my.yaml : tpcc.conf wh=10 : \
   cmp.ver.conf v5.0.0,v5.1.0 : cmp.thread.conf 4,8 : \
   cmp.thread cmp.ver
```
What 'cmp.thread cmp.ver' means:
- cmp.thread cmp.ver: run bench with different versions on each thread number
- cmp.ver cmp.thread: run bench with different threads on each version
- cmp.ver cmp.xthread: similiar with above one, not reset data on each version

Notice that there are some alias in examples, 'cmp.thread' and 'compare.threads' and 'cmp.t' are the same.

## Roadmap
[Architecture, roadmap and progress](./architecture-roadmap-progress.md)

## Advance usage
This repo is a package of many **ticat** components(modules),
for more detail usage please read [README of ticat](https://github.com/innerr/ticat/blob/main/README.md) and docs in ticat repo.
