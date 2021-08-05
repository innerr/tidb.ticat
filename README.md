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

Load TPCC data, then run bench: (the `yaml` file is predefined, deploy on local host)
```
$> ticat jt.tpcc
```

Load and run TPCC, confirm on each step:
```
$> ticat dbg.step : jt.tpcc
```

Load and run TPCC, delay 3 seconds on each step:
```
$> ticat dbg.delay 3 : jt.tpcc
```

Load and run TPCC, with yaml file:
```
$> ticat jt.tpcc yaml=<tiup-yaml-file-path>
```

Load TPCC with 2 warehouses, then run bench with 4 threads:
```
$> ticat jt.tpcc wh=2 thread=4
```

Run TPCC with 4 threads, with pre-loaded data: (after running `jt.tpcc wh=2`)
```
$> ticat jt.tpcc.prepared wh=2 thread=4
```
The score will be put into a table in a local meta db, all scores will show when bench finish.

Load and run TPCC, compare versions `v5.1.0` and `nightly`, reset data on each run:
```
$> ticat jt.tpcc.v-t versions=v5.1.0,nightly threads=4
```

Load and run TPCC, compare `thread=4` and `=8`,
each thread runs two versions (`v5.1.0` and `nightly`), reset data on each run:
```
$> ticat jt.tpcc.t-v versions=v5.1.0,nightly threads=4,8
```

Load and run TPCC, compare versions `v5.1.0` and `nightly`,
each version runs two times (thread=4 and =8), reset data on each run:
```
$> ticat jt.tpcc.v-t versions=v5.1.0,nightly threads=4,8
```

Load and run TPCC, compare versions `v5.1.0` and `nightly`,
each version runs two times (thread=4 and =8), NOT reset data on the same version:
```
$> ticat jt.tpcc.v-xt versions=v5.1.0,nightly threads=4,8
```

Load and run TPCC, compare version `nightly` and a developing version:
based on `nightly` and replace tikv(or pd, tidb) with `local built binary`:
```
$> ticat jt.tpcc.t-v versions=nightly,nightly+./bin threads=4,8
```

Check out all score records
```
$> ticat jt.meta.record
```

Show usage of a command with `:=`:
```
$> ticat jt.tpcc.t-v :=
```

Show full info of a command with `:==`:
```
$> ticat jt.tpcc.t-v :==
```

All out-of-box commands are in the command path `jt`, use `:-` to show them all:
```
$> ticat jt :-
```

(TODO: support more workloads, the roadmap is here: [Architecture, roadmap and progress](./architecture-roadmap-progress.md))

## Advance usage
This repo is a package of many **ticat** components(modules),
for more detail usage please read [README of ticat](https://github.com/innerr/ticat/blob/main/README.md) and docs in ticat repo.
