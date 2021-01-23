FROM node:lts-alpine as builder
ARG TAG=master
RUN apk add --no-cache git python build-base linux-headers udev sudo
WORKDIR /work
RUN npx degit tagyoureit/poolcontroller-manager#${TAG}
RUN npm ci
RUN npm run build

FROM node:lts-alpine
WORKDIR /usr/local/poolcontroller-manager
COPY --from=builder /work/node_modules .
# Consider moving these files to dist folder
COPY --from=builder /work/LICENSE /work/defaultConfig.json /work/package.json ./
COPY --from=builder /work/dist .
EXPOSE 4200
VOLUME [ "/usr/local/poolcontroller-manager/data" ]
ENTRYPOINT ["node", "dist/app.js"]
