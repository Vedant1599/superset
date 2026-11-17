# Stock Superset backend + Genuin frontend bundles + drivers.
# Build context = superset/static/assets (output of `npm run build`).
ARG SUPERSET_VERSION=4.1.1
FROM apachesuperset.docker.scarf.sh/apache/superset:${SUPERSET_VERSION}

ARG TARGETARCH
ARG GECKODRIVER_VERSION=0.34.0

USER root

# Firefox + geckodriver for alert/report screenshots (was installed on every container start)
RUN apt-get update \
 && apt-get install -y --no-install-recommends firefox-esr wget \
 && case "${TARGETARCH}" in \
      amd64) GD_ARCH=linux64 ;; \
      arm64) GD_ARCH=linux-aarch64 ;; \
      *) echo "unsupported arch ${TARGETARCH}" && exit 1 ;; \
    esac \
 && wget -q -O /tmp/geckodriver.tar.gz \
      "https://github.com/mozilla/geckodriver/releases/download/v${GECKODRIVER_VERSION}/geckodriver-v${GECKODRIVER_VERSION}-${GD_ARCH}.tar.gz" \
 && tar -xzf /tmp/geckodriver.tar.gz -C /usr/bin geckodriver \
 && chmod 755 /usr/bin/geckodriver \
 && rm /tmp/geckodriver.tar.gz \
 && rm -rf /var/lib/apt/lists/*

RUN pip install --no-cache-dir redis flask_cors clickhouse-connect firebolt-sqlalchemy elasticsearch-dbapi

# Our compiled frontend (manifest.json + hashed bundles) over the stock ones
COPY . /app/superset/static/assets/

USER superset
