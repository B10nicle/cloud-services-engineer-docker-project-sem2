FROM node:16-alpine AS build
WORKDIR /src
COPY package*.json ./
RUN npm ci
COPY . .
ENV VUE_APP_API_URL=/api
RUN npm run build

FROM nginx:stable-alpine
RUN addgroup -g 1001 webgroup && \
    adduser -S -D -u 1001 -G webgroup webuser && \
    chown -R webuser:webgroup /var/cache/nginx && \
    chown -R webuser:webgroup /var/log/nginx && \
    chown -R webuser:webgroup /etc/nginx/conf.d && \
    touch /var/run/nginx.pid && \
    chown -R webuser:webgroup /var/run/nginx.pid && \
    chown -R webuser:webgroup /usr/share/nginx/html

COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=build --chown=webuser:webgroup /src/dist /usr/share/nginx/html/momo-store

EXPOSE 80
USER webuser
CMD ["nginx", "-g", "daemon off;"]
