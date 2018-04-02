---
title: Bounties
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

* OP_RETURN block explorer extension to the full node

Bounty: 100 STRAT

* Block Explorer with API to the full node

Bounty: 50 STRAT

* IPFS / Torrent version of the blockchain for faster downloading

Bounty: 20 STRAT

* .NET Core based ECDH (or other encryption protocol) solution for encrypted messaging through OP_RETURN messages using public/private key pairs

Bounty: 100 STRAT

([Contact](/contact/) the SDF for more information on what is required to claim these bounties)

### Projects

Bounties are available for the [Projects](/projects/) that the SDF is working on. However, you should join our [Discord](/discord/){: .btn .btn--info} to collaborate on those projects.