---
layout: default
title: 项目经历
permalink: /experience/
---

<div class="experience-container">
    <h1 class="experience-title">
        <i class="fas fa-briefcase"></i> 项目经历
    </h1>
    
    <div class="experience-intro">
        <p>专注于游戏服务器开发和分布式系统架构，积累了丰富的大规模在线游戏后端开发经验。</p>
    </div>

    <!-- 时间线布局 -->
    <div class="timeline">
        
        <!-- 项目经历 1 -->
        <div class="timeline-item">
            <div class="timeline-marker"></div>
            <div class="timeline-content">
                <div class="timeline-date">
                    <i class="fas fa-calendar-alt"></i> 2022.01 - 至今
                </div>
                <div class="experience-card">
                    <div class="experience-header">
                        <h2>
                            <i class="fas fa-gamepad"></i> 大型MMO游戏服务器架构
                        </h2>
                        <span class="role-badge">技术负责人</span>
                    </div>
                    <div class="company-info">
                        <i class="fas fa-building"></i> 核心项目
                    </div>
                    <div class="experience-description">
                        <p>设计并实现企业级MMO游戏服务器架构，采用领域驱动设计（DDD）和微服务架构，支持大规模玩家在线、动态扩容、热更新等特性。</p>
                        <h4>核心成果：</h4>
                        <ul>
                            <li>设计基于 Actor 模型的分布式游戏服务器架构，支持横向扩展</li>
                            <li>实现玩家系统、社交系统、战斗系统、宠物系统、建筑系统等核心gameplay</li>
                            <li>使用 MongoDB 分片集群存储玩家数据，Redis 集群作为缓存层</li>
                            <li>采用 NATS 消息队列实现服务间通信，保证消息可靠性</li>
                            <li>建立完善的服务发现、配置管理、监控告警体系</li>
                            <li>服务器承载能力提升至单服 10,000+ 在线玩家，响应时间 < 50ms</li>
                        </ul>
                    </div>
                    <div class="tech-stack">
                        <h4><i class="fas fa-tools"></i> 技术栈：</h4>
                        <div class="tech-tags">
                            <span class="tech-tag">Go</span>
                            <span class="tech-tag">Actor模型</span>
                            <span class="tech-tag">MongoDB</span>
                            <span class="tech-tag">Redis</span>
                            <span class="tech-tag">NATS</span>
                            <span class="tech-tag">gRPC</span>
                            <span class="tech-tag">Docker</span>
                            <span class="tech-tag">Kubernetes</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- 项目经历 2 -->
        <div class="timeline-item">
            <div class="timeline-marker"></div>
            <div class="timeline-content">
                <div class="timeline-date">
                    <i class="fas fa-calendar-alt"></i> 2023.06 - 2024.03
                </div>
                <div class="experience-card">
                    <div class="experience-header">
                        <h2>
                            <i class="fas fa-link"></i> 区块链游戏服务器 (GameFi)
                        </h2>
                        <span class="role-badge">核心开发</span>
                    </div>
                    <div class="company-info">
                        <i class="fas fa-building"></i> 创新项目
                    </div>
                    <div class="experience-description">
                        <p>开发基于 Sui 区块链的游戏服务器，实现链上资产与游戏逻辑的深度结合，支持 NFT 道具、代币经济、玩家资产上链等功能。</p>
                        <h4>核心成果：</h4>
                        <ul>
                            <li>使用 Sui Move 开发智能合约，实现游戏资产的链上管理</li>
                            <li>设计混合架构，链下游戏逻辑 + 链上资产确权，平衡性能与去中心化</li>
                            <li>实现钱包连接、交易签名、链上查询等 Web3 功能</li>
                            <li>开发资产跨链桥接工具，支持多链资产流通</li>
                            <li>成功上线并运营，日活用户 1000+，链上交易总额超 $100K</li>
                        </ul>
                    </div>
                    <div class="tech-stack">
                        <h4><i class="fas fa-tools"></i> 技术栈：</h4>
                        <div class="tech-tags">
                            <span class="tech-tag">Go</span>
                            <span class="tech-tag">Sui Move</span>
                            <span class="tech-tag">Web3</span>
                            <span class="tech-tag">TypeScript</span>
                            <span class="tech-tag">MongoDB</span>
                            <span class="tech-tag">Redis</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- 项目经历 3 -->
        <div class="timeline-item">
            <div class="timeline-marker"></div>
            <div class="timeline-content">
                <div class="timeline-date">
                    <i class="fas fa-calendar-alt"></i> 2023.01 - 2023.08
                </div>
                <div class="experience-card">
                    <div class="experience-header">
                        <h2>
                            <i class="fas fa-tools"></i> MongoDB 可视化管理工具
                        </h2>
                        <span class="role-badge">个人项目</span>
                    </div>
                    <div class="company-info">
                        <i class="fas fa-building"></i> 开源项目
                    </div>
                    <div class="experience-description">
                        <p>开发桌面版 MongoDB 可视化管理工具，提供数据浏览、查询、编辑、导入导出等功能，简化数据库日常操作，提升开发效率。</p>
                        <h4>核心成果：</h4>
                        <ul>
                            <li>使用 Electron + TypeScript 开发跨平台桌面应用</li>
                            <li>实现连接管理、数据库/集合浏览、文档CRUD操作</li>
                            <li>支持复杂查询构建器和聚合管道可视化编辑</li>
                            <li>集成数据导入导出（JSON/CSV）和数据备份功能</li>
                            <li>开源发布，获得 GitHub Stars 500+</li>
                        </ul>
                    </div>
                    <div class="tech-stack">
                        <h4><i class="fas fa-tools"></i> 技术栈：</h4>
                        <div class="tech-tags">
                            <span class="tech-tag">TypeScript</span>
                            <span class="tech-tag">Electron</span>
                            <span class="tech-tag">React</span>
                            <span class="tech-tag">MongoDB Driver</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- 项目经历 4 -->
        <div class="timeline-item">
            <div class="timeline-marker"></div>
            <div class="timeline-content">
                <div class="timeline-date">
                    <i class="fas fa-calendar-alt"></i> 2022.09 - 2023.05
                </div>
                <div class="experience-card">
                    <div class="experience-header">
                        <h2>
                            <i class="fas fa-cube"></i> Sanji 游戏引擎
                        </h2>
                        <span class="role-badge">个人研究</span>
                    </div>
                    <div class="company-info">
                        <i class="fas fa-building"></i> 研究项目
                    </div>
                    <div class="experience-description">
                        <p>使用 Rust 开发的轻量级游戏引擎，探索现代游戏引擎架构和 ECS 设计模式，学习图形渲染、物理引擎、音频系统等游戏引擎核心技术。</p>
                        <h4>核心成果：</h4>
                        <ul>
                            <li>实现基于 ECS (Entity-Component-System) 的游戏架构</li>
                            <li>集成 wgpu 渲染后端，支持现代图形 API</li>
                            <li>实现场景管理、资源加载、输入系统等基础功能</li>
                            <li>深入学习 Rust 内存安全和并发编程特性</li>
                        </ul>
                    </div>
                    <div class="tech-stack">
                        <h4><i class="fas fa-tools"></i> 技术栈：</h4>
                        <div class="tech-tags">
                            <span class="tech-tag">Rust</span>
                            <span class="tech-tag">wgpu</span>
                            <span class="tech-tag">ECS</span>
                            <span class="tech-tag">Game Engine</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>

    </div>

    <!-- 技能总结 -->
    <section class="skills-summary">
        <h2 class="section-title">
            <i class="fas fa-code"></i> 技能总结
        </h2>
        <div class="skills-grid">
            <div class="skill-category">
                <h3><i class="fas fa-laptop-code"></i> 编程语言</h3>
                <div class="skill-items">
                    <span class="skill-item">Go</span>
                    <span class="skill-item">C++</span>
                    <span class="skill-item">TypeScript/JavaScript</span>
                    <span class="skill-item">Rust</span>
                    <span class="skill-item">C#</span>
                    <span class="skill-item">Move</span>
                </div>
            </div>
            <div class="skill-category">
                <h3><i class="fas fa-gamepad"></i> 游戏服务器</h3>
                <div class="skill-items">
                    <span class="skill-item">MMO架构</span>
                    <span class="skill-item">Actor模型</span>
                    <span class="skill-item">微服务架构</span>
                    <span class="skill-item">分布式系统</span>
                    <span class="skill-item">DDD领域驱动</span>
                    <span class="skill-item">网络编程</span>
                </div>
            </div>
            <div class="skill-category">
                <h3><i class="fas fa-network-wired"></i> 网络与协议</h3>
                <div class="skill-items">
                    <span class="skill-item">TCP/UDP</span>
                    <span class="skill-item">WebSocket</span>
                    <span class="skill-item">gRPC</span>
                    <span class="skill-item">HTTP/REST</span>
                    <span class="skill-item">KCP</span>
                    <span class="skill-item">Protobuf</span>
                </div>
            </div>
            <div class="skill-category">
                <h3><i class="fas fa-database"></i> 数据库与中间件</h3>
                <div class="skill-items">
                    <span class="skill-item">MongoDB</span>
                    <span class="skill-item">Redis</span>
                    <span class="skill-item">PostgreSQL</span>
                    <span class="skill-item">MySQL</span>
                    <span class="skill-item">NATS</span>
                    <span class="skill-item">Elasticsearch</span>
                </div>
            </div>
            <div class="skill-category">
                <h3><i class="fas fa-tools"></i> DevOps与工具</h3>
                <div class="skill-items">
                    <span class="skill-item">Docker</span>
                    <span class="skill-item">Kubernetes</span>
                    <span class="skill-item">Git</span>
                    <span class="skill-item">GitHub Actions</span>
                    <span class="skill-item">CI/CD</span>
                    <span class="skill-item">Prometheus</span>
                </div>
            </div>
            <div class="skill-category">
                <h3><i class="fas fa-link"></i> 区块链</h3>
                <div class="skill-items">
                    <span class="skill-item">Sui Move</span>
                    <span class="skill-item">智能合约</span>
                    <span class="skill-item">Web3</span>
                    <span class="skill-item">GameFi</span>
                    <span class="skill-item">DApp</span>
                    <span class="skill-item">ERC-4337</span>
                </div>
            </div>
        </div>
    </section>

</div>
