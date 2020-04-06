# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:buster-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
      build-essential \
 && rm -rf /var/lib/apt/lists/*
WORKDIR /build
COPY checksum checksum
ADD http://synthcode.com/scheme/chibi/chibi-scheme-0.8.0.tgz chibi.tgz
RUN sha256sum chibi.tar.gz && sha256sum -c checksum
RUN mkdir chibi && tar -C chibi --strip-components 1 -xf chibi.tar.gz
WORKDIR /build/chibi
RUN make
RUN make install

FROM debian:buster-slim
COPY --from=build /usr/local/ /usr/local/
COPY scheme-banner /usr/local/bin/
COPY scheme-r7rs /usr/local/bin/
RUN ldconfig
CMD ["scheme-banner"]
