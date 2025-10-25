---
layout: page
title: Tapahtumat
permalink: /tapahtumat/
---

# Tulevat tapahtumat

Tutustu tuleviin tapahtumiimme ja aktiviteetteihin. Kaikki jäsenet ja vieraat ovat tervetulleita!

<div class="events-list">
{% for event in site.events reversed %}
  <article class="event">
    <h2><a href="{{ event.url | relative_url }}">{{ event.title }}</a></h2>
    <p class="event-meta">
      <strong>Päivämäärä:</strong> {{ event.date | date: "%-d.%-m.%Y klo %H:%M" }}
      {% if event.location %}
      <br><strong>Paikka:</strong> {{ event.location }}
      {% endif %}
    </p>
    {% if event.image %}
    <img src="{{ event.image | relative_url }}" alt="{{ event.title }}">
    {% endif %}
    <div class="event-excerpt">
      {{ event.content | strip_html | truncatewords: 50 }}
    </div>
    <a href="{{ event.url | relative_url }}" class="read-more">Lue lisää &rarr;</a>
  </article>
{% endfor %}
</div>

{% if site.events.size == 0 %}
<p>Ei tulevia tapahtumia juuri nyt. Tarkista myöhemmin uudelleen!</p>
{% endif %}
