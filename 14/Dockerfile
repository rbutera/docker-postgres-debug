FROM postgres:14-alpine AS base

FROM base AS builder
RUN apk add --update git make openssl-dev clang llvm9 g++
WORKDIR /tmp
RUN git clone git://git.postgresql.org/git/pldebugger.git
WORKDIR /tmp/pldebugger
ENV USE_PGXS=1
RUN make
RUN make install

FROM base AS dist
COPY --from=builder /usr/local/lib/postgresql/ /usr/local/lib/postgresql/
COPY --from=builder /usr/local/share/postgresql/ /usr/local/share/postgresql/
COPY --from=builder /usr/local/share/doc/postgresql/ /usr/local/share/doc/postgresql/
COPY debugger-setup.sh /docker-entrypoint-initdb.d/
