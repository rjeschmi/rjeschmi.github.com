---
layout: post
title: "Environment Modules: The Challenge"
description: ""
category: 
tags: []
---

I have noticed some problems with the experimental modules on the new cluster, but I think it is worthwhile to explain how they are loaded so you can better control your environment.


So first, what is the module command (or ml command, the shorthand version)?

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

so in my particular environment the command "ruby" or "python" 
