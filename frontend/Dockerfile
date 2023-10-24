#参考
# https://github.com/vercel/next.js/blob/canary/examples/with-docker/Dockerfile

#マルチステージビルド

# node_modulesインストール
FROM node:18-alpine3.17 AS deps

WORKDIR /frontend
COPY ./frontend/package.json ./frontend/package-lock.json ./
RUN npm install --frozen-lockfile

# ビルド
FROM node:18-alpine3.17 AS builder

WORKDIR /frontend

COPY --from=deps /frontend/node_modules ./node_modules
# ローカルのfrontendディレクトリ直下のソースコードを全部、コンテナにコピー
COPY /frontend .

RUN npm run build

# スタート
FROM node:18-alpine3.17 AS runner

ENV NODE_ENV=production

COPY --from=builder /frontend/public ./public
COPY --from=builder /frontend/next.config.js ./
COPY --from=builder /frontend/.next/standalone ./
COPY --from=builder /frontend/.next/static ./.next/static

# コンテナのポートを公開
EXPOSE 3000

# ENV PORT 3000

# このコマンドがコンテナ起動時に実行される
CMD ["node", "server.js"]