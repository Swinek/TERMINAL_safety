FROM node:alpine AS build
WORKDIR /src
COPY package*.json .
RUN npm ci
COPY . .
RUN npm run build

FROM nginx:alpine
HEALTHCHECK --interval=30s --timeout=3s --retries=3 \
  CMD wget -q --spider http://localhost/ || exit 1
COPY --from=build /src/dist /usr/share/nginx/html
COPY ./nginx.conf etc/nginx/conf.d/default.conf
EXPOSE 5173
CMD [ "nginx", "-g", "daemon off;"]
