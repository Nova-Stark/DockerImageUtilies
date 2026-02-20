# Builder stage
FROM debian:bookworm-slim AS builder

# Fixed: Use key=value format to avoid Docker warnings
ENV PATH=/usr/local/bin:$PATH \
    PYTHON_VERSION=3.13.0

ARG DEBIAN_FRONTEND=noninteractive 

RUN --mount=type=cache,target=/var/cache/apt,sharing=locked \
    --mount=type=cache,target=/var/lib/apt,sharing=locked \
    set -eux; \
    apt-get update; \
    apt-get install -y --no-install-recommends \
        ca-certificates dpkg-dev gcc gnupg libbluetooth-dev libbz2-dev \
        libc6-dev libdb-dev libexpat1-dev libffi-dev libgdbm-dev \
        liblzma-dev libncursesw5-dev libreadline-dev libsqlite3-dev \
        libssl-dev make tk-dev uuid-dev wget xz-utils zlib1g-dev

WORKDIR /usr/src/python

RUN set -eux; \
    wget -O python.tar.xz "https://www.python.org/ftp/python/${PYTHON_VERSION%%[a-z]*}/Python-$PYTHON_VERSION.tar.xz"; \
    tar -xJC /usr/src/python --strip-components=1 -f python.tar.xz; \
    rm python.tar.xz; \
    \
    ./configure \
        --enable-loadable-sqlite-extensions \
        --enable-optimizations \
        --enable-shared \
        --with-lto \
        --with-ensurepip \
        --disable-gil; \
    \
    nproc=$(nproc); \
    make -j "$nproc" LDFLAGS="-Wl,--strip-all -Wl,-rpath='\$ORIGIN/../lib'"; \
    make install; \
    \
    find /usr/local -depth \
        \( \( -type d -a \( -name test -o -name tests -o -name idle_test \) \) \
        -o \( -type f -a \( -name '*.pyc' -o -name '*.pyo' -o -name 'libpython*.a' \) \) \) \
        -exec rm -rf '{}' +

# RUntime
FROM debian:bookworm-slim

RUN groupadd -g 999 python && useradd -r -u 999 -g python python

ENV PATH=/usr/local/bin:$PATH \
    PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

RUN --mount=type=cache,target=/var/cache/apt,sharing=locked \
    --mount=type=cache,target=/var/lib/apt,sharing=locked \
    apt-get update; \
    apt-get install -y --no-install-recommends \
        libsqlite3-0 \
        libssl3 \
        ca-certificates; \
    ldconfig

COPY --from=builder /usr/local /usr/local

RUN set -eux; \
    for src in idle3 pip3 pydoc3 python3 python3-config; do \
        dst="$(echo "$src" | tr -d 3)"; \
        [ -s "/usr/local/bin/$src" ] && ln -svT "$src" "/usr/local/bin/$dst"; \
    done
RUN echo "/usr/local/lib" > /etc/ld.so.conf.d/python.conf && ldconfig
USER python
CMD ["python3"]

# Hope this wont break your app ... :)