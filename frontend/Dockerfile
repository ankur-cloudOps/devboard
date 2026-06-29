# -------------------------
# Stage 1: Build frontend
# -------------------------
FROM node:20-alpine AS build

WORKDIR /app

COPY package*.json ./
RUN npm install --legacy-peer-deps

COPY . .
RUN npm run build


# -------------------------
# Stage 2: Production runtime (SECURE)
# -------------------------
FROM nginx:alpine

# Remove default nginx page
RUN rm -rf /usr/share/nginx/html/*

# Copy only build output
COPY --from=build /app/dist /usr/share/nginx/html

# Security headers (basic hardening)
RUN printf 'server_tokens off;\n' > /etc/nginx/conf.d/security.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
