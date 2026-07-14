---
layout: page
title: Projects
permalink: /projects/
---

# Programs

{% assign program_projects = site.projects | where: "section", "programs" | sort: 'order' %}
{% if program_projects.size > 0 %}
<div class="projects-grid">
  {% for project in program_projects %}
    {% include project-card.html project=project %}
  {% endfor %}
</div>
{% else %}
*Coming soon — a showcase of research projects and open-source work.*
{% endif %}

# Computational Essays

{% assign essay_projects = site.projects | where: "section", "essays" | sort: 'order' %}
{% if essay_projects.size > 0 %}
<div class="projects-grid">
  {% for project in essay_projects %}
    {% include project-card.html project=project %}
  {% endfor %}
</div>
{% else %}
*Coming soon.*
{% endif %}
