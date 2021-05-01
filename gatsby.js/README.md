# About Gatsby.js

<a target="_blank" rel="noopener noreferrer" href="https://www.gatsbyjs.com">
    <img alt="Gatsby.js Logo" align="right" src="/gatsby.js/logo.svg" width="200px" />
</a>

[Gatsby](https://www.gatsbyjs.com) is a React-based open source framework for creating websites and apps. Kick off your project with this default boilerplate. This starter ships with the main Gatsby configuration files you might need to get up and running blazing fast with the blazing fast app generator for React.

## Sample Dockerfile
```Dockerfile
# Use the full Node image to perform package install
FROM node:12-alpine AS builder
RUN apk add --no-cache \
    autoconf \
    libtool \
    make \
    g++ \
    automake \
    bash \
    libc6-compat \
    libjpeg-turbo-dev \
    libpng-dev \
    nasm

RUN mkdir /app
WORKDIR /app
ENV PATH /app/node_modules/.bin:$PATH

# Copy files required for package install
COPY package.json yarn.lock /app/
RUN yarn --pure-lockfile

# Copy the remaining assets and build the application
COPY . /app/
RUN yarn build

# Copy files from the build stage to the smaller base image
FROM nginx:mainline-alpine
WORKDIR /usr/src/app
COPY --from=builder /app/public /usr/share/nginx/html
EXPOSE 80
```
