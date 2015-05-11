---
layout: post
title: "Environment Modules: The Challenge"
description: "How to change your environment in several easy steps"
category: sysadmin
tags: []
---

# Modules on the Cluster

I started writing this and got a bit off track. I try to explain the difference between changing the environment in a sub-shell vs the top level shell. 

I have noticed some problems with the experimental modules on the new cluster, but I think it is worthwhile to explain how they work, so you can better control your environment.


First, what is the `module` command (or ml command, the shorthand version)? In short it isn't a command at all, it is a shell function. Let me explain how that works...

## Environment variables

First it might help to explain the challenge of setting an environment variable.

Most people have come across the environment variable PATH. In a bourne-like shell you can run 'echo $PATH' and see your path.

{% highlight bash %}

[rob@crick ~]$ echo $PATH
/home/rob/.pyenv/shims:/home/rob/.rbenv/shims:/home/rob/.rbenv/bin:/home/rob/.pyenv/bin:
/home/rob/bin:/gridware/sge/bin/lx26-amd64:/home/rob/.pyenv/shims:/home/rob/.rbenv/shims:
/home/rob/.rbenv/bin:/home/rob/.pyenv/bin:/home/rob/bin:/gridware/sge/bin/lx26-amd64:
/usr/local/bin:/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/sbin

{% endhighlight %}
These directories are the search path for commands you run.

so in my particular environment the command "ruby" or "python" will run from the respective shim directory:

{% highlight bash %}

[rob@crick peakranger]$ which ruby
~/.rbenv/shims/ruby

[rob@crick nfs_mount_issue]$ which python
~/.pyenv/shims/python

{% endhighlight %}

## Script vs Source


Now imagine I want to use a different version of Python, how can I easily set the environment to handle that?

If we try to do it with a script, it will work in the scope of the script, but when the script exits, we are back to our original environment...

{% highlight bash %}

[rob@crick bin]$ cat set_path.sh
#!/bin/bash
PATH=/software/experimental/software/Python/2.7.9-foss-2015a/bin:$PATH
echo $PATH

[rob@crick bin]$ ./set_path.sh
/software/experimental/software/Python/2.7.9-foss-2015a/bin:/home/rob/.pyenv/shims:/home/rob/.rbenv/shims:/home/rob/.rbenv/bin:/home/rob/.pyenv/bin:/home/rob/bin:/gridware/sge/bin/lx26-amd64:/home/rob/.pyenv
/shims:/home/rob/.rbenv/shims:/home/rob/.rbenv/bin:/home/rob/.pyenv/bin:/home/rob/bin:/gridware/sge/bin/lx26-amd64:/usr/local/bin:/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/sbin
[rob@crick bin]$ echo $PATH
/home/rob/.pyenv/shims:/home/rob/.rbenv/shims:/home/rob/.rbenv/bin:/home/rob/.pyenv/bin:/home/rob/bin:/gridware/sge/bin/lx26-amd64:/home/rob/.pyenv/shims:/home/rob/.rbenv/shims:/home/rob/.rbenv/bin:/home/rob
/.pyenv/bin:/home/rob/bin:/gridware/sge/bin/lx26-amd64:/usr/local/bin:/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/sbin

{% endhighlight %} 

If instead we "source" the script using the source command or its short form '.' we can see that the commands are change in our main shell

{% highlight bash %}

[rob@crick bin]$ source set_path.sh
/software/experimental/software/Python/2.7.9-foss-2015a/bin:/home/rob/.pyenv/shims:/home/rob/.rbenv/shims:/home/rob/.rbenv/bin:/home/rob/.pyenv/bin:/home/rob/bin:/gridware/sge/bin/lx26-amd64:/home/rob/.pyenv
/shims:/home/rob/.rbenv/shims:/home/rob/.rbenv/bin:/home/rob/.pyenv/bin:/home/rob/bin:/gridware/sge/bin/lx26-amd64:/usr/local/bin:/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/sbin
[rob@crick bin]$ echo $PATH
/software/experimental/software/Python/2.7.9-foss-2015a/bin:/home/rob/.pyenv/shims:/home/rob/.rbenv/shims:/home/rob/.rbenv/bin:/home/rob/.pyenv/bin:/home/rob/bin:/gridware/sge/bin/lx26-amd64:/home/rob/.pyenv
/shims:/home/rob/.rbenv/shims:/home/rob/.rbenv/bin:/home/rob/.pyenv/bin:/home/rob/bin:/gridware/sge/bin/lx26-amd64:/usr/local/bin:/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/sbin

{% endhighlight %}

## Module to the rescue?

Module and the modern replacement work similarly by defining a shell function to eval the output from the command, thus setting variables in the calling shell.

For example ModulesC sets:

```sh

[rjeschmi@dena ~]$ type module
module is a function
module ()
{
    eval `/cm/local/apps/environment-modules/3.2.10/Modules/$MODULE_VERSION/bin/modulecmd bash $*`
}
```

And lmod sets:

```sh

[rob@crick easybuild]$ type module
module is a function
module ()
{
    eval $($LMOD_CMD bash "$@");
    [ $? = 0 ] && eval $(${LMOD_SETTARG_CMD:-:} -s sh)
}

```

An example output from a "load" command to lmod looks like:

