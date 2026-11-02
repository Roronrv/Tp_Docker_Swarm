FROM node:22-alpine AS build
WORKDIR /app
COPY package.json yarn.lock ./
RUN yarn install --frozen-lockfile
COPY tsconfig.json ./
COPY src ./src
RUN yarn build

FROM node:22-alpine AS deps
WORKDIR /app
COPY package.json yarn.lock ./
RUN npm pkg delete devDependencies \
 && yarn install --frozen-lockfile --production \
 && yarn cache clean

FROM node:22-alpine AS main
WORKDIR /app
ENV NODE_ENV=production
COPY package.json ./
COPY --from=deps /app/node_modules ./node_modules
COPY --from=build /app/dist ./dist
EXPOSE 3000
USER node
CMD ["node", "dist/index.js"]