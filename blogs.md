---
layout: default
title: Blogs
permalink: /blogs/
---

## Blogs

Welcome to my blog section! Here you'll find my thoughts on machine learning, research, and technology.

{% assign sorted_blogs = site.blogs | sort: 'date' | reverse %}

{% if sorted_blogs.size > 0 %}
<div class="blog-list">
{% for blog in sorted_blogs %}
    <div class="blog-item">
        <h3><a href="{{ blog.url | relative_url }}">{{ blog.title }}</a></h3>
        <p class="blog-meta">
            <i class="fa fa-calendar" aria-hidden="true"></i> 
            {{ blog.date | date: site.minima.date_format }}
            {% if blog.author %}
            | <i class="fa fa-user" aria-hidden="true"></i> {{ blog.author }}
            {% endif %}
        </p>
        {% if blog.excerpt %}
        <p class="blog-excerpt">{{ blog.excerpt | strip_html | truncatewords: 30 }}</p>
        {% endif %}
        {% if blog.tags %}
        <p class="blog-tags">
            <i class="fa fa-tags" aria-hidden="true"></i> 
            {% for tag in blog.tags %}
                <span class="tag">{{ tag }}</span>{% unless forloop.last %}, {% endunless %}
            {% endfor %}
        </p>
        {% endif %}
        <p><a href="{{ blog.url | relative_url }}" class="read-more">Read more →</a></p>
    </div>
    <hr>
{% endfor %}
</div>
{% else %}
<p>No blog posts yet. Check back soon!</p>
{% endif %}
