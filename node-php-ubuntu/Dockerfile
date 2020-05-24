FROM php:7.3-cli

ENV TZ=UTC

# dont use ENV here to prevent 'cannot change locale' errors
RUN export LC_ALL=en_US.UTF-8 \
    export DEBIAN_FRONTEND=noninteractive

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get -qq update

# install common unix tools
RUN apt-get install --no-install-recommends -y \
    software-properties-common \
    curl \
    rsync \
    openssh-client \
    git \
    unzip \
    openssl \
    libssl-dev \
    gnupg2

# add apt-get repositories
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
    && echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list \
    && curl -sL https://deb.nodesource.com/setup_12.x | bash -

# install nodejs tools
RUN apt-get install --no-install-recommends -y \
    nodejs \
    yarn
RUN yarn global add cross-env

# install php tools
# default extensions:
# ctype,  curl,  date,  dom,  fileinfo,  filter,  ftp,  hash,  iconv,  json,  libxml,  mbstring,  mysqlnd,  openssl,
# pcre,  pdo,  pdo_sqlite, phar,  posix,  readline,  Reflection,  session,  simplexml,  sodium,  sqlite3,  tokenizer,
# xml,  xmlreader,  xmlwriter,  zlib
ADD https://raw.githubusercontent.com/mlocati/docker-php-extension-installer/master/install-php-extensions /usr/local/bin/

RUN chmod uga+x /usr/local/bin/install-php-extensions && sync && \
    install-php-extensions \
    zip \
    intl \
    calendar \
    curl

RUN apt-get autoremove && apt-get clean && rm -rf /var/lib/apt/lists/*

