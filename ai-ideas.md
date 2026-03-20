---
layout: default
title: AI开发想法
permalink: /ai-ideas/
---

<div class="post">
    <header class="post-header">
        <h1 class="post-title">🤖 AI开发想法</h1>
    </header>

    <div class="post-content">
        <p>这页整理我在 AI 辅助研发中的核心观点与执行标准。</p>

        <h2>一、工程规范</h2>
        <ul>
            <li>后端开发遵循 Uber Go 编码规范与工程实践。</li>
            <li>代码评审优先保证一致性、可读性与可维护性。</li>
            <li>版本迭代期间拒绝临时新增需求，避免范围蔓延。</li>
        </ul>

        <h2>二、文档标准</h2>
        <ul>
            <li>每个功能都需要配套 Markdown 文档，统一放在 <strong>doc/</strong> 目录。</li>
            <li>产品需求（PRD）必须以 Markdown 形式存放在 <strong>doc/</strong> 并进行版本化管理。</li>
            <li>接口文档、测试文档必须持续更新维护，确保长期可追溯。</li>
        </ul>

        <h2>三、AI 驱动开发要求</h2>
        <ul>
            <li>开发过程必须使用 AI 工具，并结合 <strong>Agent</strong> 与 <strong>Skill</strong> 能力。</li>
            <li>研发人员需熟练掌握 AI 辅助开发技巧：需求拆解、代码生成、重构、测试生成、文档生成。</li>
            <li>日志尽可能结构化、详细、可检索，做到 AI 友好，便于定位问题。</li>
        </ul>

        <h2>四、测试与交付</h2>
        <ul>
            <li>功能完成后，优先使用 AI 进行接口测试与回归验证。</li>
            <li>交付时必须输出测试文档（范围、用例、结果、问题记录）与接口文档。</li>
            <li>文档与代码同步迭代，确保系统可维护、可演进。</li>
        </ul>

        <blockquote>
            <p>目标：让 AI 不只是“写代码工具”，而是研发流程中的系统化生产力。</p>
        </blockquote>
    </div>

</div>
