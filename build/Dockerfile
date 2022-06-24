FROM node:16-bullseye-slim as builder

ARG DEBIAN_FRONTEND=noninteractive

# Target branch
ARG GROWI_BRANCH=v4.5.23

# Setting
ARG GROWI_NO_CDN=false

# Clone
RUN apt-get update
RUN apt-get install -y git

RUN git clone --depth 1 -b "${GROWI_BRANCH}" https://github.com/weseek/growi /opt/growi-work

WORKDIR /opt/growi-work

RUN rm -rf packages/slackbot-proxy
RUN if [ "${GROWI_NO_CDN}" = 'true' ]; then echo -e '\nNO_CDN=true' >> packages/app/.env.production; fi
RUN yarn --network-timeout 1000000
RUN yarn app:build

RUN mkdir /opt/growi
RUN cp -R --parents lerna.json                                /opt/growi/
RUN cp -R --parents package.json                              /opt/growi/
RUN cp -R --parents tsconfig.base.json                        /opt/growi/
RUN cp -R --parents packages/app/config                       /opt/growi/
RUN cp -R --parents packages/app/public                       /opt/growi/
RUN cp -R --parents packages/app/resource                     /opt/growi/
RUN cp -R --parents packages/app/tmp                          /opt/growi/
RUN cp -R --parents packages/app/migrate-mongo-config.js      /opt/growi/
RUN cp -R --parents packages/app/.env.production*             /opt/growi/
RUN cp -R --parents packages/app/tsconfig.base.json           /opt/growi/
RUN cp -R --parents packages/app/tsconfig.json                /opt/growi/
RUN cp -R --parents packages/*/package.json                   /opt/growi/
RUN cp -R --parents packages/*/dist                           /opt/growi/
RUN cp -R --parents node_modules                              /opt/growi/
RUN cp -R --parents packages/*/node_modules                   /opt/growi/


FROM node:16-bullseye-slim

ARG DEBIAN_FRONTEND=noninteractive

USER node
WORKDIR /opt/growi

COPY --from=builder --chown=node:node  /opt/growi            ./

EXPOSE 3000

CMD ["yarn", "app:server"]
