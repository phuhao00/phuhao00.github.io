---
layout: post
title: "Git常用命令速查表"
date: 2025-12-15 15:00:00 +0800
categories: [工具]
tags: [Git, 版本控制, 速查表]
---

## Git 命令速查

Git 是最流行的版本控制系统，掌握 Git 命令对开发者来说至关重要。

### 基础配置

```bash
# 配置用户信息
git config --global user.name "Your Name"
git config --global user.email "your@email.com"

# 查看配置
git config --list
```

### 仓库操作

```bash
# 初始化仓库
git init

# 克隆远程仓库
git clone <url>

# 查看远程仓库
git remote -v

# 添加远程仓库
git remote add origin <url>
```

### 基本操作

```bash
# 查看状态
git status

# 添加文件到暂存区
git add <file>
git add .  # 添加所有文件

# 提交更改
git commit -m "commit message"

# 查看提交历史
git log
git log --oneline  # 简洁显示
```

### 分支操作

```bash
# 查看分支
git branch

# 创建分支
git branch <branch-name>

# 切换分支
git checkout <branch-name>

# 创建并切换分支
git checkout -b <branch-name>

# 合并分支
git merge <branch-name>

# 删除分支
git branch -d <branch-name>
```

### 远程操作

```bash
# 拉取远程更新
git pull origin <branch>

# 推送到远程
git push origin <branch>

# 强制推送（慎用）
git push -f origin <branch>

# 查看远程分支
git branch -r
```

### 撤销操作

```bash
# 撤销工作区修改
git checkout -- <file>

# 撤销暂存区修改
git reset HEAD <file>

# 回退到上一个commit
git reset --soft HEAD^

# 回退并删除commit
git reset --hard HEAD^
```

### 标签操作

```bash
# 创建标签
git tag v1.0.0

# 创建带注释的标签
git tag -a v1.0.0 -m "version 1.0.0"

# 查看标签
git tag

# 推送标签
git push origin v1.0.0
git push origin --tags  # 推送所有标签
```

### 实用技巧

#### 1. 查看差异

```bash
# 查看工作区和暂存区的差异
git diff

# 查看暂存区和最新commit的差异
git diff --cached

# 查看两个commit的差异
git diff <commit1> <commit2>
```

#### 2. 储藏更改

```bash
# 储藏当前修改
git stash

# 查看储藏列表
git stash list

# 恢复储藏
git stash pop
```

#### 3. 重写历史

```bash
# 修改最后一次commit
git commit --amend

# 交互式rebase
git rebase -i HEAD~3
```

### 常见场景

#### 场景 1：撤销已 push 的 commit

```bash
git revert <commit-hash>
git push origin <branch>
```

#### 场景 2：合并多个 commit

```bash
git rebase -i HEAD~3
# 在编辑器中将pick改为squash
```

#### 场景 3：解决冲突

```bash
# 1. 拉取最新代码
git pull origin <branch>

# 2. 手动解决冲突文件

# 3. 标记为已解决
git add <file>

# 4. 完成合并
git commit
```

### 最佳实践

1. **频繁提交** - 保持小而频繁的提交
2. **清晰的 commit 信息** - 描述清楚做了什么改动
3. **使用分支** - 为每个功能创建独立分支
4. **定期 pull** - 保持本地代码与远程同步
5. **代码审查** - 使用 Pull Request 进行代码审查

### 参考资源

- [Pro Git Book](https://git-scm.com/book/zh/v2)
- [GitHub Guides](https://guides.github.com/)
- [Git Cheat Sheet](https://training.github.com/)

掌握这些命令，你就能高效地使用 Git 进行版本控制了！💪
