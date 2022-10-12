# FROM node:14.19.3-bullseye-slim
FROM arm64v8/node:14

LABEL maintainer="buildmaster@rocket.chat"

# dependencies
RUN groupadd -g 65533 -r rocketchat \
    && useradd -u 65533 -r -g rocketchat rocketchat \
    && mkdir -p /app/uploads \
    && chown rocketchat:rocketchat /app/uploads \
    && apt-get update \
    && apt-get install -y --no-install-recommends fontconfig

# --chown requires Docker 17.12 and works only on Linux
ADD --chown=rocketchat:rocketchat . /app

RUN aptMark="$(apt-mark showmanual)" \
    && apt-get install -y --no-install-recommends g++ make python ca-certificates \
    && cd /app/bundle/programs/server \
    && npm install \
    && apt-mark auto '.*' > /dev/null \
    && apt-mark manual $aptMark > /dev/null \
    && find /usr/local -type f -executable -exec ldd '{}' ';' \
    | awk '/=>/ { print $(NF-1) }' \
    | sort -u \
    | xargs -r dpkg-query --search \
    | cut -d: -f1 \
    | sort -u \
    | xargs -r apt-mark manual \
    && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false \
    # 
    # Start hack for sharp...
    && rm -rf npm/node_modules/sharp \
    # && rm -rf npm/node_modules/grpc \
    && npm install sharp@0.22.1 \
    # && npm install grpc@1.12.2 \
    && mv node_modules/sharp npm/node_modules/sharp \
    # && mv node_modules/grpc npm/node_modules/grpc \
    # End hack for sharp
    && cd npm \
    && npm rebuild bcrypt --build-from-source \
    # 
    && npm cache clear --force

USER rocketchat

VOLUME /app/uploads

WORKDIR /app/bundle

# needs a mongoinstance - defaults to container linking with alias 'mongo'
ENV DEPLOY_METHOD=docker-arm64 \
    NODE_ENV=production \
    MONGO_URL=mongodb://mongo:27017/rocketchat \
    HOME=/tmp \
    PORT=3000 \
    ROOT_URL=http://localhost:3000 \
    Accounts_AvatarStorePath=/app/uploads

EXPOSE 3000

CMD ["node", "main.js"]
