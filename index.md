---
layout: default
title: 首页
---

<div class="hero">
    <h1>👋 优雅永不过时</h1>
    <p>需要耐心一点</p>
</div>

<section class="portfolio-section">
    <h2 class="section-title">
        <i class="fas fa-film"></i> 项目作品录屏
    </h2>
    <div class="video-gallery">
        <div class="video-item">
            <div class="video-wrapper">
                <video controls preload="metadata" playsinline>
                    <source src="{{ '/assets/video/Feishu20260506-131528.mp4' | relative_url }}" type="video/mp4">
                    您的浏览器不支持 video 标签。
                </video>
            </div>
            <div class="video-caption">
                <h3>AI Media Agent 项目演示</h3>
                <p>项目实际操作录屏，展示内容生成、素材生产与自动化发布流程。</p>
            </div>
        </div>
    </div>
</section>

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
