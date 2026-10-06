FROM node:20-alpine AS base
WORKDIR /app

COPY package*.json ./
COPY backend/package*.json ./backend/
COPY frontend/package*.json ./frontend/
RUN npm ci

COPY backend/ ./backend/

ARG DATABASE_URL=postgresql://dummy:dummy@localhost:5432/dummy
ENV DATABASE_URL=$DATABASE_URL
RUN npx prisma generate --schema=./backend/prisma/schema.prisma

WORKDIR /app/backend
EXPOSE 3001

FROM base AS dev
CMD sh -c "npx prisma generate --schema=./prisma/schema.prisma && npx prisma migrate deploy && ../node_modules/.bin/nest start --watch"

FROM base AS prod
ENV NODE_ENV=production
RUN ../node_modules/.bin/nest build
CMD ["sh", "-c", "npx prisma migrate deploy && node dist/src/main"]
