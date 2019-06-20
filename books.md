---
layout: page
title: My Reading
permalink: /reading/
sidebar_link: true
sidebar_sort_order: 3
---

<p class="message">
  This is a list of books I've read. Obviously not *all* the books I've read, just those I've read or re-read since implementing this list. I use it to keep passages I want to remember or favourite quotations.
</p>

{% assign sorted = site.books | sort: 'date' | reverse %}
{% for book in sorted %}
### [ {{ book.name }} | {{ book.author }} ]( {{ book.url }} )
> {{ book.description }}

Finished on {{ book.date | date: "%B %-d, %Y" }}

***
{% endfor %}
