---
title: "Linux 命令行效率提升指南"
date: 2026-05-20 09:00:00 +0800
categories: [Linux, 工具]
tags: [Linux, Shell, 命令行, 效率, 工具]
pin: false
math: false
image:
  path: /assets/img/avatars/avatar.svg
  alt: Linux
---

## 前言

熟练使用 Linux 命令行可以极大提升开发效率。本文整理了日常工作中最实用的命令和技巧。

## 文件查找

```bash
# find — 强大的文件搜索
find . -name "*.go" -mtime -7          # 7天内修改的 Go 文件
find . -type f -size +10M              # 大于10MB的文件
find . -name "*.log" -exec rm {} \;    # 查找并删除

# fd — 更友好的 find 替代品
fd pattern                    # 递归搜索
fd -e go                      # 按扩展名过滤
fd -x command                 # 对结果执行命令
```

## 文本处理

```bash
# grep — 文本搜索
grep -r "TODO" src/            # 递归搜索
grep -v "debug" log.txt        # 排除匹配行
grep -c "error" *.log          # 计数

# awk — 强大的文本分析器
awk '{print $1, $3}' data.csv                # 打印第1、3列
awk -F',' '$3 > 100' data.csv                # 按条件过滤
awk '{sum+=$2} END {print sum}' data.txt     # 求和

# sed — 流编辑器
sed 's/old/new/g' file.txt     # 全局替换
sed -i 's/foo/bar/' *.txt      # 原地修改
sed -n '10,20p' file.txt       # 打印10-20行
```

## 进程管理

```bash
# htop — 交互式进程查看器（比 top 更好用）
htop                           # 按 CPU/MEM 排序，F9 杀进程

# 后台任务
command &                      # 后台运行
nohup long_task &              # 忽略挂断信号
Ctrl+Z → bg                    # 暂停后恢复后台

# 进程查找
ps aux | grep nginx
pgrep -f "python app.py"
```

## 网络工具

```bash
# 端口查看
lsof -i :8080                  # 谁在用 8080 端口
ss -tlnp                       # 查看监听端口

# HTTP 请求
curl -X POST -H "Content-Type: application/json" \
     -d '{"key":"value"}' https://api.example.com

# 带宽测试
curl -o /dev/null -s -w '%{speed_download}\n' https://example.com/file
```

## Shell 别名与函数

```bash
# ~/.zshrc 或 ~/.bashrc
alias g='git'
alias k='kubectl'
alias ll='ls -alh'
alias ..='cd ..'

# 带参数的函数
extract() {
    if [ -f $1 ]; then
        case $1 in
            *.tar.bz2) tar xjf $1   ;;
            *.tar.gz)  tar xzf $1   ;;
            *.zip)     unzip $1     ;;
            *.rar)     unrar x $1   ;;
            *)         echo "不支持: $1" ;;
        esac
    fi
}
```

## 总结

把常用操作变成习惯，效率自然就上去了。建议：
1. 保持 `.dotfiles` 版本化（放在 GitHub 上）
2. 花时间学习 `awk` / `sed` / `jq` 等文本处理工具
3. 善用 `alias` 和快捷键减少重复输入
