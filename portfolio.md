---
layout: default
title: 作品集
permalink: /portfolio/
---

<div class="portfolio-container">
    <h1 class="portfolio-title">
        <i class="fas fa-images"></i> 我的作品集
    </h1>
    
    <div class="portfolio-intro">
        <p>这里展示了我的一些项目作品、照片和视频创作。</p>
    </div>

    <!-- 照片作品区 -->
    <section class="portfolio-section">
        <h2 class="section-title">
            <i class="fas fa-camera"></i> 照片作品
        </h2>
        <div class="gallery">
            <div class="gallery-item">
                <img src="{{ '/assets/images/photo1.jpg' | relative_url }}" alt="作品1">
                <div class="gallery-caption">
                    <h3>作品标题 1</h3>
                    <p>作品描述和说明</p>
                </div>
            </div>
            <div class="gallery-item">
                <img src="{{ '/assets/images/photo2.jpg' | relative_url }}" alt="作品2">
                <div class="gallery-caption">
                    <h3>作品标题 2</h3>
                    <p>作品描述和说明</p>
                </div>
            </div>
            <div class="gallery-item">
                <img src="{{ '/assets/images/photo3.jpg' | relative_url }}" alt="作品3">
                <div class="gallery-caption">
                    <h3>作品标题 3</h3>
                    <p>作品描述和说明</p>
                </div>
            </div>
        </div>
    </section>

    <!-- 视频作品区 -->
    <section class="portfolio-section">
        <h2 class="section-title">
            <i class="fas fa-video"></i> 视频作品
        </h2>
        <div class="video-gallery">
            <div class="video-item">
                <div class="video-wrapper">
                    <iframe src="https://www.youtube.com/embed/VIDEO_ID" 
                            frameborder="0" 
                            allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
                            allowfullscreen>
                    </iframe>
                </div>
                <div class="video-caption">
                    <h3>视频标题 1</h3>
                    <p>视频描述和说明</p>
                </div>
            </div>
            <div class="video-item">
                <div class="video-wrapper">
                    <video controls>
                        <source src="{{ '/assets/videos/video1.mp4' | relative_url }}" type="video/mp4">
                        您的浏览器不支持视频标签。
                    </video>
                </div>
                <div class="video-caption">
                    <h3>视频标题 2</h3>
                    <p>视频描述和说明</p>
                </div>
            </div>
        </div>
    </section>

    <!-- 项目作品区 -->
    <section class="portfolio-section">
        <h2 class="section-title">
            <i class="fas fa-code"></i> 项目作品
        </h2>
        <div class="projects-grid">
            <div class="project-card">
                <div class="project-thumbnail">
                    <img src="{{ '/assets/images/project1.jpg' | relative_url }}" alt="项目1">
                </div>
                <div class="project-info">
                    <h3>项目名称 1</h3>
                    <p>项目简介和技术栈说明</p>
                    <div class="project-tags">
                        <span class="tag">Python</span>
                        <span class="tag">Django</span>
                        <span class="tag">Vue.js</span>
                    </div>
                    <div class="project-links">
                        <a href="#" target="_blank"><i class="fas fa-link"></i> 演示</a>
                        <a href="#" target="_blank"><i class="fab fa-github"></i> 源码</a>
                    </div>
                </div>
            </div>
            <div class="project-card">
                <div class="project-thumbnail">
                    <img src="{{ '/assets/images/project2.jpg' | relative_url }}" alt="项目2">
                </div>
                <div class="project-info">
                    <h3>项目名称 2</h3>
                    <p>项目简介和技术栈说明</p>
                    <div class="project-tags">
                        <span class="tag">React</span>
                        <span class="tag">Node.js</span>
                        <span class="tag">MongoDB</span>
                    </div>
                    <div class="project-links">
                        <a href="#" target="_blank"><i class="fas fa-link"></i> 演示</a>
                        <a href="#" target="_blank"><i class="fab fa-github"></i> 源码</a>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>
