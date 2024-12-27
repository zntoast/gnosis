dockerfile 
```
# 使用 Golange 镜像作为构建阶段
FROM golang:alpine AS builder

# 设置环境变量
ENV PROJECT_NAME=main

# 设置工作目录
WORKDIR /app

# 复制源代码到工作目录
COPY . .

# 安装依赖并构建应用
RUN go build -o ${PROJECT_NAME} .

# 使用轻量级 Linux 镜像作为运行环境
FROM alpine:latest

# 暴露应用程序端口
EXPOSE 8080

# 设置环境变量
ENV ROOT_DIR=/app \
        PROJECT_NAME=main

WORKDIR /app

# 复制构建阶段的可执行文件到运行阶段
COPY --from=builder /app/${PROJECT_NAME} /app/${PROJECT_NAME}

# 运行应用程序
CMD ["./main"
```