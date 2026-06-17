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
        <p>这里展示了我的开源项目和技术作品，包括游戏服务器框架、开发工具等。</p>
    </div>

    <!-- 照片作品区 -->
    <section class="portfolio-section" style="display: none;">
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
            <i class="fas fa-video"></i> 项目演示视频
        </h2>
        <div class="video-gallery">
            <div class="video-item">
                <div class="video-wrapper">
                    <iframe src="https://player.bilibili.com/player.html?aid=115004762365875&bvid=BV1z5bKzuERK&cid=31609392126&page=1"
                            scrolling="no"
                            border="0"
                            frameborder="no"
                            framespacing="0"
                            allowfullscreen="true"
                            style="width: 100%; height: 100%; position: absolute; left: 0; top: 0;">
                    </iframe>
                </div>
                <div class="video-caption">
                    <h3>游戏生命周期管理工具</h3>
                    <p>一个覆盖游戏开发全生命周期的工具平台，支持策划配置、代码生成、资源管理、社区运营等功能。</p>
                </div>
            </div>
            <div class="video-item">
                <div class="video-wrapper">
                    <iframe src="https://player.bilibili.com/player.html?aid=113905938470048&bvid=BV1EZFxeFEYJ&cid=28123791925&page=1"
                            scrolling="no"
                            border="0"
                            frameborder="no"
                            framespacing="0"
                            allowfullscreen="true"
                            style="width: 100%; height: 100%; position: absolute; left: 0; top: 0;">
                    </iframe>
                </div>
                <div class="video-caption">
                    <h3>Unity独立游戏开发尝试</h3>
                    <p>使用Unity引擎开发的独立游戏项目，探索游戏玩法和美术风格的创新实践。</p>
                </div>
            </div>
        </div>
    </section>

    <!-- AI Agent 项目介绍区 -->
    <section class="portfolio-section">
        <h2 class="section-title">
            <i class="fas fa-robot"></i> AI Agent 平台 · 产品展示
        </h2>
        <div class="ai-agent-intro">
            <p class="ai-agent-desc">一套面向内容创作者与开发者的全链路 AI Agent 平台，集 <strong>智能对话 Agent</strong>、<strong>虚拟伴侣</strong>、<strong>自动化工作流</strong>、<strong>多模态工作台</strong>与 <strong>MCP 能力扩展</strong>于一体，让一个人也能完成整个内容团队的工作。</p>
        </div>
        <div class="ai-agent-screenshots">

            <div class="screenshot-group">
                <h3 class="screenshot-group-title"><i class="fas fa-comments"></i> AI 对话 · 主界面</h3>
                <div class="screenshot-row">
                    <div class="screenshot-item">
                        <img src="{{ '/assets/images/ai-agent/main.png' | relative_url }}" alt="AI Agent 主界面" loading="lazy">
                        <p>智能对话中枢，支持多 Agent 协作，从想法到交付一站完成</p>
                    </div>
                    <div class="screenshot-item">
                        <img src="{{ '/assets/images/ai-agent/companion.png' | relative_url }}" alt="虚拟伴侣" loading="lazy">
                        <p>3D 虚拟伴侣「波尼」，支持语音交互与情感陪伴</p>
                    </div>
                </div>
            </div>

            <div class="screenshot-group">
                <h3 class="screenshot-group-title"><i class="fas fa-headset"></i> 客服系统 · 工作台</h3>
                <div class="screenshot-row">
                    <div class="screenshot-item">
                        <img src="{{ '/assets/images/ai-agent/customer-service.png' | relative_url }}" alt="智能客服" loading="lazy">
                        <p>知识库检索 + 大模型回答，支持 FAQ / Agent 双模式</p>
                    </div>
                    <div class="screenshot-item">
                        <img src="{{ '/assets/images/ai-agent/workbench.png' | relative_url }}" alt="工作台" loading="lazy">
                        <p>全链路内容生产工作台：脚本、文案、图片、视频、音乐一站生成</p>
                    </div>
                </div>
            </div>

            <div class="screenshot-group">
                <h3 class="screenshot-group-title"><i class="fas fa-project-diagram"></i> 工作流 · 实验与工具</h3>
                <div class="screenshot-row">
                    <div class="screenshot-item">
                        <img src="{{ '/assets/images/ai-agent/workflow.png' | relative_url }}" alt="自动化工作流" loading="lazy">
                        <p>可视化拖拽工作流编排，热点生成视频全流程自动化</p>
                    </div>
                    <div class="screenshot-item">
                        <img src="{{ '/assets/images/ai-agent/labs.png' | relative_url }}" alt="实验与工具" loading="lazy">
                        <p>专业 Agent 工具集：客服、法律、广告、游戏策划等垂直领域</p>
                    </div>
                </div>
            </div>

            <div class="screenshot-group">
                <h3 class="screenshot-group-title"><i class="fas fa-plug"></i> MCP 能力 · 平台接入</h3>
                <div class="screenshot-row">
                    <div class="screenshot-item">
                        <img src="{{ '/assets/images/ai-agent/mcp.png' | relative_url }}" alt="MCP 配置" loading="lazy">
                        <p>MCP 插件市场，一键接入 DuckDuckGo、Playwright、Memory 等能力</p>
                    </div>
                    <div class="screenshot-item">
                        <img src="{{ '/assets/images/ai-agent/platform.png' | relative_url }}" alt="平台接入" loading="lazy">
                        <p>多平台接入管理：飞书 / Lark、Discord 等 IM 频道统一配置</p>
                    </div>
                </div>
            </div>

        </div>

        <div class="ai-agent-tags-row">
            <span class="tag">Python</span>
            <span class="tag">Node.js</span>
            <span class="tag">MCP</span>
            <span class="tag">LLM</span>
            <span class="tag">工作流自动化</span>
            <span class="tag">RAG 知识库</span>
            <span class="tag">飞书 / Discord</span>
            <span class="tag">多模态</span>
        </div>
    </section>

    <!-- 项目作品区 -->
    <section class="portfolio-section">
        <h2 class="section-title">
            <i class="fas fa-code"></i> 开源项目
        </h2>
        <div class="projects-grid">
            <div class="project-card">
                <div class="project-thumbnail">
                    <div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); height: 200px; display: flex; align-items: center; justify-content: center; color: white; font-size: 3rem;">
                        <i class="fas fa-gamepad"></i>
                    </div>
                </div>
                <div class="project-info">
                    <h3>游戏生命周期管理工具</h3>
                    <p>一站式游戏开发工作流管理平台，覆盖策划、程序、美术、运营全流程</p>
                    <div class="project-features">
                        <p><i class="fas fa-check-circle"></i> 策划Excel配置与数据管理</p>
                        <p><i class="fas fa-check-circle"></i> 自动生成框架代码和协议</p>
                        <p><i class="fas fa-check-circle"></i> 美术资源管理与抽卡系统</p>
                        <p><i class="fas fa-check-circle"></i> 社区管理与运营工具集成</p>
                    </div>
                    <div class="project-tags">
                        <span class="tag">Golang</span>
                        <span class="tag">Vue.js</span>
                        <span class="tag">自动化</span>
                        <span class="tag">工作流</span>
                    </div>
                    <div class="project-links">
                        <a href="https://www.bilibili.com/video/BV1z5bKzuERK/" target="_blank"><i class="fab fa-bilibili"></i> 演示视频</a>
                        <a href="https://github.com/phuhao00" target="_blank"><i class="fab fa-github"></i> GitHub</a>
                    </div>
                </div>
            </div>

            <div class="project-card">
                <div class="project-thumbnail">
                    <div style="background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%); height: 200px; display: flex; align-items: center; justify-content: center; color: white; font-size: 3rem;">
                        <i class="fas fa-server"></i>
                    </div>
                </div>
                <div class="project-info">
                    <h3>分布式游戏服务器框架</h3>
                    <p>基于Actor模型的高性能游戏服务器框架，支持云原生部署</p>
                    <div class="project-features">
                        <p><i class="fas fa-check-circle"></i> Actor模型 + 微服务架构</p>
                        <p><i class="fas fa-check-circle"></i> 支持高并发、可扩展</p>
                        <p><i class="fas fa-check-circle"></i> 云原生，易于部署和维护</p>
                        <p><i class="fas fa-check-circle"></i> 完善的监控和日志系统</p>
                    </div>
                    <div class="project-tags">
                        <span class="tag">Golang</span>
                        <span class="tag">Actor模型</span>
                        <span class="tag">微服务</span>
                        <span class="tag">Docker</span>
                        <span class="tag">K8s</span>
                    </div>
                    <div class="project-links">
                        <a href="https://github.com/phuhao00" target="_blank"><i class="fab fa-github"></i> 源码</a>
                    </div>
                </div>
            </div>

            <div class="project-card">
                <div class="project-thumbnail">
                    <div style="background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%); height: 200px; display: flex; align-items: center; justify-content: center; color: white; font-size: 3rem;">
                        <i class="fas fa-database"></i>
                    </div>
                </div>
                <div class="project-info">
                    <h3>MongoDB 可视化管理工具</h3>
                    <p>跨平台的MongoDB数据库管理工具，简化日常数据库操作</p>
                    <div class="project-features">
                        <p><i class="fas fa-check-circle"></i> 跨平台桌面应用</p>
                        <p><i class="fas fa-check-circle"></i> 可视化查询构建器</p>
                        <p><i class="fas fa-check-circle"></i> 数据导入导出功能</p>
                        <p><i class="fas fa-check-circle"></i> 聚合管道可视化</p>
                    </div>
                    <div class="project-tags">
                        <span class="tag">TypeScript</span>
                        <span class="tag">Electron</span>
                        <span class="tag">React</span>
                        <span class="tag">MongoDB</span>
                    </div>
                    <div class="project-links">
                        <a href="https://github.com/phuhao00" target="_blank"><i class="fab fa-github"></i> 源码</a>
                    </div>
                </div>
            </div>

            <div class="project-card">
                <div class="project-thumbnail">
                    <div style="background: linear-gradient(135deg, #fa709a 0%, #fee140 100%); height: 200px; display: flex; align-items: center; justify-content: center; color: white; font-size: 3rem;">
                        <i class="fas fa-link"></i>
                    </div>
                </div>
                <div class="project-info">
                    <h3>区块链游戏服务器</h3>
                    <p>基于Sui Move的GameFi游戏服务器，实现链上资产与游戏的融合</p>
                    <div class="project-features">
                        <p><i class="fas fa-check-circle"></i> Sui Move智能合约</p>
                        <p><i class="fas fa-check-circle"></i> 链上资产管理</p>
                        <p><i class="fas fa-check-circle"></i> Web3钱包集成</p>
                        <p><i class="fas fa-check-circle"></i> 混合架构设计</p>
                    </div>
                    <div class="project-tags">
                        <span class="tag">Golang</span>
                        <span class="tag">Sui Move</span>
                        <span class="tag">Web3</span>
                        <span class="tag">区块链</span>
                    </div>
                    <div class="project-links">
                        <a href="https://github.com/phuhao00" target="_blank"><i class="fab fa-github"></i> 源码</a>
                    </div>
                </div>
            </div>
        </div>
    </section>

</div>
