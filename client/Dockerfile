FROM node:18-alpine AS deps
RUN apk add --no-cache libc6-compat
WORKDIR /usr/src/app

COPY package.json package-lock.json ./
RUN  npm install --production

FROM node:18-alpine AS builder
WORKDIR /usr/src/app
COPY --from=deps /usr/src/app/node_modules ./node_modules
COPY . .

ENV NEXT_TELEMETRY_DISABLED 1

RUN npm run build

FROM node:18-alpine AS runner
WORKDIR /usr/src/app

ENV NODE_ENV production
ENV NEXT_TELEMETRY_DISABLED 1

COPY --from=builder --chown=node:node /usr/src/app/.next ./.next
COPY --from=builder --chown=node:node /usr/src/app/node_modules ./node_modules
COPY --from=builder --chown=node:node /usr/src/app/package.json ./package.json

EXPOSE 3000

ENV PORT 3000

CMD ["npm", "start"]
