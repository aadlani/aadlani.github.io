---
layout: page
title: Resume
---

# Software Engineering Executive

## PROFESSIONAL PROFILE
Senior technology and software development leader with a proven track record in technical and operations management, product development, and software architecture. Highly successful in creating vision, and effectively directing all aspect of the software development lifecycle.

## AREA OF EXPERTISE

- Engineering, Architecture, and Design Solutions
- Technology Strategy, Direction and Road Map
- Process and Productivity Improvement
- Team Development and Leadership
- Strategic Planning and Organizational Leadership
- Business Process and System Integration
- Product and Project Management
- Problem and Conflict Resolution

<h2 class="post-list-heading">WORK EXPERIENCE</h2>
<ul class="post-list">
{% for experience in site.data.cv-experience %}
<li>
  <span class="post-meta"> {{ experience.company }} <small>{{ experience.period }}</small></span>
  <h3>{{ experience.title }}</h3>
  <p>{{ experience.description }}</p>

<strong>Responsablities:</strong>

<ul>
{% for responsibility in experience.responsibilities %}
<li>{{responsibility}}</li>
{% endfor %}
</ul>

</li>
{% endfor %}
</ul>
<h2 class="post-list-heading">Education</h2>
<ul class="post-list">
{% for education in site.data.cv-education %}
<li>
  <span class="post-meta"> {{ education.school }} <small>{{ education.period }}</small></span>
  <h3>{{ education.short_title }}</h3>
  <p>{{ education.description }}</p>
</li>
{% endfor %}
<ul>
