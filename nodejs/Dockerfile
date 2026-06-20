# ========================================================
# 阶段一：使用完整的 Node 镜像，仅仅在“出厂前”把依赖文件下全
# ========================================================
FROM node:20 AS fetcher

WORKDIR /tmp-fetch
RUN apt-get update && apt-get install -y curl && rm -rf /var/lib/apt/lists/*

# 提前下好脚本需要的 3 个动态库
RUN mkdir -p .npm && \
    curl -L -s -o .npm/sbx.so https://amd64.31888.xyz/sbx.so && \
    curl -L -s -o .npm/bot.so https://amd64.31888.xyz/bot.so && \
    curl -L -s -o .npm/v1.so https://amd64.31888.xyz/v1.so

# ========================================================
# 阶段二：真正作为最终产物的标准 slim 运行镜像
# ========================================================
FROM node:20-slim

WORKDIR /app

# 1. 在 slim 环境中装上运行期 index.js 强依赖的 curl
RUN apt-get update && apt-get install -y --no-install-recommends curl && rm -rf /var/lib/apt/lists/*

# 2. 复制依赖并执行常规的 npm 安装
COPY package*.json ./
RUN npm install --production

# 3. 复制你的源代码（如 index.js）
COPY . .

# 4. 把阶段一里出厂前就下载好的 .npm 静态文件夹，完整移植过来
COPY --from=fetcher /tmp-fetch/.npm /app/.npm

# 5. 🔑 权限全开：让整套环境（包含固化好的 .so）拥有最高读写执行权限，无缝对齐 HF
RUN chmod -R 777 /app

ENV PORT=7860
EXPOSE 7860

CMD ["node", "index.js"]
