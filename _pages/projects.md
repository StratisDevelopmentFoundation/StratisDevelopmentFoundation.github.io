---
title: Projects
author_profile: false
permalink: /projects/
---

# Active SDF Projects

{% assign all_pages = site.pages%}
{% for current_page in all_pages %}
{% if current_page.categories contains 'projects' %}
* [{{ current_page.title }}]({{ site.baseurl }}{{ current_page.url }}) {{ current_page.short_overview }}
{% endif %}
{% endfor %}
