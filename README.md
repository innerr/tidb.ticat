# tidb.ticat
This project meant to build a unified TiDB system include all related utilities and tools, without extra costs.

From this all non-production labor works of TiDB will be greatly reduced,
hence we could efficiently build a stronger TiDB, and make a better community.

[Why we need this and how it can help](./why-we-need-this-and-how-it-can-help.md)

[Architecture, roadmap and progress](./architecture-roadmap-progress.md)

## Usage

### Build (or download) ticat
`golang` is needed:
```
$> git clone https://github.com/innerr/ticat
$> cd ticat
$> make
```
Highly recommend to set `ticat/bin` to system `$PATH`, it's handy.

### Add this repo to ticat:
```
$> ticat hub.add innerr/tidb.ticat
```

### Typical usage examples
(TODO)

### Advance usage
This repo is a package of many **ticat** components(modules),
for detail usage please read [README of ticat](https://github.com/innerr/ticat/blob/main/README.md).
