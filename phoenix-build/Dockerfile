FROM erlang:20.0-slim

## APT

RUN apt-get update \
 && DEBIAN_FRONTEND=noninteractive apt-get install -y \
    curl \
    make \
    build-essential \
    sudo \
    mysql-server \
    git-core \
 && rm -rf /var/lib/apt/lists/*

## ELIXIR

ENV ELIXIR_VERSION="v1.4.5" \
  LANG=C.UTF-8

RUN set -xe \
  && ELIXIR_DOWNLOAD_URL="https://github.com/elixir-lang/elixir/releases/download/${ELIXIR_VERSION}/Precompiled.zip" \
  && ELIXIR_DOWNLOAD_SHA256="a740e634e3c68b1477e16d75a0fd400237a46c62ceb5d04551dbc46093a03f98"\
  && buildDeps=' \
    unzip \
  ' \
  && apt-get update \
  && apt-get install -y --no-install-recommends $buildDeps \
  && curl -fSL -o elixir-precompiled.zip $ELIXIR_DOWNLOAD_URL \
  && echo "$ELIXIR_DOWNLOAD_SHA256 elixir-precompiled.zip" | sha256sum -c - \
  && unzip -d /usr/local elixir-precompiled.zip \
  && rm elixir-precompiled.zip \
  && apt-get purge -y --auto-remove $buildDeps \
  && rm -rf /var/lib/apt/lists/*

RUN mix local.hex --force && mix local.rebar --force

## NODE

ENV NODE_VERSION=8.1.2 \
    LANG=C.UTF-8 \
    NPM_CONFIG_LOGLEVEL=info

RUN curl -sL https://deb.nodesource.com/setup_8.x \
    | bash - && sudo apt-get install -y nodejs && rm -rf /var/lib/apt/lists/*

## YARN

RUN curl -o- -L https://yarnpkg.com/install.sh | bash

CMD ["bin/bash"]
