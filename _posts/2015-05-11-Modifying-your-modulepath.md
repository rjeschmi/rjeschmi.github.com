---
layout: post
title: "Environment Modules: managing your modulepath"
description: "How to change your environment in several easy steps"
category: sysadmin
tags: []
---


# MODULEPATH and how to change your module perspective

I [previously examined]({% post_url 2015-05-08-Environment-modules-the-challenge %})  in some detail exactly what the command `module` does for us. I typically use [lmod](https://www.tacc.utexas.edu/research-development/tacc-projects/lmod), but the basic features work with both.

One powerful aspect of modules is that the configuration is managed by a small number of environment variables, which means you can reconfigure module with module itself.

## The MODULEPATH

In the same way that PATH is a searchpath for binaries you run on the command-line, MODULEPATH is the searchpath for modulefiles.

In my standard install I configure a bunch of reasonable defaults using a modulefile as described in [lmod docs](https://www.tacc.utexas.edu/research-development/tacc-projects/lmod/system-administrators-guide/providing-a-standard-set-of-modules).

```sh

[rob@crick ohri]$ cat /software/packages/lmod/lmod/modulefiles/Core/StdEnv.lua

prepend_path("MODULEPATH", "/software/packages/EasyBuild/modules/all")
prepend_path("MODULEPATH", "/software/experimental/modules/all")
prepend_path("MODULEPATH", "/software/modulefiles")
setenv("EASYBUILD_CONFIGFILES", "/software/packages/EasyBuild/etc/config.cfg")

```




