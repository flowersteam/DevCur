---
layout: page
title: Participate
permalink: /participate
---

<p>Interested in contributing to our research? Below you'll find opportunities to participate remotely or join us at one of our partner sites.</p>

## Remote

<div class="project-list">
  {% assign remote_posts = site.posts | where: 'participate_type', 'remote' | where: 'status', 'Ongoing' %}
  {% for post in remote_posts %}
  <a href="{{ post.url | relative_url }}" class="project-card">
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
    <span class="project-card__status project-card__status--ongoing">{{ post.status }}</span>
  </a>
  {% endfor %}
</div>

## On-Site

<div class="project-list">
  {% assign onsite_posts = site.posts | where: 'participate_type', 'On-site' | where: 'status', 'Ongoing' %}
  {% for post in onsite_posts %}
  <a href="{{ post.url | relative_url }}" class="project-card">
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
    <span class="project-card__status project-card__status--ongoing">{{ post.status }}</span>
  </a>
  {% endfor %}
</div>
