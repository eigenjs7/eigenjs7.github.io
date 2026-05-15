---
layout: page
title: Services
permalink: /services/
---

# Tutoring

{% assign sorted_services = site.services | sort: 'order' %}
{% if sorted_services.size > 0 %}
<div class="services-grid">
  {% for service in sorted_services %}
    {% include service-card.html service=service %}
  {% endfor %}
</div>
{% else %}
*Coming soon.*
{% endif %}

# Consulting

*By inquiry — details coming soon.*
