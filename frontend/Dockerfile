FROM node:20-alpine AS base
WORKDIR /app

COPY package*.json ./
COPY backend/package*.json ./backend/
COPY frontend/package*.json ./frontend/
RUN npm ci

COPY frontend/ ./frontend/
WORKDIR /app/frontend
EXPOSE 3000

FROM base AS dev
CMD ["../node_modules/.bin/next", "dev"]

FROM base AS prod
ENV NODE_ENV=production
RUN ../node_modules/.bin/next build
CMD ["../node_modules/.bin/next", "start", "-p", "3000"]