```sh


[rob@crick ohri]$ /software/packages/lmod/lmod/libexec/lmod bash load Perl
LMOD_DEFAULT_MODULEPATH="/software/packages/modulefiles/Linux:/software/packages/modulefiles/Core:/software/packages/lmod/lmod/modulefiles/Core";
export LMOD_DEFAULT_MODULEPATH;
MODULEPATH="/software/modulefiles:/software/experimental/modules/all:/software/packages/EasyBuild/modules/all:/software/packages/modulefiles/Linux:/software/packages/modulefiles/Core:/software/packages/lmod/lmod
/modulefiles/Core";
export MODULEPATH;
LMOD_DEFAULT_MODULEPATH="/software/packages/modulefiles/Linux:/software/packages/modulefiles/Core:/software/packages/lmod/lmod/modulefiles/Core";
export LMOD_DEFAULT_MODULEPATH;
LOADEDMODULES="StdEnv:Perl/5.20.1";
export LOADEDMODULES;
MODULEPATH="/software/modulefiles:/software/experimental/modules/all:/software/packages/EasyBuild/modules/all:/software/packages/modulefiles/Linux:/software/packages/modulefiles/Core:/software/packages/lmod/lmod
/modulefiles/Core";
export MODULEPATH;
PATH="/software/tools/packages/plenv/versions/5.20.1/bin:/home/rob/.pyenv/shims:/home/rob/.rbenv/shims:/home/rob/.rbenv/bin:/home/rob/.pyenv/bin:/home/rob/bin:/gridware/sge/bin/lx26-amd64:/home/rob/.pyenv/shims:
/home/rob/.rbenv/shims:/home/rob/.rbenv/bin:/home/rob/.pyenv/bin:/home/rob/bin:/gridware/sge/bin/lx26-amd64:/usr/local/bin:/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/sbin";
export PATH;
_LMFILES_="/software/packages/lmod/lmod/modulefiles/Core/StdEnv.lua:/software/packages/lmod/lmod/modulefiles/Core/Perl/5.20.1.lua";
export _LMFILES_;
_ModuleTable001_="X01vZHVsZVRhYmxlXz17WyJhY3RpdmVTaXplIl09MixiYXNlTXBhdGhBPXsiL3NvZnR3YXJlL3BhY2thZ2VzL21vZHVsZWZpbGVzL0xpbnV4IiwiL3NvZnR3YXJlL3BhY2thZ2VzL21vZHVsZWZpbGVzL0NvcmUiLCIvc29mdHdhcmUvcGFja2FnZXMvbG1vZ
C9sbW9kL21vZHVsZWZpbGVzL0NvcmUiLH0sWyJjX3JlYnVpbGRUaW1lIl09ODY0MDAsWyJjX3Nob3J0VGltZSJdPTYuMTAyNjczMDUzNzQxNSxmYW1pbHk9e30saW5hY3RpdmU9e30sbVQ9e1Blcmw9e1siRk4iXT0iL3NvZnR3YXJlL3BhY2thZ2VzL2xtb2QvbG1vZC9tb2R1bGVm
aWxlcy9Db3JlL1BlcmwvNS4yMC4xLmx1YSIsWyJkZWZhdWx0Il09MSxbImZ1bGxOYW1lIl09IlBlcmwvNS4yMC4xIixbImxvYWRPcmRlciJd";
export _ModuleTable001_;
_ModuleTable002_="PTIscHJvcFQ9e30sWyJzaG9ydCJdPSJQZXJsIixbInN0YXR1cyJdPSJhY3RpdmUiLH0sU3RkRW52PXtbIkZOIl09Ii9zb2Z0d2FyZS9wYWNrYWdlcy9sbW9kL2xtb2QvbW9kdWxlZmlsZXMvQ29yZS9TdGRFbnYubHVhIixbImRlZmF1bHQiXT0wLFsiZnVsb
E5hbWUiXT0iU3RkRW52IixbImxvYWRPcmRlciJdPTEscHJvcFQ9e30sWyJzaG9ydCJdPSJTdGRFbnYiLFsic3RhdHVzIl09ImFjdGl2ZSIsfSx9LG1wYXRoQT17Ii9zb2Z0d2FyZS9tb2R1bGVmaWxlcyIsIi9zb2Z0d2FyZS9leHBlcmltZW50YWwvbW9kdWxlcy9hbGwiLCIvc29m
dHdhcmUvcGFja2FnZXMvRWFzeUJ1aWxkL21vZHVsZXMvYWxsIiwiL3NvZnR3YXJlL3BhY2thZ2VzL21vZHVsZWZpbGVzL0xpbnV4IiwiL3Nv";
export _ModuleTable002_;
_ModuleTable003_="ZnR3YXJlL3BhY2thZ2VzL21vZHVsZWZpbGVzL0NvcmUiLCIvc29mdHdhcmUvcGFja2FnZXMvbG1vZC9sbW9kL21vZHVsZWZpbGVzL0NvcmUiLH0sWyJzeXN0ZW1CYXNlTVBBVEgiXT0iL3NvZnR3YXJlL3BhY2thZ2VzL21vZHVsZWZpbGVzL0xpbnV4Oi9zb
2Z0d2FyZS9wYWNrYWdlcy9tb2R1bGVmaWxlcy9Db3JlOi9zb2Z0d2FyZS9wYWNrYWdlcy9sbW9kL2xtb2QvbW9kdWxlZmlsZXMvQ29yZSIsWyJ2ZXJzaW9uIl09Mix9";
export _ModuleTable003_;
_ModuleTable_Sz_="3";
export _ModuleTable_Sz_;

```

