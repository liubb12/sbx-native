# ========================================================
# 阶段一：用官方正版官方二进制拉取
# ========================================================
FROM ghcr.io/sagernet/sing-box:latest AS sb-bin
FROM cloudflare/cloudflared:latest AS cf-bin

# ========================================================
# 阶段二：使用纯正的 node:20-slim 作为最终运行环境
# ========================================================
FROM node:20-slim

WORKDIR /app

# 1. 补上运行期可能需要的工具
RUN apt-get update && apt-get install -y --no-install-recommends curl ca-certificates && rm -rf /var/lib/apt/lists/*

# 2. 从第一阶段，把官方原生编译好的、绝对安全的官方二进制程序复制进来
COPY --from=sb-bin /usr/local/bin/sing-box /usr/local/bin/sing-box
COPY --from=cf-bin /usr/local/bin/cloudflared /usr/local/bin/cloudflared

# 3. 复制你的代码和项目配置文件（如果你有需要的话）
COPY package*.json ./
RUN npm install --production
COPY . .

# 4. 创建一个开箱即用的启动脚本，把原先 index.js 里繁琐的功能用原生二进制拉起
RUN echo '#!/bin/sh' > /app/start.sh && \
    echo 'echo "Starting official native cores in slim environment..."' >> /app/start.sh && \
    echo '# 在后台静默安全地拉起原生的云穿透核心' >> /app/start.sh && \
    echo 'cloudflared tunnel --url http://localhost:7860 --no-autoupdate > /dev/null 2>&1 &' >> /app/start.sh && \
    echo '# 用 Node.js 只来跑你的 HTTP 网页和订阅分发（安全合规，绝不越权调用）' >> /app/start.sh && \
    echo 'exec node index.js' >> /app/start.sh && \
    chmod +x /app/start.sh

ENV PORT=7860
EXPOSE 7860

# 5. 使用安全启动脚本启动容器
ENTRYPOINT ["/bin/sh", "/app/start.sh"]
