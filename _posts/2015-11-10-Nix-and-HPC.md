---
layout: post
title: "Nix and HPC Software"
description: "compiling code for 20 versions and 7 compilers"
category: sysadmin
tags: []
---


# Using nix for HPC and Scientific software builds.

`Nix` has received a lot of attention. It is an interesting concept and worth considering as a system to build an OS.

In the scientific software domain, we have somewhat different requirements and I want to highlight how you might use `nix` to build scientific software and limitations in its use.

`Nix` uses a language that is functional. This makes it very expressive and fairly easy to make changes that have significant impact on your software tree.

For example, to build boost in `nix` is pretty easy


```sh

[root@4a5ed720317e git]# nix-env -i boost
replacing old ‘boost-1.59.0’
installing ‘boost-1.59.0’
download-from-binary-cache.pl: could not download ‘http://bincache:8080/nix-cache-info’ (Curl error 6)
these paths will be fetched (7.88 MiB download, 112.49 MiB unpacked):
  /nix/store/8yg6znnpiwc8i59h6fyk0xyzr97vkgjl-boost-1.59.0
  /nix/store/v76ajdqd6db72s14clhpk94v0hk7aham-boost-1.59.0-dev
  /nix/store/xwnakwqfjx23vv9k349jpwscr3kbr3vw-boost-1.59.0-lib
fetching path ‘/nix/store/v76ajdqd6db72s14clhpk94v0hk7aham-boost-1.59.0-dev’...
could not download ‘http://bincache:8080/nix-cache-info’ (Curl error 6)

*** Downloading ‘https://cache.nixos.org/nar/0nkx8sj0qnvdcqqjdw0qwwdzvsar15b7xi4bjca7pjmiz00295iq.nar.xz’ to ‘/nix/store/v76ajdqd6db72s14clhpk94v0hk7aham-boost-1.59.0-dev’...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 5827k  100 5827k    0     0  3118k      0  0:00:01  0:00:01 --:--:-- 3121k

fetching path ‘/nix/store/xwnakwqfjx23vv9k349jpwscr3kbr3vw-boost-1.59.0-lib’...
could not download ‘http://bincache:8080/nix-cache-info’ (Curl error 6)

*** Downloading ‘https://cache.nixos.org/nar/1xqbpxxww0ly741rwqaqz16jk9027dx51836vpppzh5c45ykhypr.nar.xz’ to ‘/nix/store/xwnakwqfjx23vv9k349jpwscr3kbr3vw-boost-1.59.0-lib’...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 2237k  100 2237k    0     0  3703k      0 --:--:-- --:--:-- --:--:-- 3704k

fetching path ‘/nix/store/8yg6znnpiwc8i59h6fyk0xyzr97vkgjl-boost-1.59.0’...
could not download ‘http://bincache:8080/nix-cache-info’ (Curl error 6)

*** Downloading ‘https://cache.nixos.org/nar/1h9ilsvamdp65knp5gxvdrqxifxqb9nlrls7aj8dm31vnp2136lh.nar.xz’ to ‘/nix/store/8yg6znnpiwc8i59h6fyk0xyzr97vkgjl-boost-1.59.0’...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   288  100   288    0     0   1076      0 --:--:-- --:--:-- --:--:--  1082

building path(s) ‘/nix/store/00rgybcn2i0vpygkhxcq6h67612l5xnn-user-environment’

```
