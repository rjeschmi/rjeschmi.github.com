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

So we can see here a few MODULEPATHs are prepended to our path.

There is an easy commandline option to do the same thing: `module use`

# module use to prepend new paths


say for some instance you clear all your MODULEPATH out (perhaps you don't have the StdEnv module loaded). In this case you will only see the modules available in the default Core path.

```sh

[rob@crick ohri]$ ml list
No modules loaded

[rob@crick ohri]$ ml av

---------------------------------------------------------------------------------- /software/packages/lmod/lmod/modulefiles/Core ----------------------------------------------------------------------------------
   DevEnv           EasyBuild/git-devel    EasyBuild/prod-testing        OneSIS/2.0.3    Perl/5.18.2             Perl/5.20.1 (D)    StdEnv       lmod/5.8rc2          settarg/5.8rc2
   EasyBuild/dev    EasyBuild/git          EasyBuild/prod         (D)    Perl/plenv      Perl/5.20.1@anyevent    R/3.1.1     (D)    gcc/4.9.1    lsscsi/0.28b4r120

  Where:
   (D):  Default Module

Use "module spider" to find all possible modules.
Use "module keyword key1 key2 ..." to search for all possible modules matching any of the "keys".

[rob@crick ohri]$


```


You can then re-add a module path, like the experimental one above using `module use`

```sh

[rob@crick ohri]$ module use /software/experimental/modules/all

[rob@crick ohri]$ echo $MODULEPATH
/software/experimental/modules/all:/software/packages/modulefiles/Linux:/software/packages/modulefiles/Core:/software/packages/lmod/lmod/modulefiles/Core

[rob@crick ohri]$ ml av
Rebuilding cache, please wait ... (not written to file) done

--------------------------------------------------------------------------------------- /software/experimental/modules/all ----------------------------------------------------------------------------------------
   Autoconf/2.69-GCC-4.9.2                     Java/1.7.0_80                                 SAMtools/0.1.19-foss-2015a                                      libreadline/6.3-foss-2015a
   Automake/1.15-GCC-4.9.2                     Java/1.8.0_25                          (D)    SAMtools/1.2-foss-2015a-HTSlib-1.2.1-intel                      libtool/2.4.2-GCC-4.9.2
   Boost/1.55.0-foss-2015a-Python-2.7.9        M4/1.4.17-GCC-4.9.2                           SAMtools/1.2-foss-2015a-HTSlib-1.2.1                     (D)    libxml2/2.9.2-foss-2015a
   CMake/2.8.12-foss-2015a                     NASM/2.11.06-foss-2015a                       ScaLAPACK/2.0.2-gompi-2015a-OpenBLAS-0.2.13-LAPACK-3.5.0        libxslt/1.1.28-foss-2015a
   CMake/3.1.0-GCC-4.9.2                       OpenBLAS/0.2.13-GCC-4.9.2-LAPACK-3.5.0        bzip2/1.0.6-foss-2015a                                          ncurses/5.9-foss-2015a
   CMake/3.1.3-foss-2015a               (D)    OpenMPI/1.8.4-GCC-4.9.2                       cURL/7.40.0-GCC-4.9.2                                           ncurses/5.9-GCC-4.9.2       (D)
   Cufflinks/2.2.1-foss-2015a                  PeakRanger/1.18-foss-2015a                    expat/2.1.0-GCC-4.9.2                                           numactl/2.0.10-GCC-4.9.2
   Cufflinks/2.2.1-goolf-1.4.10         (D)    Python/2.7.9-foss-2015a                       foss/2015a                                                      zlib/1.2.7-foss-2014b
   Eigen/3.2.3-foss-2015a                      R/3.1.2-foss-2015a                            gettext/0.19.4-GCC-4.9.2                                        zlib/1.2.8-foss-2014b
   FFTW/3.3.4-gompi-2015a                      R/3.2.0-foss-2015a-bare                (D)    gompi/2015a                                                     zlib/1.2.8-foss-2015a
   GCC/4.9.2                                   RStan/2.6.0-foss-2015a-R-3.2.0                hwloc/1.10.0-GCC-4.9.2                                          zlib/1.2.8-GCC-4.9.2
   GMP/5.1.3-foss-2015a                        Ruby/2.1.5-foss-2015a                         libjpeg-turbo/1.4.0-foss-2015a                                  zlib/1.2.8-goolf-1.4.10
   HISAT/0.1.6-beta-foss-2015a                 Ruby-FPM/2.1.5-1.0.0-foss-2015a               libpng/1.6.16-foss-2015a                                        zlib/1.2.8-intel-foss-2015a (D)
   HTSlib/1.2.1-foss-2015a                     SAMtools/0.1.18-foss-2015a                    libpng/1.6.17-foss-2015a                                 (D)

---------------------------------------------------------------------------------- /software/packages/lmod/lmod/modulefiles/Core ----------------------------------------------------------------------------------
   DevEnv           EasyBuild/git-devel    EasyBuild/prod-testing        OneSIS/2.0.3    Perl/5.18.2             Perl/5.20.1 (D)    StdEnv       lmod/5.8rc2          settarg/5.8rc2
   EasyBuild/dev    EasyBuild/git          EasyBuild/prod         (D)    Perl/plenv      Perl/5.20.1@anyevent    R/3.1.1            gcc/4.9.1    lsscsi/0.28b4r120

  Where:
   (D):  Default Module

Use "module spider" to find all possible modules.
Use "module keyword key1 key2 ..." to search for all possible modules matching any of the "keys".


```

This is a powerful tool, and allows you to write your own modulefiles and store them in your home directory. To add it to your path you would just `module use` the directory.

This can also be useful to hide directories of modules that you really aren't interested in. As we develop a longer list of software I will look at these options to reduce the overwhelming selection of software available



