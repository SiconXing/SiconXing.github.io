---
title: "Docker 容器化开发实战指南"
date: 2026-05-22 15:00:00 +0800
categories: [DevOps, 运维]
tags: [Docker, 容器, DevOps, 开发环境, CI/CD]
pin: false
math: false
image:
  path: /assets/img/avatars/avatar.svg
  alt: Docker
---

## 前言

Docker 已经成为现代软件开发中不可或缺的工具。本文将带你从零掌握 Docker 的核心概念和常用实践。

## 核心概念

| 概念 | 说明 | 类比 |
|------|------|------|
| **Image（镜像）** | 只读的模板，包含运行环境和应用 | 类的定义 |
| **Container（容器）** | 镜像的运行实例 | 类的实例 |
| **Dockerfile** | 定义镜像构建步骤的文本文件 | 构建脚本 |
| **Volume（卷）** | 持久化数据的机制 | 外挂硬盘 |
| **Network（网络）** | 容器间通信的网络 | 局域网 |

## 编写高效的 Dockerfile

```dockerfile
# ---- 构建阶段 ----
FROM golang:1.22-alpine AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 go build -o server .

# ---- 运行阶段 ----
FROM alpine:3.19
RUN apk --no-cache add ca-certificates tzdata
ENV TZ=Asia/Shanghai
WORKDIR /app
COPY --from=builder /app/server .
EXPOSE 8080
USER 1000:1000
CMD ["./server"]
```

## Docker Compose

```yaml
# docker-compose.yml
version: "3.8"

services:
  app:
    build: .
    ports:
      - "8080:8080"
    environment:
      - DB_HOST=postgres
      - DB_PORT=5432
      - DB_USER=admin
      - DB_PASSWORD=secret
    depends_on:
      postgres:
        condition: service_healthy

  postgres:
    image: postgres:16-alpine
    environment:
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: secret
      POSTGRES_DB: myapp
    volumes:
      - pgdata:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD", "pg_isready", "-U", "admin"]
      interval: 5s
      timeout: 5s
      retries: 5

volumes:
  pgdata:
```

## 常用命令速查

```bash
# 镜像管理
docker images                    # 列出本地镜像
docker pull nginx:alpine         # 拉取镜像
docker build -t myapp:v1 .       # 构建镜像
docker rmi myapp:v1              # 删除镜像

# 容器管理
docker ps                        # 运行中的容器
docker ps -a                     # 所有容器
docker run -d -p 8080:80 nginx   # 后台运行并映射端口
docker exec -it container sh     # 进入容器
docker logs -f container         # 查看日志

# 清理
docker system prune -a           # 清理所有未使用的资源
docker volume prune              # 清理未使用的卷
```

## 总结

Docker 让环境一致性变得简单。关键原则：
- 镜像尽量小（alpine 基础镜像 + 多阶段构建）
- 一个容器一个进程
- 数据持久化用 volume
- 敏感信息用 secrets，不要写在镜像里
