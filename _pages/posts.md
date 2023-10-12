---
permalink: /posts
title: "Posts"
excerpt: "post"
author_profile: true
layout: archive
# category: "/_posts"
#redirect_from:
#- /about/
#- /about.html
---

<ul class="posts">
    <h3>{{page.title}}</h3>
  {% for post in page.posts %}
    <li>
      <span class="post-date">{{ post.date | date: "%b %-d, %Y" }}</span>
      <a class="post-link" href="{{ post.url | relative_url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
