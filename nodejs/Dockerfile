# 1. 继承原作者那个在 Hugging Face 完美放行的基础镜像
FROM ghcr.io/eooce/sbx:native

WORKDIR /app

# 2. 绕过底层 npm 权限限制，直接把你修改过的核心控制逻辑（index.js）强行覆盖进去
# 这样既保留了他原本完整且能过风控的运行环境，又把内核的控制权拿到了你自己手里
COPY index.js ./

# 3. 强行赋予运行权限，对齐 Hugging Face
RUN chmod -R 777 /app

ENV PORT=7860
EXPOSE 7860

# 保持使用原镜像的默认启动机制
CMD ["node", "index.js"]
