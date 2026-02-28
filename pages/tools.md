---
layout: default
title: tools
permalink: /tools/
---

<div class="main">
  <section class="content">

    <div class="section-head">
      <span class="label">ls -lt ./tools/</span>
      <span class="line"></span>
      <span class="count">{{ site.tools | size }} tools :: github.com/martabyte</span>
    </div>

    {% for tool in site.tools %}
    <div class="tool-entry{% if tool.status == 'in_progress' %} wip{% endif %}">
      <div class="tool-top">
        <div class="tool-index">{{ forloop.index | prepend: '0' | slice: -2, 2 }}</div>
        <div class="tool-header">
          <div class="tool-name-row">
            <a class="tool-name" href="{{ tool.url }}" target="_blank" rel="noopener">{{ tool.name }}</a>
            {% if tool.status == 'in_progress' %}
            <span class="tool-status wip">in_progress</span>
            {% else %}
            <span class="tool-status released">released</span>
            {% endif %}
          </div>
          <div class="tool-meta">
            <span class="tool-lang">{{ tool.lang }}</span>
            {% for tag in tool.tags %}
            <span class="pe-tag">{{ tag }}</span>
            {% endfor %}
          </div>
        </div>
        {% unless tool.status == 'in_progress' %}
        <a class="tool-clone" href="{{ tool.url }}" target="_blank" rel="noopener">
          $ git clone →
        </a>
        {% endunless %}
      </div>
      <div class="tool-desc">{{ tool.desc }}</div>
    </div>
    {% endfor %}

  </section>

  <aside class="sidebar">
    <div class="panel">
      <div class="panel-title"><span>$ stat tools</span><span class="pt-icon">▮</span></div>
      <div class="panel-body">
        <div class="stat-line"><span class="stat-k">total</span><span class="stat-v">{{ site.tools | size }}</span></div>
        {% assign langs = site.tools | map: 'lang' | uniq | sort %}
        {% for lang in langs %}
        {% assign count = site.tools | where: 'lang', lang | size %}
        <div class="stat-line"><span class="stat-k">{{ lang }}</span><span class="stat-v">{{ count }}</span></div>
        {% endfor %}
      </div>
    </div>

    <div class="panel">
      <div class="panel-title"><span>$ ls ./tags/</span><span class="pt-icon">▮</span></div>
      <div class="panel-body">
        <div class="tag-grid">
          {% assign tool_tags = site.tools | map: 'tags' | join: ',' | split: ',' | uniq | sort %}
          {% for tag in tool_tags %}
          <span class="tag-item">{{ tag }}</span>
          {% endfor %}
        </div>
      </div>
    </div>
  </aside>
</div>
