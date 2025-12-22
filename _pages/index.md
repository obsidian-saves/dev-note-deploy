---
layout: page
title: Home
permalink: /
---

<div class="home-container">

  <h1>🌱 Digital Garden</h1>
  <p class="subtitle">공부하며 자라고 있는 개발 노트들</p>

  {% assign grouped_notes = site.notes
    | group_by_exp: "note", "note.path | split: '/' | slice: 2,1 | first" %}

  {% for group in grouped_notes %}
    <section class="category">
      <h2>📁 {{ group.name }}</h2>

      <ul class="note-list">
        {% for note in group.items %}
          {% if note.published %}
            <li>
              <a class="internal-link" href="{{ site.baseurl }}{{ note.url }}">
                {{ note.title }}
              </a>
            </li>
          {% endif %}
        {% endfor %}
      </ul>
    </section>
  {% endfor %}

</div>
