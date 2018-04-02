---
title: Bounties
author_profile: false
permalink: /bounties/
---
# Bounties

Process for completing bounties for reward: [Bounty Process](/bountyprocess/)

### Article Writing

{% assign bounty_posts = site.posts | where: "categories","learning" | sort: 'bounty' %}
{% for post in bounty_posts %}
{% if post.bounty %}
* [{{ post.title }}]({{ site.baseurl }}{{ post.url }})

Bounty: {{ post.bounty }} STRAT
{% endif %}
{% endfor %}

### Software/Tools to be created

* OP_RETURN block explorer
* Stratis blockchain REST API

### Projects

Bounties are available for the [Projects](/projects/) that the SDF is working on. However, you should join our [Discord](/discord/) to collaborate on those projects.