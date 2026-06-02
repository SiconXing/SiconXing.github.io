---
title: "Git 工作流最佳实践"
date: 2026-05-18 14:00:00 +0800
categories: [工具, 协作]
tags: [Git, 版本控制, 工作流, 团队协作]
pin: false
math: false
image:
  path: /assets/img/avatars/avatar.svg
  alt: Git
---

## 前言

Git 是现代软件开发的标配，但用好 Git 不止是 `add` `commit` `push`。本文总结了一套实用的 Git 工作流和常见场景的处理方式。

## 分支策略

```
main        ●────●─────────●─────────●── 生产环境
              \           /         /
develop   ●────●─────●───●───●─────●─── 开发主线
                \       /
feature/x  ●─────●─────●                 功能分支
```

**原则：**
- `main` 分支始终可部署
- 功能开发在 `feature/*` 分支
- 合并到 `main` 通过 Pull Request
- 提交信息遵循规范格式

## 提交信息规范

```bash
# 格式: <type>(<scope>): <subject>
# type: feat/fix/docs/style/refactor/test/chore

# ✅ 好的提交信息
feat(auth): 添加 JWT 登录功能
fix(api): 修复分页参数溢出问题
docs(readme): 更新安装说明
refactor(db): 重写数据库连接池

# ❌ 不好的提交信息
update
fix bug
wip
```

## 常用操作

### 修改最近一次提交

```bash
# 追加修改到上一 commit（不改 message）
git add forgotten_file.py
git commit --amend --no-edit

# 修改上一 commit 的 message
git commit --amend -m "新的提交信息"
```

### 合并 vs Rebase

```bash
# Merge — 保留完整历史，产生合并提交
git checkout main
git merge feature/login

# Rebase — 线性历史，更清晰
git checkout feature/login
git rebase main
git checkout main
git merge feature/login  # fast-forward
```

### 交互式 Rebase

```bash
# 整理最近 3 个 commit
git rebase -i HEAD~3

# 在编辑器中:
# pick a1b2c3d feat: 添加 API
# squash d4e5f6g fix: 修复拼写      ← 合并到上一个
# reword h7i8j9k fix: 更新日志      ← 修改 message

# 结果: 2 个干净的 commit
```

### Cherry-pick

```bash
# 把某个 commit 单独应用到当前分支
git cherry-pick abc1234
```

## 紧急修复

```bash
# 1. 从 main 切出 hotfix 分支
git checkout main
git checkout -b hotfix/critical-bug

# 2. 修复并提交
git commit -m "fix: 紧急修复支付异常"

# 3. 合并到 main 和 develop
git checkout main && git merge hotfix/critical-bug
git checkout develop && git merge hotfix/critical-bug

# 4. 打 tag
git tag -a v1.2.1 -m "hotfix: 支付修复"
```

## `.gitignore` 模板

```gitignore
# 依赖
node_modules/
vendor/
.pnpm-store/

# 构建产物
dist/
build/
*.pyc
__pycache__/

# IDE
.vscode/
.idea/
*.swp

# 环境变量
.env
.env.local

# OS
.DS_Store
Thumbs.db
```

## 总结

好的 Git 习惯能避免很多麻烦：
1. **小步提交**，每次 commit 只做一件事
2. **写清楚的 message**，半年后你还能看懂
3. **pull 前先 commit/stash**，避免冲突丢失
4. **保护 main 分支**，永远通过 PR 合并
