---
title: "欢迎来到我的博客"
date: 2026-06-01 12:00:00 +0800
categories: [博客, 入门]
tags: [Jekyll, Chirpy, GitHub Pages, 博客搭建]
math: true
image:
  path: /assets/img/avatars/avatar.png
  alt: 博客头像
---

## 关于这个博客

你好！这是我在 GitHub Pages 上搭建的个人技术博客，使用 [Jekyll](https://jekyllrb.com/) 和 [Chirpy](https://github.com/cotes2020/jekyll-theme-chirpy) 主题构建。

### 主要特性

- 💻 **代码高亮** — 支持多种编程语言
- 📐 **数学公式** — 基于 MathJax
- 🌙 **暗色模式** — 自动适应系统主题
- 💬 **评论系统** — 基于 Giscus + GitHub Discussions
- 🏷️ **标签分类** — 方便检索和组织内容
- 🔍 **全文搜索** — 快速查找文章

## 代码示例

### Python

```python
def fibonacci(n: int) -> list[int]:
    """生成斐波那契数列"""
    if n <= 0:
        return []
    if n == 1:
        return [0]

    fib = [0, 1]
    for _ in range(2, n):
        fib.append(fib[-1] + fib[-2])
    return fib

# 测试
if __name__ == "__main__":
    result = fibonacci(10)
    print(f"前 10 个斐波那契数: {result}")
    # 输出: [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

### JavaScript/TypeScript

```typescript
// 一个简单的防抖函数
function debounce<T extends (...args: any[]) => any>(
  fn: T,
  delay: number
): (...args: Parameters<T>) => void {
  let timer: ReturnType<typeof setTimeout>;

  return (...args: Parameters<T>) => {
    clearTimeout(timer);
    timer = setTimeout(() => fn(...args), delay);
  };
}

// 使用示例
const handleSearch = debounce((query: string) => {
  console.log(`搜索: ${query}`);
}, 300);
```

### Go

```go
package main

import (
    "fmt"
    "sync"
)

// SafeCounter 是线程安全的计数器
type SafeCounter struct {
    mu    sync.Mutex
    count int
}

func (c *SafeCounter) Increment() {
    c.mu.Lock()
    defer c.mu.Unlock()
    c.count++
}

func (c *SafeCounter) Value() int {
    c.mu.Lock()
    defer c.mu.Unlock()
    return c.count
}

func main() {
    counter := &SafeCounter{}
    var wg sync.WaitGroup

    for i := 0; i < 100; i++ {
        wg.Add(1)
        go func() {
            defer wg.Done()
            counter.Increment()
        }()
    }

    wg.Wait()
    fmt.Printf("计数结果: %d\n", counter.Value())
}
```

### Shell

```bash
#!/bin/bash
# 快速备份脚本

BACKUP_DIR="/tmp/backups"
TIMESTAMP=$(date +"%Y%m%d_%H%M%S")

mkdir -p "$BACKUP_DIR"

# 创建压缩备份
tar -czf "$BACKUP_DIR/backup_$TIMESTAMP.tar.gz" \
    --exclude="node_modules" \
    --exclude=".git" \
    --exclude="*.log" \
    ./project

echo "✅ 备份完成: $BACKUP_DIR/backup_$TIMESTAMP.tar.gz"
```

## 数学公式

Chirpy 主题支持 **MathJax** 渲染数学公式。

### 行内公式

爱因斯坦质能方程：$$E = mc^2$$

### 块级公式

斐波那契数列的闭式表达：

$$
F_n = \frac{1}{\sqrt{5}} \left[ \left( \frac{1 + \sqrt{5}}{2} \right)^n - \left( \frac{1 - \sqrt{5}}{2} \right)^n \right]
$$

贝叶斯定理：

$$
P(A|B) = \frac{P(B|A) \cdot P(A)}{P(B)}
$$

## 文本格式化

> **引用块**: 技术是解决问题的艺术，而不仅仅是工具的堆砌。

- 清晰的**加粗**和*斜体*强调
- 有序列表和无序列表
- [链接](https://github.com/SiconXing) 和图片
- ~~删除线~~ 和 `行内代码`

### 表格

| 特性 | 状态 | 说明 |
|------|:----:|------|
| 代码高亮 | ✅ | Rouge 语法高亮 |
| 数学公式 | ✅ | MathJax 渲染 |
| 暗色模式 | ✅ | 自动/手动切换 |
| 评论系统 | ✅ | Giscus 集成 |
| RSS 订阅 | 🔜 | 即将支持 |

---

感谢阅读！如果你有任何问题或建议，欢迎在评论区留言讨论。未来我会在这里分享更多关于编程、架构设计和工程实践的内容。
