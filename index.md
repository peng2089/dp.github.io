---
layout: default
title: 蜗牛的部落格
---

<h2><a href="{{ site.url }}"> {{ page.title }} </a></h2>

<ul>
	{% for post in site.posts %}
	<li>
		<a href="{{ post.url }}" target="_blank">{{ post.title }}</a> {{ post.date | date_to_string }}
	</li>
	{% endfor %}
</ul>