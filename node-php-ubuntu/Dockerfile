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
    && curl -sL https://deb.nodesource.com/setup_14.x | bash -

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

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \ 
    && php -r "if (hash_file('sha384', 'composer-setup.php') === 'c31c1e292ad7be5f49291169c0ac8f683499edddcfd4e42232982d0fd193004208a58ff6f353fde0012d35fdd72bc394') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \ 
    && php composer-setup.php --install-dir=/usr/bin --filename=composer \
    && chmod +x /usr/bin/composer \
    && rm composer-setup.php