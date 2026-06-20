# 使用轻量级 Node.js slim 基础镜像
FROM node:20-slim

# 设置容器内的工作目录
WORKDIR /app

# 关键修复：更新 Debian 软件源，并安装此脚本极度依赖的 curl 工具
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    && rm -rf /var/lib/apt/lists/*

# 复制依赖配置文件并安装依赖
COPY package*.json ./
RUN npm install --production

# 复制剩下的所有源代码到容器中
COPY . .

# 暴露端口（Hugging Face 强制 7860）
EXPOSE 7860

# 启动命令
CMD ["node", "index.js"]
