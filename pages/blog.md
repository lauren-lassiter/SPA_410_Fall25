---
layout: page
title: Blog
---

<h1>Course Blog</h1>

<ul>
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> » {{ post.author}}, 
      <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>