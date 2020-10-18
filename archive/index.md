---
layout: page
title: Archive
description: Thoughts about the software engineer life
permalink: /archive/
---

Whenever I'm trying to solve a specific problem in software engineering, I make sure to document everything so that I can remember them in the future. Hopefully, this section will interest you as others did.

<ul>
  {% for post in site.categories.archive %}
    <li>
        <span>{{ post.date | date_to_string }}</span> » <a href="{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a>
        <meta name="description" content="{{ post.summary | escape }}">
        <meta name="keywords" content="{{ post.tags | join: ', ' | escape }}"/>
    </li>
  {% endfor %}
</ul>
