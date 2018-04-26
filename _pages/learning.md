---
title: Learning
permalink: /learning/
sidebar:
  nav: "newdev"
toc: true
toc_label: "Learning Resources"
toc_icon: "x"
---
# Learning Resources:

## Installation Instructions

{% assign install_posts = site.posts | where: "categories","install" | sort: 'post_importance' %}
{% for post in install_posts %}
### [{{ post.title }}]({{ site.baseurl }}{{ post.url }})
{% endfor %}
{% assign install_pages = site.pages | where: "categories","install" | sort: 'post_importance' %}
{% for post in install_pages %}
### [{{ post.title }}]({{ site.baseurl }}{{ post.url }})
{% endfor %}

## Tutorials

{% assign tutorial_posts = site.posts | where: "categories","tutorial" | sort: 'post_importance' %}
{% for tutorial_post in tutorial_posts %}
### [{{ tutorial_post.title }}]({{ site.baseurl }}{{ tutorial_post.url }})
{% endfor %}

## Stratis C# code samples

{% assign code_samples = site.posts | where: "categories","code_sample" | sort: 'post_importance' %}
{% for code_sample in code_samples %}
### [{{ code_sample.title }}]({{ site.baseurl }}{{ code_sample.url }})
{% endfor %}

## Official Stratis team produced resources

### [Stratis Academy landing page](https://stratisplatform.com/academy/academy-resources/)

### [Programming the blockchain in C#](https://programmingblockchain.gitbooks.io/programmingblockchain/content/)

## Community produced resources

### [Stratis Price and Staking Information](https://stratispool.com/)

### [Stratis Block Explorer](https://chainz.cryptoid.info/strat/)