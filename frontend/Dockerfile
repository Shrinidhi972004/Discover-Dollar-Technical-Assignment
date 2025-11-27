# Build stage
FROM node:18-alpine AS builder

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

# build angular app
RUN npm run build -- --configuration=production

# Producton stage
FROM nginx:alpine AS production

RUN rm -rf /usr/share/nginx/html/*

COPY --from=builder /app/dist/angular-15-crud /usr/share/nginx/html

# nginx config
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
    CMD wget --no-verbose --tries=1 --spider http://localhost:80/ || exit 1

CMD ["nginx", "-g", "daemon off;"]
