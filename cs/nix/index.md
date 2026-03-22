# Nix

## Build configuration without applying
```sh
sudo nixos-rebuild build 
```


## Find out why package is installed:
```sh
nix path-info -r /run/current-system --extra-experimental-features nix-command  | grep amd  


nix why-depends /run/current-system /nix/store/p9hi51xnn7gfhxm7d5r433k4aab6m7lg-xf86-video-amdgpu-23.0.0 --extra-experimental-features nix-command 
/nix/store/0d84ynz3m8gy4ash8r33scpzzx3jxafq-nixos-system-nixos-25.11.6561.1267bb4920d0
└───/nix/store/2fiaizj43xs04ghw35cch23cg4p73fp9-etc
    └───/nix/store/pzrzgqi6dqm1j5m99xnlbn6b6dhs6173-config.ini
        └───/nix/store/m21ci7x644xvdw2mvb0y895dvz9fhq4z-xserver-wrapper
            └───/nix/store/mxk36ns8g2pkm7q4df8v1gqvv031ig5v-xserver.conf
                └───/nix/store/p9hi51xnn7gfhxm7d5r433k4aab6m7lg-xf86-video-amdgpu-23.0.0
```
## List packages

```sh
nix-store --query --requisites /run/current-system 
```

## Find in store:
```sh
find /nix/store -name "vmlinux*"

```

## Update & upgrade
```sh
sudo nix-channel --update
sudo nixos-rebuild switch --upgrade
```


## List available options. Searching available kernels


```sh
nix repl

:l <nixpkgs>
Added 12607 variables.

nix-repl> pkgs.linuxPackages.kernels
....
linux_6_18 ...
linux_6_19 ...
...  
# find dev version:
nix-repl> pkgs.linuxKernel.kernels.linux_6_19
# check deriviation file for details
```

## Read derivation file
```sh
nix derivation show /nix/store/sbz2mq79a9blpqd2zi644rr14gcbd441-linux-6.19.7.drv --extra-experimental-features nix-command 
```


## Install dev-kernel package:
in the `configuration.nix`
```nix
# pin the kernel, it will pin kernel but will confirm kernel version
boot.kernelPackages = pkgs.linuxKernel.packages.linux_6_19;
....

# in systemPackages
environment.systemPackages = with pkgs; [               
    pkgs.linuxKernel.kernels.linux_6_19.dev    
];

```