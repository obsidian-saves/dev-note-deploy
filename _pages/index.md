---
layout: page
title: Home
id: home
permalink: /
---

<div align="center">
<h1>옵시디언 노트</h1>

<strong>최근 노트</strong>

<ul>
  {% assign recent_notes = site.notes | sort: "last_modified_at_timestamp" | reverse %}
  {% for note in recent_notes limit: 5 %}
    <li>
      {{ note.last_modified_at | date: "%Y-%m-%d" }} — <a class="internal-link" href="{{ site.baseurl }}{{ note.url }}">{{ note.title }}</a>
    </li>
  {% endfor %}
</ul>
  </div>

<style>
  .wrapper {
    max-width: 46em;
  }
</style>
