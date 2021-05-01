# About Node.js Express

<a target="_blank" rel="noopener noreferrer" href="http://expressjs.com/">
    <img alt="Node.js Logo" align="right" src="/node-express/logo.png" width="100px" />
</a>

[Express](http://expressjs.com/) is a minimal and flexible Node.js web application framework that provides a robust set of features for web and mobile applications. 

## Sample Dockerfile

```Dockerfile
FROM node:12-alpine AS builder

RUN mkdir -p /app
WORKDIR /app

COPY package.json  .
COPY yarn.lock .
RUN yarn install

COPY . .

EXPOSE 80
CMD [ "yarn", "run", "start" ]
```
