---
layout: page
title: Talks
description: Conference Talks and Posters by Harshit Prasad
permalink: /talks/
---

This section highlights about my tech conference talks which I've given till date. I hope you'll also learn something from this section!  

<ul>
  {% for post in site.categories.talks %}
    <li>
        <span>{{ post.date | date_to_string }}</span> » <a href="{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
