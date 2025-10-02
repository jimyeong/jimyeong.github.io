---
permalink: /algorithms
title: "Algorithms"
excerpt: "Algorithm problems and solutions"
layout: archive
author_profile: true
---

<div class="tag-filter-container">
  <h3>Filter by Topic</h3>
  <div class="tag-filter-buttons">
    <button class="tag-filter-btn active" data-tag="all">All Posts <span class="tag-count">({{ site.algorithms | size }})</span></button>
    {% assign all_tags = site.algorithms | map: 'tags' | join: ',' | split: ',' | uniq | sort %}
    {% for tag in all_tags %}
      {% if tag != "" %}
        {% assign tag_posts = site.algorithms | where_exp: "post", "post.tags contains tag" %}
        <button class="tag-filter-btn" data-tag="{{ tag | slugify }}">{{ tag }} <span class="tag-count">({{ tag_posts | size }})</span></button>
      {% endif %}
    {% endfor %}
  </div>
</div>

<div id="posts-container">
  {% assign posts = site.algorithms %}
  {% for post in posts %}
    <div class="post-item" data-tags="{% for tag in post.tags %}{{ tag | slugify }} {% endfor %}">
      {% include archive-single.html %}
    </div>
  {% endfor %}
</div>

<script>
document.addEventListener('DOMContentLoaded', function() {
  const filterButtons = document.querySelectorAll('.tag-filter-btn');
  const postItems = document.querySelectorAll('.post-item');

  filterButtons.forEach(button => {
    button.addEventListener('click', function() {
      const selectedTag = this.getAttribute('data-tag');

      // Update active button
      filterButtons.forEach(btn => btn.classList.remove('active'));
      this.classList.add('active');

      // Filter posts
      postItems.forEach(item => {
        const itemTags = item.getAttribute('data-tags');

        if (selectedTag === 'all' || itemTags.includes(selectedTag)) {
          item.style.display = '';
        } else {
          item.style.display = 'none';
        }
      });
    });
  });
});
</script>
