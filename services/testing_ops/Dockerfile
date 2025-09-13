# Stage 1: Build
FROM node:20-alpine AS build

# Install basic build tools for npm native modules
RUN apk add --no-cache python3 make g++

WORKDIR /app

# Copy only package files first for caching
COPY package*.json ./

# Install all dependencies (dev + prod)
RUN npm ci

# Copy the rest of the source code
COPY . .

# Build the app (e.g., NestJS)
RUN npm run build

# Stage 2: Production image
FROM node:20-alpine

WORKDIR /app

# Copy built artifacts
COPY --from=build /app/dist ./dist

# Copy package.json and lock file
COPY --from=build /app/package*.json ./

# Install only production dependencies
RUN npm ci --omit=dev

# Expose the app port
EXPOSE 3000

# Start the application
CMD ["node", "dist/main.js"]
