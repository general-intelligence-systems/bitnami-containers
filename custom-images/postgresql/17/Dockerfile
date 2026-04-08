FROM ghcr.io/general-intelligence-systems/postgresql:17.6.0-debian-12-r4
USER root

# ── pg_partman (compile from source) ──────────────────────────────
RUN install_packages gcc make curl libc6-dev && \
    curl --retry 3 --retry-delay 5 -SsLf -o /tmp/pg_partman.tar.gz \
      https://github.com/pgpartman/pg_partman/archive/refs/tags/v5.2.4.tar.gz && \
    tar xzf /tmp/pg_partman.tar.gz -C /tmp && \
    cd /tmp/pg_partman-5.2.4 && make && make install && \
    cd / && rm -rf /tmp/pg_partman-* /tmp/pg_partman.tar.gz

# ── pgvector (compile from source) ────────────────────────────────
RUN curl --retry 3 --retry-delay 5 -SsLf -o /tmp/pgvector.tar.gz \
      https://github.com/pgvector/pgvector/archive/refs/tags/v0.8.2.tar.gz && \
    tar xzf /tmp/pgvector.tar.gz -C /tmp && \
    cd /tmp/pgvector-0.8.2 && make && make install && \
    cd / && rm -rf /tmp/pgvector-* /tmp/pgvector.tar.gz

# ── pg_search / ParadeDB (prebuilt .deb, relocated to Bitnami paths) ─
# The .deb installs to PGDG paths (/usr/lib/postgresql/17/...),
# so we extract and move files to /opt/bitnami/postgresql/.
RUN curl --retry 3 --retry-delay 5 -SsLf -o /tmp/pg_search.deb \
      https://github.com/paradedb/paradedb/releases/download/v0.22.5/postgresql-17-pg-search_0.22.5-1PARADEDB-bookworm_amd64.deb && \
    dpkg-deb -x /tmp/pg_search.deb /tmp/pg_search && \
    cp /tmp/pg_search/usr/lib/postgresql/17/lib/*.so /opt/bitnami/postgresql/lib/ && \
    cp /tmp/pg_search/usr/share/postgresql/17/extension/pg_search* /opt/bitnami/postgresql/share/extension/ && \
    rm -rf /tmp/pg_search /tmp/pg_search.deb

# ── cleanup build deps ────────────────────────────────────────────
RUN uninstall_packages gcc make curl libc6-dev

USER 1001
