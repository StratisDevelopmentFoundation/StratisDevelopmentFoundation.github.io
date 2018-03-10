---
title: Learning
navigation_weight: 3
---
# Learning Resources

## Official Stratis team produced resources

[Stratis Academy landing page](https://stratisplatform.com/academy/academy-resources/)

[Programming the blockchain in C#](https://programmingblockchain.gitbooks.io/programmingblockchain/content/)

## SDF produced articles

{% assign learning_posts = site.posts | where: "categories","learning" | sort: 'post_importance' %}
{% for post in learning_posts %}
### [{{ post.title }}]({{ site.baseurl }}{{ post.url }})
{% endfor %}

## Community produced resources

[Stratis Price and Staking Information](https://stratispool.com/)

[Stratis Block Explorer](https://chainz.cryptoid.info/strat/)