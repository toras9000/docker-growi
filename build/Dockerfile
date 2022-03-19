FROM node:16-bullseye-slim as cloner

ARG DEBIAN_FRONTEND=noninteractive

# Target branch
ARG GROWI_BRANCH=master

# Clone
RUN apt-get update
RUN apt-get install -y git
RUN git clone --depth 1 -b "${GROWI_BRANCH}" https://github.com/weseek/growi /opt/growi
RUN rm -rf /opt/growi/.git


FROM node:16-bullseye-slim

ARG DEBIAN_FRONTEND=noninteractive

# Copy source
COPY --from=cloner  /opt/growi  /opt/growi

# Build
RUN set -x \
 && cd /opt/growi \
 && yarn --network-timeout 1000000 \
 && yarn app:build

# work dir
WORKDIR /opt/growi

# Service port
EXPOSE 3000

# Startup command
CMD ["yarn", "app:server"]
