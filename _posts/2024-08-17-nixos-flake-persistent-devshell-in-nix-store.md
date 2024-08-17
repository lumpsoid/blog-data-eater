---
title: "How to make flake devShells be persistent in nix store"
date: 2024-08-17
---

When working with flake based devShells, I was confused when tried to clear nix store with `nix store --gc` and afterwards found out, that all packages for the devShell I need to redownload again.

I wondered, is there a way to make them persistent?

Let's take this flake as an example:
```nix
{
  description = "Dev web extension";

  inputs = {
    nixpkgs.url = "github:nixos/nixpkgs?ref=nixos-unstable";
  };

  outputs = { self, nixpkgs }: 
  let
    system = "x86_64-linux";
    pkgs = nixpkgs.legacyPackages.${system};
  in 
  {

    devShells.${system}.default = pkgs.mkShell {
      name = "web-ext-devshell";

      buildInputs = with pkgs; [
        web-ext
      ];
    };

  };
}
```

Let's create our test directory and the flake:
```sh
mkdir testFlakeDevShell
cd testFlakeDevShell
vim flake.nix
# paste flake content
# exit vim
nix flake develop
exit # exit from the devShell
```

now, to give nixos garbage collector understand that we need this environment to persist in nix store, we need to create a symlinks to store paths of our source path and devShell derivations.

to find out what they are we need to find all paths with the name of our devShell - `web-ext-devshell`
```
ls /nix/store | grep web-ext-devshell
```

there would be 3 paths:
1. one for <name>.drv
2. <name>-env.drv
3. <name>-env

and also, it would be nice to make a link for gcroot to the flake source. It's not that important in terms of how much time you will wait for all dependencies to be downloaded if you wouldn't link paths to the devShell derivations, but why don't we make it complete?

to find out our flake source path, execute this command in the flake root:
```
# inside testFlakeDevShell directory
nix flake metadata
```

we will see the overview of our flake:
```
Resolved URL:  ...
Locked URL:    ...
Description:   Dev web extension
Path:          /nix/store/mz61kq27wb6hww7pfg4jf1wr62jzl5c3-source # what we are looking for
Revision:      ab532acd76ea5ba5ba2d6ebaa8e7b8d5e35bba71
Revisions:     21
Last modified: 2024-08-15 13:00:18
Inputs:
└───nixpkgs: github:nixos/nixpkgs/a58bc8ad779655e790115244571758e8de055e3d
```

important part for us is:
`Path:          /nix/store/mz61kq27wb6hww7pfg4jf1wr62jzl5c3-source`


We need to make links for all of them, to tell the gc that we need to keep them in the store.

```
# example with the flake source path
sudo ln -s /nix/store/mz61kq27wb6hww7pfg4jf1wr62jzl5c3-source /nix/var/nix/gcroots/web-ext-devshell-source
```

you can choose any name you want for the `web-ext-devshell` part, you also can create a directory inside `/nix/var/nix/gcroots/` and place links there.

```
# change first part for your actual paths
sudo ln -s <name>.drv /nix/var/nix/gcroots/web-ext-devshell-drv
sudo ln -s <name>-env /nix/var/nix/gcroots/web-ext-devshell-env
sudo ln -s <name>-env.drv /nix/var/nix/gcroots/web-ext-devshell-env-drv
```

after that you can freely use `nix-store --gc` and these devShell packages won't be deleted.


in order to undo our changes, you will need to `rm` these links in `/nix/var/nix/gcroots/`:
```
sudo rm /nix/var/nix/gcroots/web-ext-devshell-drv
sudo rm /nix/var/nix/gcroots/web-ext-devshell-env
sudo rm /nix/var/nix/gcroots/web-ext-devshell-env-drv
sudo rm /nix/var/nix/gcroots/web-ext-devshell-source
```
