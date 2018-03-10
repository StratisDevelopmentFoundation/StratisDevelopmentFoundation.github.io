---
title: Bounties
navigation_weight: 5
---

# Bounties

Process for completing bounties for reward: x

## Currently Available Bounties:

### Articles to be written

* Setting up the Stratis Full Node for RPC

* Setting up an Azure cloud development environment

* Setting up a Stratis blockchain block explorer on Azure

### Articles to be improved

{% assign bounty_posts = site.posts | sort: 'bounty_for_improvement' %}
{% for post in bounty_posts %}
{% if post.bounty_for_improvement %}
### [{{ post.title }}]({{ site.baseurl }}{{ post.url }})
### [edit on github](https://github.com/StratisDevelopmentFoundation/StratisDevelopmentFoundation.github.io/blob/master/_posts/{{ post.path }})
Bounty: ${{ post.bounty_for_improvement }} USD
{% endif %}
{% endfor %}

### Software to be written