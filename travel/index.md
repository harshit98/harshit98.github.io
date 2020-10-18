---
layout: page
title: Travel
description: Travel blog by Harshit Prasad
permalink: /travel/
---

This section of my website tells more about where I have travelled within my country India or across India. You will enjoy a lot by reading and going through this section!  

<ul>
  {% for post in site.categories.travel %}
    <li>
        <span>{{ post.date | date_to_string }}</span> » <a href="{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
