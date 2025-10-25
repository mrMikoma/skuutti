---
layout: page
title: Galleria
permalink: /galleria/
---

# Kuvagalleria

Selaa kokoelmaamme kuvia kilpailuista, tapahtumista ja seuran toiminnasta.

<div class="gallery-grid">
{% for album in site.gallery %}
  <article class="gallery-item">
    <a href="{{ album.url | relative_url }}">
      {% if album.cover_image %}
      <img src="{{ album.cover_image | relative_url }}" alt="{{ album.title }}">
      {% endif %}
      <h3>{{ album.title }}</h3>
      <p class="gallery-date">{{ album.date | date: "%B %Y" }}</p>
    </a>
  </article>
{% endfor %}
</div>

{% if site.gallery.size == 0 %}
<p>Ei vielä kuva-albumeita. Tarkista myöhemmin uudelleen kuvia tapahtumistamme!</p>
{% endif %}
