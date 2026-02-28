---
layout: default
title: whoami
permalink: /whoami/
---

<!--<div class="wrapper whoami-wrapper">-->
<div class="main">
  <div class="page-header" style="grid-column: 1 / -1">
    <!--<div class="page-title">whoami</div>-->
    <div class="page-subtitle mono">
      afr0dita@m0unt-0lympus:~/blog# cat ./whoami.md<span class="cursor"></span>
    </div>
  </div>

  <main>
    <!-- BIO -->
    <div class="section">
      <div class="section-head">whoami</div>
      <div class="bio-block">
        <div class="bio-header">stdout — user.profile</div>
        <div class="bio-body">
          <div class="bio-line"><span class="bio-k">alias&nbsp;</span><span class="bio-v">{{ site.author.alias }}</span></div>
          <div class="bio-line"><span class="bio-k">focus&nbsp;</span><span class="bio-v">{{ site.author.focus }}</span></div>
          <div class="bio-line"><span class="bio-k">deployed_at&nbsp;</span><span class="bio-v">{{ site.author.deployed_at }}</span></div>
          <div class="bio-sep"></div>
          <div class="bio-line">
            <span class="bio-k">last_commit&nbsp;</span>
            <span class="bio-v">{{ site.posts.first.title }}</span>
          </div>
          <div class="bio-line"><span class="bio-k">0days&nbsp;</span><span class="bio-v fun">0&nbsp;<span style="color:var(--g2);font-size:10px">// not yet ;)</span></span></div>
          <div class="bio-line"><span class="bio-k">coffee_dependency&nbsp;</span><span class="bio-v warn">{{ site.author.coffee }}</span></div>
          <div class="bio-line"><span class="bio-k">soundtrack&nbsp;</span><span class="bio-v">{{ site.author.soundtrack }}</span></div>
          <div class="bio-sep"></div>
          <div class="bio-line"><span class="bio-k"></span><span class="bio-v" style="color:var(--g2)">red team operator. soy concha, entro.</span></div>
        </div>
      </div>
    </div>

    <!-- CERTS -->
    <div class="section">
      <div class="section-head">certs</div>
      <div class="cert-list">
        {% for cert in site.author.certs %}
        <div class="cert-item{% if cert.wip %} wip{% endif %}">
          <div class="cert-badge">{{ cert.badge }}</div>
          <div class="cert-info">
            <div class="cert-name">{{ cert.name }}</div>
            <div class="cert-issuer">{{ cert.issuer }}</div>
          </div>
          <div class="cert-year">{{ cert.year }}</div>
        </div>
        {% endfor %}
      </div>
    </div>

    <!-- CONTACT -->
    <div class="section">
      <div class="section-head">contact</div>
      <div class="contact-list">
        {% for link in site.author.contact %}
        <a class="contact-item" href="{{ link.url }}">
          <span class="ci-key">{{ link.key }}</span>
          <span class="ci-val">{{ link.val }}</span>
          <span class="ci-arrow">→</span>
        </a>
        {% endfor %}
        <!--<div class="pgp-block">
          <div class="pgp-header">$ gpg --fingerprint {{ site.author.pgp }}</div>
          <div class="pgp-body">
            pub &nbsp; rsa4096 2022-11-04 [SC]<br>
            &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span class="pgp-key">{{ site.author.pgp_fingerprint }}</span><br>
            uid &nbsp; [ultimate] {{ site.author.alias }} &lt;{{ site.author.contact[3].url | remove: 'mailto:' }}&gt;
          </div>
        </div>-->
      </div>
    </div>
  </main>

  <!-- HTB SIDEBAR -->
  <!--<aside class="sidebar">
    <div class="panel">
      <div class="panel-title"><span>$ htb --stats</span><span class="pt-icon">▮</span></div>
      <div class="panel-body">
        <div class="htb-mini-grid">
          <div class="htb-mini-stat">
            <span class="sn">{{ site.author.htb_pwned }}</span>
            <span class="sl">pwned</span>
          </div>
          <div class="htb-mini-stat">
            <span class="sn">{{ site.author.htb_challenges }}</span>
            <span class="sl">challenges</span>
          </div>
          <div class="htb-mini-stat">
            <span class="sn">{{ site.author.htb_ubloods }}</span>
            <span class="sl">u_bloods</span>
          </div>
          <div class="htb-mini-stat">
            <span class="sn">{{ site.author.htb_rbloods }}</span>
            <span class="sl">r_bloods</span>
          </div>
        </div>
        <div class="htb-row"><span class="hk">rank</span><span class="hv rank">{{ site.author.htb_rank }}</span></div>
        <div class="htb-row"><span class="hk">global</span><span class="hv">{{ site.author.htb_top }}</span></div>
        <div class="htb-row"><span class="hk">points</span><span class="hv">{{ site.author.htb_points }}</span></div>
        <div class="htb-row"><span class="hk">best_cat</span><span class="hv">{{ site.author.htb_best }}</span></div>
        <div class="htb-row"><span class="hk">since</span><span class="hv">{{ site.author.htb_since }}</span></div>
      </div>
    </div>
  </aside>-->

</div>
