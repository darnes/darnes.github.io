# perf

## Record 
```sh
perf record -g ./client -p <pid>
```

## Report
```sh
perf report --vmlinux /nix/store/...-linux-6.19.7-dev/vmlinux
```