#参考
# https://github.com/vercel/next.js/blob/canary/examples/with-docker/Dockerfile

# インストール
FROM node:18-alpine3.17 AS deps

RUN ls
WORKDIR /frontend
COPY package.json package-lock.json ./
RUN npm install --frozen-lockfile

# ビルド
FROM node:18-alpine3.17 AS builder

WORKDIR /frontend

COPY --from=deps /frontend/node_modules ./node_modules
COPY . .

RUN npm run build

# スタート
FROM node:18-alpine3.17 AS runner

# WORKDIR /frontend

ENV NODE_ENV=production

# RUN addgroup --system --gid 1001 nodejs
# RUN adduser --system --uid 1001 nextjs

COPY --from=builder /frontend/public ./public

# RUN mkdir .next
# RUN chown nextjs:nodejs .next

# COPY --from=builder /frontend/next.config.js ./

COPY --from=builder /frontend/next.config.js ./
COPY --from=builder /frontend/.next/standalone ./
COPY --from=builder /frontend/.next/static ./.next/static

# USER nextjs

EXPOSE 3000

ENV PORT 3000

# このコマンドがコンテナ起動時に実行される
CMD ["node", "server.js"]