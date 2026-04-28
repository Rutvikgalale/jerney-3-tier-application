FROM node:20-alpine AS build
WORKDIR /app
COPY package.json package-lock* ./
# dependencies related to production only
RUN npm ci --only-production && npm cache clean --force

# no need to build backend image because backend is very lightweight
FROM node:20-alpine AS production

# in node:20-alpine user is not present so we need to create user
# In Alpine Linux -S means system
# It tells the OS to create a system user or system group instead of a normal login user.
# running container as non root user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

RUN apk --no-cache add dumb-init

WORKDIR /app

COPY --from=build /app/node_modules ./node_modules

COPY src/ ./src/

USER appuser

EXPOSE 5000

ENTRYPOINT ["dumb-init", "--"]
CMD ["node", "src/index.js"]
