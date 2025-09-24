FROM node:20-alpine AS builder

WORKDIR /app

# 复制 package 文件
COPY package.json pnpm-lock.yaml ./

# 安装依赖
RUN npm install

# 复制源代码并构建
COPY . .
RUN npm run build

# 生产阶段
FROM node:20-alpine
WORKDIR /app

ENV NODE_ENV=production
ENV PORT=3000

# 复制必要的运行文件
COPY --from=builder /app/package.json ./
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public
COPY --from=builder /app/next.config.ts ./

# 安装生产依赖
RUN npm install --only=production

EXPOSE 3000

CMD ["npm", "start"]
