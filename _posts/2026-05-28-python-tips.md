---
title: "Python 实用技巧：写出更优雅的代码"
date: 2026-05-28 20:00:00 +0800
categories: [Python, 编程]
tags: [Python, 代码优化, 最佳实践, 技巧]
pin: true
math: false
image:
  path: /assets/img/avatars/avatar.svg
  alt: Python
---

## 前言

Python 以简洁优雅著称，但真正写出地道的 Python 代码需要掌握一些技巧。本文整理了几个在日常开发中非常实用的 Python 写法。

## 1. 使用 `dataclass` 简化数据类

```python
from dataclasses import dataclass, field
from typing import List

@dataclass
class User:
    name: str
    age: int
    email: str = ""
    tags: List[str] = field(default_factory=list)
    is_active: bool = True

# 自动生成 __init__, __repr__, __eq__
user = User(name="Alice", age=28, tags=["python", "backend"])
print(user)  # User(name='Alice', age=28, email='', tags=['python', 'backend'], is_active=True)
```

## 2. 上下文管理器 (`with`)

```python
from contextlib import contextmanager
import time

@contextmanager
def timer(name: str = ""):
    """一个简单的计时器上下文管理器"""
    start = time.perf_counter()
    yield
    elapsed = time.perf_counter() - start
    print(f"{name} 耗时: {elapsed:.3f}s" if name else f"耗时: {elapsed:.3f}s")

# 使用
with timer("数据处理"):
    time.sleep(1.5)
# 输出: 数据处理 耗时: 1.500s
```

## 3. 类型提示

```python
from typing import Optional, Union, Callable, TypeVar

T = TypeVar("T")

def find_first(predicate: Callable[[T], bool], items: list[T]) -> Optional[T]:
    """返回第一个满足条件的元素"""
    return next((item for item in items if predicate(item)), None)

# IDE 可以正确推断返回类型
users = [User("Bob", 25), User("Alice", 28)]
result = find_first(lambda u: u.age > 26, users)
print(result.name if result else "未找到")  # Alice
```

## 4. `match` 语句 (Python 3.10+)

```python
def handle_command(command: str):
    match command.split():
        case ["get", item]:
            return f"获取 {item}"
        case ["create", item, *options]:
            return f"创建 {item}，选项: {options}"
        case ["delete", item]:
            return f"删除 {item}"
        case _:
            return "未知命令"

print(handle_command("get user"))        # 获取 user
print(handle_command("create post draft private"))  # 创建 post，选项: ['draft', 'private']
```

## 总结

这些技巧可以让你写出更简洁、更安全的 Python 代码。最重要的是保持一致性——选择一个风格并坚持下去。
