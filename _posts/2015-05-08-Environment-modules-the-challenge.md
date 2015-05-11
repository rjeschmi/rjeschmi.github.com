---
layout: post
title: "Environment Modules: The Challenge"
description: ""
category: 
tags: []
---

# Modules on the Cluster

I have noticed some problems with the experimental modules on the new cluster, but I think it is worthwhile to explain how they are loaded so you can better control your environment.


So first, what is the module command (or ml command, the shorthand version)? In short it isn't a comand, it is a shell function. Let me explain how that works...

## The module shell function

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

Now imagine I want to use a different version of Python, how can I easily set the environment to handle that?

If we try to do it with a script, it will work in the scope of the script, but whent he script exits, we are back to our original environment...

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


