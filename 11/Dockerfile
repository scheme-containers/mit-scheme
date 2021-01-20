# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:buster-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
      build-essential \
      m4 \
 && rm -rf /var/lib/apt/lists/*

WORKDIR /build
COPY checksum checksum
ADD https://ftp.gnu.org/gnu/mit-scheme/testing.pkg/11.0.90/mit-scheme-11.0.90-x86-64.tar.gz mit-scheme.tar.gz
RUN sha256sum mit-scheme.tar.gz && sha256sum -c checksum
RUN mkdir mit-scheme && tar -C mit-scheme --strip-components 1 -xf mit-scheme.tar.gz
WORKDIR /build/mit-scheme/src
COPY werror.patch werror.patch
RUN patch -p 0 <werror.patch
RUN ./configure
RUN make
RUN make install

FROM debian:buster-slim
COPY --from=build /usr/local/ /usr/local/
RUN ln -s mit-scheme /usr/local/bin/scheme-banner
CMD ["scheme-banner"]
