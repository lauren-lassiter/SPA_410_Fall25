---
layout: page
title: Lab
permalink: /lab/
---

<h1>Blog Posts by Exercise</h1>

{% assign posts_by_exercise = site.posts | group_by: "exercise" %}
{% for group in posts_by_exercise %}
  <h2 id="{{ group.name | slugify }}">{{ group.name }}</h2>
  <ul>
    {% for post in group.items %}
      <li>
        {{ post.author}}, <a href="{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a> — {{ post.date | date: "%B %d, %Y" }}
      </li>
    {% endfor %}
  </ul>
{% endfor %}

