FROM ghcr.io/general-intelligence-systems/postgresql:17.6.0-debian-12-r4
USER root
RUN install_packages gcc make curl libc6-dev && \
    curl -SsLf https://github.com/pgpartman/pg_partman/archive/refs/tags/v5.2.4.tar.gz \
      | tar xz -C /tmp && \
    cd /tmp/pg_partman-5.2.4 && make && make install && \
    cd / && rm -rf /tmp/pg_partman-* && \
    uninstall_packages gcc make curl libc6-dev
USER 1001
