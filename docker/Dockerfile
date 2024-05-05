FROM debian:bookworm-slim

COPY ["./installer", "/tmp/installer"]

RUN apt update \
  && chmod u+x ./tmp/installer \
  && ./tmp/installer

COPY ["./php-src", "/usr/local/src/php-src"]
COPY ["./config.nice", "/usr/local/src/php-src/config.nice"]

RUN cd /usr/local/src/php-src \
  && ./buildconf \
  && chmod u+x ./config.nice \
  && ./config.nice \
  && make \
  && make install

COPY ["./vld", "/usr/local/src/vld"]

RUN cd /usr/local/src/vld \
  && phpize \
  && ./configure \
  && make \
  && make install

RUN cd $HOME \
  && curl -LO https://git.io/.gdbinit \
  && cat /usr/local/src/php-src/.gdbinit >> $HOME/.gdbinit

# #15 0.931 No releases available for package "pecl.php.net/ast"
# #15 0.931 install failed
#RUN pecl channel-update pecl.php.net \
#  && pecl install ast

WORKDIR /usr/local/src/php-src
