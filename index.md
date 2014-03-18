---
layout: default
title: blog
---

<h2> {{ page.title }} </h2>

<ul>
	{% for post in site.posts %}
	<li>
		<a href="{{ post.url }}" target="_blank">{{ post.title }}</a> {{ post.date | date_to_string }}
	</li>
	{% endfor %}
</ul>