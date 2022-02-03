FROM docker.io/node:14.16.1

RUN mkdir -p /app
WORKDIR /app

COPY frontend/package.json /app
RUN yarn install --network-timeout 3600000

#CMD ["bash", "-c", "yarn install && yarn run dev"]
ENTRYPOINT ["yarn", "run", "dev"]
