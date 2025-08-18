---
layout: page
title: Blog
permalink: /blog/
---

<h1>Course Blog</h1>

<p>This blog showcases posts written by students, organized by topic or assignment.</p>

{% assign grouped_posts = site.posts | group_by: "exercise" %}
{% for group in grouped_posts %}
  <h2>{{ group.name }}</h2>
  <ul>
    {% for post in group.items %}
      <li>
        <a href="{{ post.baseurl }}">{{ post.title }}</a> – {{ post.date | date: "%B %d, %Y" }}
      </li>
    {% endfor %}
  </ul>
{% endfor %}



==========================

{% if page.subject %}
<p><strong>Subject:</strong> {{ page.subject }}</p>
{% endif %}


<ul>
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> » {{ post.author}}, 
      <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>