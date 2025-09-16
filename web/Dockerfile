FROM node:22-alpine AS base

# 构建阶段
FROM base AS builder
WORKDIR /app

# 直接复制所有文件
COPY . .

# 启用 pnpm 并安装依赖
RUN corepack enable pnpm \
    && pnpm install \
    && pnpm build

# 生产运行阶段
FROM base AS runner
WORKDIR /app

ENV NODE_ENV=production
ENV PORT=3000
ENV HOSTNAME="0.0.0.0"

# 创建非root用户
RUN addgroup --system --gid 1001 nodejs && \
    adduser --system --uid 1001 nextjs

# 从构建阶段复制必要的文件
COPY --from=builder --chown=nextjs:nodejs /app/package.json ./
COPY --from=builder --chown=nextjs:nodejs /app/.next ./.next
COPY --from=builder --chown=nextjs:nodejs /app/public ./public
COPY --from=builder --chown=nextjs:nodejs /app/node_modules ./node_modules

# 切换到非root用户
USER nextjs

EXPOSE 3000

# 启动应用
CMD ["npm", "start"]
