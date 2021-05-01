# About Angular.js

<a target="_blank" rel="noopener noreferrer" href="https://angularjs.org">
    <img alt="Angular.js Logo" align="right" src="/angular.js/logo.png" width="100px" />
</a>

[AngularJS](https://angularjs.org) is a JavaScript-based open-source front-end web framework mainly maintained by Google and by a community of individuals and corporations to address many of the challenges encountered in developing single-page applications.

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
COPY --from=build-stage /app/dist/example /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```
