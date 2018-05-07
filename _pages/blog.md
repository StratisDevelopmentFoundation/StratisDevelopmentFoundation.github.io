---
title: Blog
permalink: /blog/
author_profile: false
---

{% assign blog_posts = site.posts | where: "categories","blog" | sort: 'date' | reverse%}

<h3 class="archive__subtitle">{{ site.data.ui-text[site.locale].recent_posts | default: "Recent Posts" }}</h3>

{%- for post in blog_posts -%}
  {% include archive-single.html %}
{%- endfor -%}
