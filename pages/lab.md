---
layout: page
title: Lab
permalink: /lab/
---

<h1>Welcome to the Lab / Bienvenido al Lab</h1>

This is the collaborative space where your ideas take shape. Throughout the semester, you’ll use this Lab to share reflections, digital experiments, and creative responses to our readings and digital activities. Remember, you’re not just completing assignments, you’re building a portfolio of work that reflects your journey. :) 

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

