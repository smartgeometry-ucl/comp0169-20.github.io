---
title: Exhaustive Search
parent: Election Simulator
nav_order: 15
summary: BJP 12.5
---

# {{ page.title }}

[Ed](https://us.edstem.org/courses/631/lessons/1619){: .btn .btn-purple .fs-5 }

{% assign slides = site.slides | where_exp: "slide", "slide.url contains page.url" | sort: "nav_order" %}
{% if slides != empty %}
## Table of contents
{: .text-delta }

{% for slide in slides %}
1. [{{ slide.title }}](#{{ slide.title | slugify }}){% endfor %}

---

{% for slide in slides %}
## {{ slide.title }}
{{ slide.content }}
{% endfor %}
{% endif %}
