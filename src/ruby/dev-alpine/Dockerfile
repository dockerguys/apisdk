FROM funcapi/ruby:alpine

RUN apk add --no-cache git make \
        libssl1.0 build-base libxml2-dev libxslt-dev libbz2 libevent-dev libffi-dev glib-dev ncurses-dev readline-dev yaml-dev zlib-dev \
        libpq sqlite-dev mariadb-libs postgresql-dev \
        ruby-dev ruby ruby-io-console ruby-bundler ; \
    gem install --no-rdoc --no-ri bundler
