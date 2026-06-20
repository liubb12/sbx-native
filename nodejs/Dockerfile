# 使用轻量级的 Node.js slim 基础镜像
FROM node:20-slim

# 设置容器内的工作目录
WORKDIR /app

# 复制依赖配置文件并安装依赖
COPY package*.json ./
RUN npm install --production

# 复制剩下的所有源代码到容器中
COPY . .

# 如果你的 index.js 监听了某个端口（例如 3000），可以写上，不写也行
EXPOSE 3000

# 启动命令
CMD ["node", "index.js"]
