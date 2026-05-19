---
layout: page
title: Contact
permalink: /contact
---

<p>Authors of this webpage are collaborators of the DevCur project, and can be contacted through information provided at the end of each subproject page.</p>

<p>For general inquiries regarding the DevCur project, please send a group email to the four co-leaders of this project listed below. Otherwise, you can also contact the team of your interest.</p>

<style>
.collab-filters {
  display: flex;
  gap: 0.75rem;
  margin-bottom: 1.75rem;
  flex-wrap: wrap;
  align-items: center;
}
.filter-btn {
  padding: 0.35rem 1.1rem;
  border: 2px solid #666;
  border-radius: 2rem;
  background: transparent;
  cursor: pointer;
  font-size: 0.88rem;
  font-weight: 600;
  color: #666;
  transition: background 0.18s, color 0.18s, border-color 0.18s;
}
.filter-btn.active {
  background: #444;
  border-color: #444;
  color: #fff;
}
.collab-section {
  margin-bottom: 2.5rem;
}
.collab-section__title {
  font-size: 1rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.07em;
  color: #555;
  margin: 0 0 1.1rem 0;
  padding-bottom: 0.4rem;
  border-bottom: 2px solid #ddd;
}
</style>

<div class="collab-filters">
  <button class="filter-btn" id="btn-coleader">Co-leader</button>
  <button class="filter-btn" id="btn-location">Location</button>
</div>

<div id="collab-container"></div>

<script>
(function () {
  var BASE_URL = "{{ '/' | relative_url }}";
  var PEOPLE = [
    {% for person in site.collaborators %}
    {
      name:     {{ person.name     | jsonify }},
      initials: {{ person.initials | jsonify }},
      role:     {{ person.role     | jsonify }},
      subrole:  {{ person.subrole  | jsonify }},
      photo:    {{ person.photo    | jsonify }},
      institution: {{ person.institution | jsonify }},
      city:     {{ person.city     | jsonify }},
      country:  {{ person.country  | jsonify }},
      email:    {{ person.email    | jsonify }},
      website:  {{ person.website  | jsonify }},
      coleader: {{ person.coleader }}
    }{% unless forloop.last %},{% endunless %}
    {% endfor %}
  ];

  var activeFilter = 'coleader';
  var btnColeader  = document.getElementById('btn-coleader');
  var btnLocation  = document.getElementById('btn-location');
  var container    = document.getElementById('collab-container');

  function esc(s) {
    return String(s || '')
      .replace(/&/g, '&amp;').replace(/</g, '&lt;')
      .replace(/>/g, '&gt;').replace(/"/g, '&quot;');
  }

  function shuffle(arr) {
    for (var i = arr.length - 1; i > 0; i--) {
      var j = Math.floor(Math.random() * (i + 1));
      var t = arr[i]; arr[i] = arr[j]; arr[j] = t;
    }
    return arr;
  }

  function cardHtml(p) {
    var avatar = (p.photo && p.photo !== '')
      ? '<img src="' + BASE_URL + p.photo + '" alt="' + esc(p.name) + '" />'
      : '<span>' + esc(p.initials) + '</span>';
    var role = esc(p.role) + (p.subrole ? ' &middot; ' + esc(p.subrole) : '');
    var email = (p.email && p.email !== '')
      ? '<a class="collab-card__email" href="mailto:' + esc(p.email) + '">'
        + '<i class="fas fa-envelope" aria-hidden="true"></i> ' + esc(p.email) + '</a>'
      : '';
    var site = (p.website && p.website !== '')
      ? '<a class="collab-card__website" href="' + esc(p.website) + '" target="_blank" rel="noopener">'
        + '<i class="fas fa-external-link-alt" aria-hidden="true"></i> Website</a>'
      : '';
    return '<div class="collab-card">'
      + '<div class="collab-card__avatar">' + avatar + '</div>'
      + '<div class="collab-card__info">'
      + '<h3 class="collab-card__name">' + esc(p.name) + '</h3>'
      + '<p class="collab-card__role">' + role + '</p>'
      + '<p class="collab-card__location"><i class="fas fa-map-marker-alt" aria-hidden="true"></i> '
        + esc(p.city) + ' &middot; ' + esc(p.country) + '</p>'
      + email + site
      + '</div></div>';
  }

  function sectionHtml(title, people) {
    var header = title ? '<h2 class="collab-section__title">' + esc(title) + '</h2>' : '';
    return '<div class="collab-section">' + header
      + '<div class="collab-grid">'
      + shuffle(people.slice()).map(cardHtml).join('')
      + '</div></div>';
  }

  function render() {
    var html = '';
    if (activeFilter === 'coleader') {
      var leaders = PEOPLE.filter(function (p) { return p.coleader; });
      var others  = PEOPLE.filter(function (p) { return !p.coleader; });
      html += sectionHtml('Co-leaders', leaders);
      html += sectionHtml('Team and Collaborators', others);
    } else if (activeFilter === 'location') {
      var groups = {};
      PEOPLE.forEach(function (p) {
        var key = p.institution || (p.city + ' · ' + p.country);
        (groups[key] = groups[key] || []).push(p);
      });
      shuffle(Object.keys(groups)).forEach(function (inst) {
        html += sectionHtml(inst, groups[inst]);
      });
    } else {
      html += sectionHtml(null, PEOPLE);
    }
    container.innerHTML = html;
  }

  btnColeader.addEventListener('click', function () {
    activeFilter = (activeFilter === 'coleader') ? null : 'coleader';
    btnColeader.classList.toggle('active', activeFilter === 'coleader');
    btnLocation.classList.remove('active');
    render();
  });

  btnLocation.addEventListener('click', function () {
    activeFilter = (activeFilter === 'location') ? null : 'location';
    btnLocation.classList.toggle('active', activeFilter === 'location');
    btnColeader.classList.remove('active');
    render();
  });

  btnColeader.classList.add('active');
  render();
}());
</script>
