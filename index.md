---
layout: page
title: Home page
tagline: Supporting tagline
use_math: true
menu: Home
weight: 0
---
{% include JB/setup %}

<div class="blog-index">
  {% assign post = site.posts.first %}
  {% assign content = post.content %}
  {% assign sources = post.sources %}
  {% assign authors = post.authors %}
  {% include post_detail.html %}
</div>

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}"><strong>{{ post.title }}</strong></a> – <em>{{ post.tagline }}</em><br>{{post.description}}</li>
  {% endfor %}
</ul>
