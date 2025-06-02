# Build Stage
FROM node:18-alpine AS builder
RUN corepack enable && corepack prepare pnpm@latest --activate
WORKDIR /app
COPY package*.json pnpm-lock.yaml ./
# RUN npm ci --legacy-peer-deps
RUN pnpm install --frozen-lockfile

COPY . .
# COPY .env .env

RUN npm run build

# Production Stage
FROM node:18-alpine
RUN corepack enable && corepack prepare pnpm@latest --activate
WORKDIR /app
COPY --from=builder /app/.next .next
COPY --from=builder /app/public public
COPY --from=builder /app/package*.json /app/pnpm-lock.yaml ./
# RUN npm ci --legacy-peer-deps --omit=dev
RUN pnpm install --frozen-lockfile --prod

EXPOSE 3000
# CMD ["npm", "start"]
CMD ["pnpm", "start"]
