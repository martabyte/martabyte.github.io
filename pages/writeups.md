---
layout: default
title: writeups
permalink: /writeups/
---

<div class="main">
  <section class="content">

    <div class="section-head">
      <span class="label">ls -lt ./writeups/</span>
      <div class="lang-toggle">
        <span class="lt-label">lang:</span>
        <!--<button class="lang-btn active" onclick="filterLang('all')">all</button>-->
        <button class="lang-btn" onclick="filterLang('es')">es</button>
        <button class="lang-btn" onclick="filterLang('en')">en</button>
      </div>
      <span class="line"></span>
      <span class="count" id="post-count">
        {{ site.posts | where_exp: "p", "p.tags contains 'writeup'" | size }} entries :: tag:writeup
      </span>
    </div>

    {% assign writeups = site.posts | where_exp: "p", "p.tags contains 'writeup'" %}
    {% for post in writeups %}
    <div class="post-entry" data-lang="{{ post.lang | default: 'es' }}">
      <div class="pe-top">
        <div class="pe-index">{{ forloop.index | prepend: '0' | slice: -2, 2 }}</div>
        <div class="pe-header">
          <a class="pe-title" href="{{ post.url | relative_url }}">{{ post.title }}</a>
          <div class="pe-meta">
            <span class="pe-date">{{ post.date | date: "%Y-%m-%d %H:%M" }}</span>
            <span class="pe-tag lang-tag">{{ post.lang | default: 'es' }}</span>
            {% for tag in post.tags %}
            <span class="pe-tag">{{ tag }}</span>
            {% endfor %}
          </div>
        </div>
      </div>
      <div class="pe-body">{{ post.excerpt | strip_html | truncatewords: 30 }}</div>
      <div class="pe-footer">
        <span>size: {{ post.content | strip_html | size | divided_by: 1000 }}K :: read_time: ~{{ post.content | strip_html | number_of_words | divided_by: 200 | plus: 1 }}min</span>
        <a class="pe-read" href="{{ post.url | relative_url }}">{{ post.slug }}</a>
      </div>
    </div>
    {% else %}
    <div class="empty-state">
      <span class="es-prompt">$</span> find . -tag writeup — <span class="es-result">0 results</span>
    </div>
    {% endfor %}

  </section>

  <aside class="sidebar">

    <div class="panel">
      <div class="panel-title"><span>$ stat writeups</span><span class="pt-icon">▮</span></div>
      <div class="panel-body">
        {% assign writeups_all = site.posts | where_exp: "p", "p.tags contains 'writeup'" %}
        {% assign writeups_es  = writeups_all | where: "lang", "es" %}
        {% assign writeups_en  = writeups_all | where: "lang", "en" %}
        <div class="stat-line"><span class="stat-k">total</span><span class="stat-v">{{ writeups_all | size }}</span></div>
        <div class="stat-line"><span class="stat-k">lang:es</span><span class="stat-v">{{ writeups_es | size }}</span></div>
        <div class="stat-line"><span class="stat-k">lang:en</span><span class="stat-v">{{ writeups_en | size }}</span></div>
      </div>
    </div>

    <div class="panel">
      <div class="panel-title"><span>$ ls ./tags/</span><span class="pt-icon">▮</span></div>
      <div class="panel-body">
        <div class="tag-grid">
          {% assign all_tags = writeups_all | map: 'tags' | join: ',' | split: ',' | uniq | sort %}
          {% for tag in all_tags %}
          <a class="tag-item" href="/tags/{{ tag }}">{{ tag }}</a>
          {% endfor %}
        </div>
      </div>
    </div>

  </aside>
</div>

<script>
  function filterLang(lang) {
    document.querySelectorAll('.lang-btn').forEach(btn => {
      btn.classList.toggle('active', btn.textContent === lang);
    });
    const posts = document.querySelectorAll('.post-entry');
    let visible = 0;
    posts.forEach(post => {
      const match = lang === 'all' || post.dataset.lang === lang;
      post.classList.toggle('hidden', !match);
      if (match) visible++;
    });
    const count = document.getElementById('post-count');
    if (count) {
      const suffix = lang === 'all' ? 'tag:writeup' : 'lang:' + lang;
      count.textContent = visible + ' entr' + (visible === 1 ? 'y' : 'ies') + ' :: ' + suffix;
    }
  }
  filterLang('es');
</script>
