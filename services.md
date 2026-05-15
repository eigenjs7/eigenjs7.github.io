---
layout: page
title: Services
permalink: /services/
---

# Tutoring

{% assign tutoring_services = site.services | where: "section", "tutoring" | sort: "order" %}
{% if tutoring_services.size > 0 %}
<div class="services-grid">
  {% for service in tutoring_services %}
    {% include service-card.html service=service %}
  {% endfor %}
</div>
{% else %}
*Coming soon.*
{% endif %}

# Consulting

{% assign consulting_services = site.services | where: "section", "consulting" | sort: "order" %}
{% if consulting_services.size > 0 %}
<div class="services-grid services-grid--no-glyph">
  {% for service in consulting_services %}
    {% include service-card.html service=service %}
  {% endfor %}
</div>
{% else %}
*By inquiry — details coming soon.*
{% endif %}
