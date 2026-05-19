---
layout: page
title: Projects
permalink: /projects
---

<div class="project-list">
  {% for post in site.posts %}
  <a href="{{ post.url | relative_url }}" class="project-card{% if post.status == 'Completed' %} project-card--completed{% endif %}">
    <div class="project-card__body">
      <h3 class="project-card__title">{{ post.title }}</h3>
      {% if post.people %}
      <p class="project-card__people">
        <i class="fas fa-users" aria-hidden="true"></i>
        {{ post.people | join: ", " }}
      </p>
      {% endif %}
      <p class="project-card__summary">{{ post.summary }}</p>
    </div>
    {% if post.thumbnail %}
    <div class="project-card__thumbnail">
      <img src="{{ post.thumbnail | relative_url }}" alt="{{ post.title }}" />
    </div>
    {% endif %}
    {% if post.location %}
    <span class="project-card__location">
      <i class="fas fa-map-marker-alt" aria-hidden="true"></i>
      {{ post.location }}
    </span>
    {% endif %}
    <span class="project-card__status project-card__status--{{ post.status | downcase | replace: '-', '' | replace: ' ', '-' }}">{{ post.status }}</span>
  </a>
  {% endfor %}
</div>
