---
title: Learning
navigation_weight: 3
---
# Learning Resources

## Official Stratis team produced resources

## SDF produced articles

{% assign learning_posts = site.posts | where: "categories","learning" | sort: 'post_importance' %}
{% for post in learning_posts %}
### [{{ post.title }}]({{ site.baseurl }}{{ post.url }})
{% endfor %}

## Community produced resources