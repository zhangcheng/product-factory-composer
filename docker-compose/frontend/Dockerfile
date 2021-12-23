FROM docker.io/node:14.16.1
#FROM node:15.5.0-buster

RUN mkdir -p /app
WORKDIR /app

COPY frontend/package.json /app
RUN yarn install

#CMD ["bash", "-c", "yarn install && yarn run dev"]
ENTRYPOINT ["yarn", "run", "dev"]
