---
permalink: /portfolio
title: "Portfolio"
excerpt: "Showcasing my projects and work"
layout: archive
author_profile: true
---

<div class="portfolio-intro">
  <p>Here are some of the projects I've worked on. Each showcases different skills and technologies I've mastered throughout my development journey.</p>
</div>

<div class="portfolio-filter-container">
  <h3>Filter by Technology</h3>
  <div class="portfolio-filter-buttons">
    <button class="portfolio-filter-btn active" data-filter="all">All Projects <span class="filter-count">({{ site.portfolio | size }})</span></button>
    {% assign all_techs = "" | split: "" %}
    {% for item in site.portfolio %}
      {% if item.tech_stack %}
        {% for tech in item.tech_stack %}
          {% unless all_techs contains tech %}
            {% assign all_techs = all_techs | push: tech %}
          {% endunless %}
        {% endfor %}
      {% endif %}
    {% endfor %}
    {% assign sorted_techs = all_techs | sort %}
    {% for tech in sorted_techs %}
      {% assign tech_count = 0 %}
      {% for item in site.portfolio %}
        {% if item.tech_stack contains tech %}
          {% assign tech_count = tech_count | plus: 1 %}
        {% endif %}
      {% endfor %}
      <button class="portfolio-filter-btn" data-filter="{{ tech | slugify }}">{{ tech }} <span class="filter-count">({{ tech_count }})</span></button>
    {% endfor %}
  </div>
</div>

<div class="portfolio-grid" id="portfolio-container">
  {% assign sorted_portfolio = site.portfolio | sort: 'date' | reverse %}
  {% for item in sorted_portfolio %}
    <div class="portfolio-card" data-techs="{% if item.tech_stack %}{% for tech in item.tech_stack %}{{ tech | slugify }} {% endfor %}{% endif %}">
      <div class="portfolio-card-inner">
        {% if item.thumbnail %}
          <div class="portfolio-card-image">
            <a href="{{ item.url | relative_url }}">
              <img src="{{ item.thumbnail | relative_url }}" alt="{{ item.title }}" />
              <div class="portfolio-card-overlay">
                <span class="view-project">View Project <i class="fas fa-arrow-right"></i></span>
              </div>
            </a>
          </div>
        {% endif %}

        <div class="portfolio-card-content">
          <h2 class="portfolio-card-title">
            <a href="{{ item.url | relative_url }}">{{ item.title }}</a>
          </h2>

          {% if item.subtitle %}
            <p class="portfolio-card-subtitle">{{ item.subtitle }}</p>
          {% endif %}

          {% if item.excerpt %}
            <p class="portfolio-card-excerpt">{{ item.excerpt | markdownify | strip_html | truncate: 150 }}</p>
          {% endif %}

          {% if item.tech_stack %}
            <div class="portfolio-card-tech">
              {% for tech in item.tech_stack limit:5 %}
                <span class="tech-badge">{{ tech }}</span>
              {% endfor %}
              {% if item.tech_stack.size > 5 %}
                <span class="tech-badge more">+{{ item.tech_stack.size | minus: 5 }}</span>
              {% endif %}
            </div>
          {% endif %}

          <div class="portfolio-card-meta">
            {% if item.date %}
              <span class="portfolio-card-date">
                <i class="far fa-calendar-alt"></i> {{ item.date | date: "%b %Y" }}
              </span>
            {% endif %}

            {% if item.project_type %}
              <span class="portfolio-card-type">
                <i class="fas fa-tag"></i> {{ item.project_type }}
              </span>
            {% endif %}
          </div>

          <div class="portfolio-card-links">
            {% if item.live_url %}
              <a href="{{ item.live_url }}" class="card-link" target="_blank" rel="noopener" title="Live Demo">
                <i class="fas fa-external-link-alt"></i>
              </a>
            {% endif %}
            {% if item.github_url %}
              <a href="{{ item.github_url }}" class="card-link" target="_blank" rel="noopener" title="View Code">
                <i class="fab fa-github"></i>
              </a>
            {% endif %}
          </div>
        </div>
      </div>
    </div>
  {% endfor %}
</div>

<script>
document.addEventListener('DOMContentLoaded', function() {
  const filterButtons = document.querySelectorAll('.portfolio-filter-btn');
  const portfolioCards = document.querySelectorAll('.portfolio-card');

  filterButtons.forEach(button => {
    button.addEventListener('click', function() {
      const selectedFilter = this.getAttribute('data-filter');

      // Update active button
      filterButtons.forEach(btn => btn.classList.remove('active'));
      this.classList.add('active');

      // Filter portfolio items
      portfolioCards.forEach(card => {
        const cardTechs = card.getAttribute('data-techs');

        if (selectedFilter === 'all' || cardTechs.includes(selectedFilter)) {
          card.style.display = '';
          // Add fade-in animation
          card.style.animation = 'fadeIn 0.5s ease-in';
        } else {
          card.style.display = 'none';
        }
      });
    });
  });
});
</script>

<style>
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>
