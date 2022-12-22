FROM node:18-alpine as ts-build

WORKDIR /usr/src/app
COPY . .

RUN yarn install
RUN yarn run build

#################################

#FROM node:18-alpine as deps

#WORKDIR /usr/src/app
#COPY . .

#RUN yarn install

#################################

FROM alpine as prod

RUN apk add --no-cache nodejs

ARG APP_ENV=${APP_ENV:-development}
ARG PORT=${PORT:-80}

ENV APP_ENV=${APP_ENV} \
    PORT=${PORT}

RUN mkdir -p /usr/src/app

WORKDIR /usr/src/app
COPY . .
COPY --from=ts-build /usr/src/app/dist ./dist
COPY --from=ts-build /usr/src/app/node_modules ./node_modules

EXPOSE ${PORT}
CMD [ "node", "dist/src/main.js" ]
