---
layout: default
title: contact
permalink: /contact/
---

<div class="contact-page">
  <div class="contact-terminal">

    <div class="ct-header">
      <span class="ct-dot" style="background:var(--red)"></span>
      <span class="ct-dot" style="background:var(--g2)"></span>
      <span class="ct-dot" style="background:var(--cyan)"></span>
      <span class="ct-title">afr0dita@m0unt-0lympus:~/contact</span>
    </div>

    <div class="ct-body">
      <div class="ct-line dim">afr0dita@m0unt-0lympus:~/contact# cat ./reach_me.txt</div>
      <div class="ct-line">&nbsp;</div>
      <div class="ct-line comment"># preferably encrypted.</div>
      <div class="ct-line comment"># i read everything, i reply to what deserves a reply.</div>
      <div class="ct-line">&nbsp;</div>

      <a class="ct-link" href="mailto:{{ site.author.contact[3].url | remove: 'mailto:' }}">
        <span class="ct-key">email</span>
        <span class="ct-arrow">→</span>
        <span class="ct-val">{{ site.author.contact[3].url | remove: 'mailto:' }}</span>
      </a>

      <a class="ct-link" href="{{ site.author.contact[0].url }}" target="_blank" rel="noopener">
        <span class="ct-key">github</span>
        <span class="ct-arrow">→</span>
        <span class="ct-val">{{ site.author.contact[0].val }}</span>
      </a>

      <div class="ct-line">&nbsp;</div>
      <div class="ct-pgp">
        <div class="ct-line dim">afr0dita@m0unt-0lympus:~/contact# gpg --fingerprint {{ site.author.pgp }}</div>
        <div class="ct-line pgp-fp">{{ site.author.pgp_fingerprint }}</div>
      </div>
      <div class="ct-line">&nbsp;</div>
      <div class="ct-line dim">afr0dita@m0unt-0lympus:~/contact# <span class="cursor"></span></div>
    </div>

  </div>
</div>
