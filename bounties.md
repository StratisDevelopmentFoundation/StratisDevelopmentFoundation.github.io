---
title: Bounties
navigation_weight: 5
---

# Bounties

Process for completing bounties for reward: [Bounty Process](/bountyprocess.html)

## Currently Available Bounties:

### Articles to be written

* Setting up the Stratis Full Node for RPC
* Setting up an Azure cloud development environment
* Setting up a Stratis blockchain block explorer on Azure
* How to use github to contribute to the SDF

### Articles to be improved

{% assign bounty_posts = site.posts | sort: 'bounty_for_improvement' %}
{% for post in bounty_posts %}
{% if post.bounty_for_improvement %}
* [{{ post.title }}]({{ site.baseurl }}{{ post.url }}) **[Edit on github.com](https://github.com/StratisDevelopmentFoundation/StratisDevelopmentFoundation.github.io/edit/master/{{ post.path }})**
Bounty: ${{ post.bounty_for_improvement }} USD
{% endif %}
{% endfor %}

### Software/Tools to be created

* OP_RETURN block explorer
* Stratis blockchain REST API
* Signing Messages and verifying signed messages sample code

### Protocols to be written

* Initial protocol specification
* Voting protocol
* Human readable address alias protocol
* Ownership of address attestation protocol
