---
layout: resume
title: "Resume"
permalink: /resume/
---

{% assign resume = site.data.resume %}

<h2>Education</h2>
{% for edu in resume.education %}
  <h3>{{ edu.institution }}</h3>
  <p>{{ edu.degree }} <span style="float: right;">{{ edu.date }}</span></p>
  {% if edu.details %}
<ul>
  {% for detail in edu.details %}
    <li>{{ detail }}</li>
  {% endfor %}
</ul>
  {% endif %}
{% endfor %}

<h2>Skills</h2>
<ul>
  {% for skill in resume.skills %}
    <li>{{ skill }}</li>
  {% endfor %}
</ul>

<h2>Experience</h2>
{% for exp in resume.experience %}
  <h3>{{ exp.company }}</h3>
  <p>{{ exp.role }} <span style="float: right;">{{ exp.date }}</span></p>
  <ul>
    {% for detail in exp.details %}
      <li>{{ detail }}</li>
    {% endfor %}
  </ul>
{% endfor %}

<h2>Projects</h2>
{% for proj in resume.projects %}
  <h3>{{ proj.name }}</h3>
  <p>{{ proj.role }}</p>
  <ul>
    {% for detail in proj.details %}
      <li>{{ detail }}</li>
    {% endfor %}
  </ul>
{% endfor %}
