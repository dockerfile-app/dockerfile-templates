# About Next.js

<a target="_blank" rel="noopener noreferrer" href="https://nextjs.org/">
    <img alt="Next.js Logo" align="right" src="/next.js/logo.png" width="100px" />
</a>

[Next.js](https://nextjs.org/) is a React framework developed by Vercel supporting hybrid static & server rendering, TypeScript support, smart bundling and route pre-fetching.

## Sample Dockerfile
```Dockerfile
FROM node:12-alpine AS builder
WORKDIR /app
COPY package.json package.json
RUN yarn install
COPY . .
RUN yarn build && yarn --production

FROM node:12-alpine
WORKDIR /app
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/public ./public
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/next.config.js ./next.config.js

EXPOSE 3000
CMD ["node_modules/.bin/next", "start"]
```
