# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:buster-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
        build-essential \
        m4 \
        && rm -rf /var/lib/apt/lists/*

ADD https://ftp.gnu.org/gnu/mit-scheme/stable.pkg/10.1.10/mit-scheme-10.1.10-x86-64.tar.gz mit-scheme-10.1.10-x86-64.tar.gz
RUN tar -xvf mit-scheme-10.1.10-x86-64.tar.gz
WORKDIR mit-scheme-10.1.10/src
RUN ./configure
RUN make
RUN make install

FROM debian:buster-slim
COPY --from=build /usr/local/ /usr/local/
RUN ln -s mit-scheme /usr/local/bin/scheme-banner
CMD ["scheme-banner"]
