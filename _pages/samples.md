---
title: Code Samples
permalink: /samples/
sidebar:
  nav: "newdev"
---
## Stratis C# code samples

{% assign code_samples = site.posts | where: "categories","code_sample" | sort: 'post_importance' %}
{% for code_sample in code_samples %}
### [{{ code_sample.title }}]({{ site.baseurl }}{{ code_sample.url }})
{% endfor %}