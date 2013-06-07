---
layout: page
title: Hello World!
tagline: Supporting tagline
---

Hi
==

I'm Robert Schmidt, a Linux Systems Administrator by day and distracted by night. My day job is with [Canadiana](http://canadiana.ca) where I dream up new ways to do things cheaper but not always simpler.

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
