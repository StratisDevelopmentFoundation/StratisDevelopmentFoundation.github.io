---
title: Bounties
permalink: /bounties/
toc: true
toc_label: "Bounties"
toc_icon: "x"
sidebar:
  nav: "newdev"
author_profile: false
---
# Bounties

Process for completing bounties for reward: [Bounty Process](/bountyprocess/)

{% assign all_pages = site.pages%}

([Contact](/contact/) the SDF for more information on what is required to claim these bounties)

## Coding Tasks

{% for current_page in all_pages %}
{% if current_page.categories contains 'bounties' %}
{% if current_page.categories contains 'coding' %}
### [{{ current_page.title }}]({{ site.baseurl }}{{ current_page.url }})

Bounty: {{ current_page.bounty }} STRAT
{% endif %}
{% endif %}
{% endfor %}

## Article Writing

{% for current_page in all_pages %}
{% if current_page.categories contains 'bounties' %}
{% if current_page.categories contains 'article' %}
### [{{ current_page.title }}]({{ site.baseurl }}{{ current_page.url }})

Bounty: {{ current_page.bounty }} STRAT
{% endif %}
{% endif %}
{% endfor %}

## Projects

Bounties are available for the [Projects](/projects/) that the SDF is working on. However, you should join our [Discord](/discord/){: .btn .btn--info} to collaborate on those projects.

