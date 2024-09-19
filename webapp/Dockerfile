FROM node:16 AS build
RUN mkdir /frontend
WORKDIR /frontend
COPY . /frontend
RUN npm install
RUN npm run build

FROM nginx
COPY --from=build /frontend/dist /usr/share/nginx/html

