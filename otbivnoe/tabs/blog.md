---
title: Otbivnoe's blog
layout: default
---

<div class="main__avatar-container">
	<img src="/assets/img/side-menu/avatar.png" class="avatar" alt="Avatar">				
</div>

<nav class="sidebar__nav--mobile">	
	<a class="sidebar__nav-item selected" href="#contact">Blog</a>
	<a class="sidebar__nav-item" href="#about">About</a>
	<a class="sidebar__nav-item" href="#contact">Contact</a>
</nav>

{% for post in site.posts | sort: "date" %}
<article class ="article">
	<p class="article-date">{{ post.date | date_to_long_string }}</p>
	<h1 class="article-title">{{ post.title }}</h1>
	<p class="article-description">{{ post.subtitle }}</h1>
</article>
{% endfor %}
