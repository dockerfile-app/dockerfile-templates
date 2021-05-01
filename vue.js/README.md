# About Vue.js

<a target="_blank" rel="noopener noreferrer" href="https://vuejs.org/">
    <img alt="Vue.js Logo" align="right" src="/vue.js/logo.png" width="100px" />
</a>

[Vue.js ](https://vuejs.org/) is an open-source JavaScript framework for building user interfaces and single-page applications. It was created by Evan You, and is maintained by him and the rest of the active core team members coming from various companies such as Netlify and Netguru.

## Sample Dockerfile

```Dockerfile
# build stage
FROM node:lts-alpine as build-stage
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# production stage
FROM nginx:stable-alpine as production-stage
COPY --from=build-stage /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```
