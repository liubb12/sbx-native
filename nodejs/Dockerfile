# 核心精髓：直接继承原作者那个能完美通过风控、完美适配 koffi 环境的镜像
FROM ghcr.io/eooce/sbx:native

WORKDIR /app

# 1. 强行清除原作者残留在容器里的旧配置和旧脚本
RUN rm -f index.js package.json package-lock.json 2>/dev/null || true

# 2. 复制你自己修改过的 package.json（包含你自己的 UUID、哪吒配置等默认变量描述）
COPY package*.json ./

# 3. 重新安装依赖（确保纯净）
RUN npm install --production

# 4. 复制你自己的 index.js 脚本到容器中，全面接管控制权
COPY . .

# 5. 再次把目录权限刷满，对齐普通用户
RUN chmod -R 777 /app

ENV PORT=7860
EXPOSE 7860

# 启动你自己的脚本逻辑
CMD ["node", "index.js"]
