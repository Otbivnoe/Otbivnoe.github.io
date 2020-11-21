---
title: Otbivnoe's blog
layout: default
---

{% for post in site.posts | sort: "date" %}
<article class ="article">
	<p class="article-date">{{ post.date | date_to_long_string }}</p>
	<h1 class="article-title"><a href={{ post.url }}>{{ post.title }}</a></h1>
	<p class="article-description">{{ post.subtitle }}</h1>
</article>
{% endfor %}
