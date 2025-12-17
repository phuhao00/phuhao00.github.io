---
layout: default
title: 文章归档
permalink: /archive/
---

<div class="post">
    <header class="post-header">
        <h1 class="post-title">📚 文章归档</h1>
    </header>
    
    <div class="post-content">
        {% assign postsByYear = site.posts | group_by_exp: "post", "post.date | date: '%Y'" %}
        
        {% for year in postsByYear %}
        <h2>{{ year.name }}</h2>
        <ul class="archive-list">
            {% for post in year.items %}
            <li>
                <time datetime="{{ post.date | date_to_xmlschema }}">
                    {{ post.date | date: "%m-%d" }}
                </time>
                <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
                {% if post.categories %}
                <span class="category-badge">{{ post.categories | first }}</span>
                {% endif %}
            </li>
            {% endfor %}
        </ul>
        {% endfor %}
        
        {% if site.posts.size == 0 %}
        <p style="text-align: center; color: var(--text-secondary); padding: 2rem;">
            <i class="fas fa-inbox"></i> 暂无文章
        </p>
        {% endif %}
    </div>
</div>

<style>
.archive-list {
    list-style: none;
    margin: 0;
    padding: 0;
}

.archive-list li {
    padding: 0.8rem 0;
    border-bottom: 1px solid var(--border-color);
    display: flex;
    align-items: center;
    gap: 1rem;
}

.archive-list time {
    color: var(--text-secondary);
    font-size: 0.9rem;
    min-width: 50px;
}

.archive-list a {
    flex: 1;
    color: var(--text-color);
    text-decoration: none;
    transition: color 0.3s ease;
}

.archive-list a:hover {
    color: var(--primary-color);
}

.category-badge {
    background: var(--gradient);
    color: white;
    padding: 0.2rem 0.8rem;
    border-radius: 15px;
    font-size: 0.8rem;
}
</style>
