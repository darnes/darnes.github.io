# perf

## Record 
```sh
perf record -g ./client -p <pid>
# start with perf, sampling cpu frequency to 999 (~1000Hz)
perf record -F 999 -g ./client  
```

## Report
```sh
perf report --vmlinux /nix/store/...-linux-6.19.7-dev/vmlinux
# press `+` for tree or 
perf report --call-graph # ? 

```
## Stat
```sh
perf stat -e cycles,instructions,cache-misses ./client  
# man per-stat, perf list for list of events
```
## Top
```sh
perf top -p <pid>
```

## Flamegraph visualization
```sh
git clone https://github.com/brendangregg/FlameGraph.git --depth=1
# after 
perf record -F 999 -g ./client
# for nix os only
# sudo ln -s  /run/current-system/sw/bin/perl  /usr/bin/perl
perf script | FlameGraph/stackcollapse-perf.pl | FlameGraph/flamegraph.pl > flame.svg
```


## Build with debug symbols:
### Go
```sh
go build -gcflags="all=-N -l" client.go  
```

## pidof 
```sh
# very handy
pidof ./client
# so can use:
perf top -p $(pidof ./client )
```