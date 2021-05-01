# About Create React App

<a target="_blank" rel="noopener noreferrer" href="https://github.com/facebook/create-react-app">
    <img alt="React Logo" align="right" src="/create-react-app/logo.png" width="100px" />
</a>

[Create React App](https://github.com/facebook/create-react-app) is a comfortable environment for learning React, and is the best way to start building a new single-page application in React.


## Sample Dockerfile
```Dockerfile
# Use the full Node image to perform package install
FROM node:12-alpine AS builder
RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app
ENV PATH /usr/src/app/node_modules/.bin:$PATH

# Copy files required for package install
COPY package.json  .
RUN yarn install

# Copy the remaining assets and build the application
COPY . /usr/src/app
RUN yarn build

# Copy files from the build stage to the smaller base image
FROM nginx:mainline-alpine
WORKDIR /usr/src/app
RUN apk --no-cache add curl
COPY ./nginx.config /etc/nginx/conf.d/default.conf
COPY --from=builder /usr/src/app/public /usr/share/nginx/html
COPY --from=builder /usr/src/app/build /usr/share/nginx/html
```
