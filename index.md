---
layout: default
title: 首页
---

<div class="hero">
    <h1>👋 欢迎来到我的技术博客</h1>
    <p>记录代码人生，分享技术见解</p>
</div>

<div class="posts-list">
    {% for post in site.posts %}
    <article class="post-card">
        <h2>
            <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
        </h2>
        {% if post.excerpt %}
        <div class="post-excerpt">
            {{ post.excerpt | strip_html | truncatewords: 50 }}
        </div>
        {% endif %}
        <div class="post-meta">
            <time datetime="{{ post.date | date_to_xmlschema }}">
                <i class="far fa-calendar"></i> {{ post.date | date: "%Y-%m-%d" }}
            </time>
            {% if post.categories %}
            <span class="post-category">
                <i class="far fa-folder"></i> {{ post.categories | join: ", " }}
            </span>
            {% endif %}
            {% if post.tags %}
            <span class="post-tags-preview">
                <i class="fas fa-tags"></i> {{ post.tags | slice: 0, 3 | join: ", " }}
            </span>
            {% endif %}
        </div>
    </article>
    {% endfor %}
</div>

{% if site.posts.size == 0 %}

<div class="no-posts" style="text-align: center; padding: 3rem; color: var(--text-secondary);">
    <i class="fas fa-pencil-alt" style="font-size: 3rem; margin-bottom: 1rem;"></i>
    <p>还没有文章，敬请期待...</p>
</div>
{% endif %}
