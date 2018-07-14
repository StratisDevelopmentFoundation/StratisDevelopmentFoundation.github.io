---
title: Possible Users Auto
permalink: /PossibleUsersAuto/
author_profile: false
---

# Welcome Possible Users

If you are a Possible User who wants to learn about Stratis, we have laid out groups of interest and subsequenst lesson plans with the topics you should focus on: 
{% assign all_pages = site.pages%}

## Solo Developers
{% for current_page in all_pages %}
{% if current_page.categories contains 'solodev' %}
- ### [{{ current_page.title }}]({{ site.baseurl }}{{ current_page.url }}) 
{% endif %}
{% endfor %}

## Companies
### Crypto Company
{% for current_page in all_pages %}
{% if current_page.categories contains 'cryptocompany' %}
- ### [{{ current_page.title }}]({{ site.baseurl }}{{ current_page.url }})
{% endif %}
{% endfor %}

### Established Companies
{% for current_page in all_pages %}
{% if current_page.categories contains 'establishedcompany' %}
- ### [{{ current_page.title }}]({{ site.baseurl }}{{ current_page.url }})
{% endif %}
{% endfor %}

## Investors
{% for current_page in all_pages %}
{% if current_page.categories contains 'investor' %}
- ### [{{ current_page.title }}]({{ site.baseurl }}{{ current_page.url }})
{% endif %}
{% endfor %}