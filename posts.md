---
layout: page
title: My Stuff
permalink: /posts/
sidebar_link: true
sidebar_sort_order: 2
---

<p class="message">
  Things I've done and taken the time to write about, or other notes.
</p>

{% for post in site.posts %}
## [ {{ post.title }}]( {{ site.baseurl }}{{ post.url }} )
{{ post.date | date: "%B %-d, %Y" }}

{% if post.excerpt %}
  {{ post.excerpt }}
{% else %}
  {{ post.content }}
{% endif %}

{% if post.excerpt %}
  {% comment %}Excerpt may be equal to content. Check.{% endcomment %}
  {% capture content_words %}
    {{ post.content | number_of_words }}
  {% endcapture %}
  {% capture excerpt_words %}
    {{ post.excerpt | number_of_words }}
  {% endcapture %}

  {% if content_words != excerpt_words %}
[More &hellip;]( {{ site.baseurl }}{{ post.url }} )
  {% endif %}
{% endif %}

	{% if forloop.last %}{% else %}
***
  {% endif %}
{% endfor %}
