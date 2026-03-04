# Build Stage

FROM node:25 AS build_stage

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

RUN npm run build

# Production Stage

FROM nginx:1.29.5

COPY --from=build_stage /app/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]