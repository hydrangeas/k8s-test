# ==================================================
# パッケージのインストールのレイヤー
# ==================================================
FROM node:18-alpine AS deps
WORKDIR /base

COPY package.json package-lock.json ./
RUN  npm install --production


# ==================================================
# アプリのビルドレイヤー
# ==================================================
FROM node:18-alpine AS builder
WORKDIR /build
COPY --from=deps /base/node_modules ./node_modules

COPY ./app /build/app
COPY ./public /build/public
COPY package.json ./

# buildは、tsからjsに変換するのでこれがないとエラーになる
COPY tsconfig.json .

# nextのbuildの設定ルール
COPY next.config.js .

# tailwindの設定ルール
COPY tailwind.config.ts .

# envファイルの設定
COPY .env.local .env

RUN npm run build


# ==================================================
# アプリの実行レイヤー
# ==================================================
FROM node:18-alpine AS runner
#
WORKDIR /app

COPY --from=builder /build/.next ./.next
COPY --from=builder /build/node_modules ./node_modules
COPY --from=builder /build/package.json ./package.json
COPY --from=builder /build/public ./public

CMD ["npm", "start"]
EXPOSE 3000