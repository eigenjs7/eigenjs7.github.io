---
layout: page
title: Projects
permalink: /projects/
---

# Projects

{% assign sorted_projects = site.projects | sort: 'order' %}
{% if sorted_projects.size > 0 %}
<div class="projects-grid">
  {% for project in sorted_projects %}
    {% include project-card.html project=project %}
  {% endfor %}
</div>
{% else %}
*Coming soon — a showcase of research projects and open-source work.*
{% endif %}
