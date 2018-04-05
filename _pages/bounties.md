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

* adding getReceivedByAddress function to the full node

Bounty: 30 STRAT

* Build a dashboard UI on top of the api feature that allow to display stats about the node, this has to be flexible so other features can register links to there UIs

* Build a UI on top of the previous task for the wallet.

* Build a feature that allowes to index trx balances and query them via API

* Build a coloured coins wallet feature

* Build API endpoints that allow to build multisig trx and also sign existing multisig trx

* Build [WAVES gateway](https://github.com/jansenmarc/WavesGatewayFramework)

([Contact](/contact/) the SDF for more information on what is required to claim these bounties)

### Projects

Bounties are available for the [Projects](/projects/) that the SDF is working on. However, you should join our [Discord](/discord/){: .btn .btn--info} to collaborate on those projects.