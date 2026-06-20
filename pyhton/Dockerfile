# 纯正、轻量的 Python slim 基础镜像
FROM python:3.11-slim

WORKDIR /app

# 1. 补上系统运行和跨网释放可能需要的 curl
RUN apt-get update && apt-get install -y --no-install-recommends curl && rm -rf /var/lib/apt/lists/*

# 2. 复制 Python 的依赖描述文件并安装（如 requests 等）
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt 2>/dev/null || true

# 3. 复制你专属的 python 核心控制脚本
COPY . .

# 4. 🔑 权限全开：提前把整个运行目录的读、写、执行权限全部刷满 (777)
# 这是为了确保脚本在 Hugging Face 运行起来后，有权去动态替换、释放和执行文件
RUN chmod -R 777 /app

ENV PORT=7860
EXPOSE 7860

# 启动 Python 主程序
CMD ["python", "main.py"]
