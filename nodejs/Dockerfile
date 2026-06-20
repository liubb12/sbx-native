# 使用纯正、轻量的 node:20-slim 作为基础运行环境
FROM node:20-slim

# 设置容器内工作目录
WORKDIR /app

# 1. 复制依赖描述文件
COPY package*.json ./

# 2. 安装项目运行所需的纯纯的 JS/TS 生产环境依赖
RUN npm install --production

# 3. 复制剩余的所有 Node.js 源代码（包括你的纯 JS 逻辑文件）
COPY . .

# 4. 🔑 权限全开：让整套纯代码环境拥有最高读写执行权限，无缝对齐 HF
RUN chmod -R 777 /app

# 5. 强行指定 Hugging Face 的放行端口
ENV PORT=7860
EXPOSE 7860

# 纯粹用 Node 启动你的主脚本
CMD ["node", "index.js"]
