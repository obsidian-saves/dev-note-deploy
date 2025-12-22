---
layout: page
title: Home
id: home
permalink: /
---

<div class="home-container">

  <header class="home-header">
    <h1>📓 옵시디언 노트</h1>
    <p class="subtitle">
      생각을 기록하고, 연결하고, 키워가는 나만의 디지털 가든
    </p>
  </header>

  <section class="recent-section">
    <h2>🕒 최근 노트</h2>

    <ul class="recent-list">
      {% assign recent_notes = site.notes | sort: "last_modified_at_timestamp" | reverse %}
      {% for note in recent_notes limit: 5 %}
        <li class="recent-item">
          <span class="date">
            {{ note.last_modified_at | date: "%Y-%m-%d" }}
          </span>
          <a class="internal-link title" href="{{ site.baseurl }}{{ note.url }}">
            {{ note.title }}
          </a>
        </li>
      {% endfor %}
    </ul>

  </section>

</div>

<style>
/* 전체 컨테이너 */
.home-container {
  max-width: 46em;
  margin: 0 auto;
  padding: 2.5rem 1.5rem;
}

/* 헤더 */
.home-header {
  text-align: center;
  margin-bottom: 3rem;
}

.home-header h1 {
  font-size: 2.2rem;
  margin-bottom: 0.5rem;
}

.subtitle {
  font-size: 0.95rem;
  color: #777;
}

/* 최근 노트 섹션 */
.recent-section h2 {
  font-size: 1.3rem;
  margin-bottom: 1rem;
}

/* 리스트 */
.recent-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.recent-item {
  display: flex;
  gap: 0.75rem;
  align-items: baseline;
  padding: 0.75rem 0.5rem;
  border-radius: 8px;
  transition: background 0.2s ease;
}

.recent-item:hover {
  background: rgba(0, 0, 0, 0.04);
}

/* 날짜 */
.recent-item .date {
  font-size: 0.8rem;
  color: #999;
  min-width: 90px;
}

/* 제목 */
.recent-item .title {
  font-size: 0.95rem;
  text-decoration: none;
}

.recent-item .title:hover {
  text-decoration: underline;
}
</style>
