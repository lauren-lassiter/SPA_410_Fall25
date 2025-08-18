---
layout: page
title: Lab
permalink: /lab/
---

<h1>Welcome to the Lab / Bienvenido al Lab</h1>

[EN]
This is the collaborative space where your ideas take shape. Throughout the semester, you’ll use this Lab to share reflections, digital experiments, and creative responses to our readings and digital activities. Remember, you’re not just completing assignments, you’re building a portfolio of work that reflects your learning journey. :)

[SPA]
Este es el espacio colaborativo donde tus ideas toman forma. A lo largo del semestre, usarás este Lab para compartir reflexiones, experimentos digitales y respuestas creativas a nuestras lecturas y actividades digitales. Recuerda que no solo estás completando tareas, estás construyendo un portafolio que refleja tu recorrido de aprendizaje. :)

{% assign posts_by_exercise = site.posts | group_by: "exercise" %}
{% for group in posts_by_exercise %}
  <h2 id="{{ group.name | slugify }}">{{ group.name }}</h2>
  <ul>
    {% for post in group.items %}
      <li>
        {{ post.author}}, <a href="{{ post.url | relative_url }}" title="{{ post.title }}">{{ post.title }}</a> — {{ post.date | date: "%B %d, %Y" }}
      </li>
    {% endfor %}
  </ul>
{% endfor %}

