---
layout: default
title: 运维经验
permalink: /devops/
---

<div class="devops-container">
    <h1 class="devops-title">
        <i class="fas fa-server"></i> 运维经验
    </h1>
    
    <div class="devops-intro">
        <p>在游戏服务器运维领域拥有丰富的实战经验，擅长云平台部署、CI/CD流程搭建和容器化部署。</p>
    </div>

    <!-- 腾讯云运维经验 -->
    <section class="devops-section">
        <h2 class="section-title">
            <i class="fas fa-cloud"></i> 腾讯云运维实践
        </h2>
        
        <div class="devops-content">
            <div class="devops-description">
                <h3>云原生架构部署</h3>
                <p>基于腾讯云平台构建游戏服务器集群，实现自动化部署和弹性伸缩。</p>
                
                <div class="tech-highlights">
                    <div class="highlight-item">
                        <i class="fas fa-check-circle"></i>
                        <div>
                            <h4>容器化部署</h4>
                            <p>使用Docker + Kubernetes实现服务容器化，支持快速扩容和滚动更新</p>
                        </div>
                    </div>
                    <div class="highlight-item">
                        <i class="fas fa-check-circle"></i>
                        <div>
                            <h4>CI/CD流水线</h4>
                            <p>搭建自动化构建和部署流水线，实现代码提交到生产环境的全自动化</p>
                        </div>
                    </div>
                    <div class="highlight-item">
                        <i class="fas fa-check-circle"></i>
                        <div>
                            <h4>监控告警系统</h4>
                            <p>集成Prometheus + Grafana，实时监控服务状态和性能指标</p>
                        </div>
                    </div>
                    <div class="highlight-item">
                        <i class="fas fa-check-circle"></i>
                        <div>
                            <h4>日志管理</h4>
                            <p>部署ELK stack，集中管理和分析海量游戏日志数据</p>
                        </div>
                    </div>
                </div>
            </div>

            <div class="devops-images">
                <div class="devops-image-card">
                    <img src="{{ '/assets/images/tencent_cloud/集群.png' | relative_url }}" alt="腾讯云集群资源管理">
                    <div class="image-caption">
                        <h4>集群资源管理</h4>
                        <p>资源概览与用量监控，实时掌握集群运行状态</p>
                        <div class="image-tags">
                            <span class="tag">资源监控</span>
                            <span class="tag">节点管理</span>
                            <span class="tag">组件部署</span>
                        </div>
                    </div>
                </div>

                <div class="devops-image-card">
                    <img src="{{ '/assets/images/tencent_cloud/cicd.png' | relative_url }}" alt="CI/CD流水线">
                    <div class="image-caption">
                        <h4>CI/CD自动化流水线</h4>
                        <p>构建计划与持续集成，实现代码自动构建和部署</p>
                        <div class="image-tags">
                            <span class="tag">自动构建</span>
                            <span class="tag">持续集成</span>
                            <span class="tag">代码管理</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- 技术栈 -->
    <section class="devops-section">
        <h2 class="section-title">
            <i class="fas fa-tools"></i> 运维技术栈
        </h2>
        
        <div class="tech-stack-grid">
            <div class="tech-category">
                <h3><i class="fas fa-cloud"></i> 云平台</h3>
                <div class="tech-items">
                    <span class="tech-badge">腾讯云</span>
                    <span class="tech-badge">AWS</span>
                    <span class="tech-badge">阿里云</span>
                </div>
            </div>

            <div class="tech-category">
                <h3><i class="fas fa-box"></i> 容器化</h3>
                <div class="tech-items">
                    <span class="tech-badge">Docker</span>
                    <span class="tech-badge">Kubernetes</span>
                    <span class="tech-badge">Helm</span>
                </div>
            </div>

            <div class="tech-category">
                <h3><i class="fas fa-code-branch"></i> CI/CD</h3>
                <div class="tech-items">
                    <span class="tech-badge">GitLab CI</span>
                    <span class="tech-badge">Jenkins</span>
                    <span class="tech-badge">ArgoCD</span>
                </div>
            </div>

            <div class="tech-category">
                <h3><i class="fas fa-chart-line"></i> 监控告警</h3>
                <div class="tech-items">
                    <span class="tech-badge">Prometheus</span>
                    <span class="tech-badge">Grafana</span>
                    <span class="tech-badge">CloudWatch</span>
                </div>
            </div>

            <div class="tech-category">
                <h3><i class="fas fa-file-alt"></i> 日志管理</h3>
                <div class="tech-items">
                    <span class="tech-badge">ELK Stack</span>
                    <span class="tech-badge">OpenSearch</span>
                    <span class="tech-badge">Fluentd</span>
                </div>
            </div>

            <div class="tech-category">
                <h3><i class="fas fa-database"></i> 数据库</h3>
                <div class="tech-items">
                    <span class="tech-badge">MongoDB</span>
                    <span class="tech-badge">Redis</span>
                    <span class="tech-badge">MySQL</span>
                    <span class="tech-badge">PostgreSQL</span>
                </div>
            </div>
        </div>
    </section>

    <!-- 运维成果 -->
    <section class="devops-section">
        <h2 class="section-title">
            <i class="fas fa-trophy"></i> 运维成果
        </h2>
        
        <div class="achievements-grid">
            <div class="achievement-card">
                <div class="achievement-icon">
                    <i class="fas fa-rocket"></i>
                </div>
                <h3>部署效率提升</h3>
                <p class="achievement-stat">200%</p>
                <p>通过CI/CD自动化，部署时间从小时级降至分钟级</p>
            </div>

            <div class="achievement-card">
                <div class="achievement-icon">
                    <i class="fas fa-shield-alt"></i>
                </div>
                <h3>服务可用性</h3>
                <p class="achievement-stat">99.9%</p>
                <p>基于K8s实现高可用架构，服务稳定性显著提升</p>
            </div>

            <div class="achievement-card">
                <div class="achievement-icon">
                    <i class="fas fa-chart-bar"></i>
                </div>
                <h3>资源利用率</h3>
                <p class="achievement-stat">40%</p>
                <p>通过容器化和动态扩缩容，资源利用率提升40%</p>
            </div>

            <div class="achievement-card">
                <div class="achievement-icon">
                    <i class="fas fa-clock"></i>
                </div>
                <h3>故障响应时间</h3>
                <p class="achievement-stat">5min</p>
                <p>完善的监控告警体系，实现故障快速定位和恢复</p>
            </div>
        </div>
    </section>
</div>
