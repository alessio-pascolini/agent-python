FROM nvidia/cuda:11.8.0-base-ubuntu22.04 as builder

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    build-essential \
    python3 \
    python3-pip

ENV DEB_PYTHON_INSTALL_LAYOUT=deb

WORKDIR /app

RUN pip install --upgrade pip

COPY htpclient/ /app/htpclient/
COPY setup.cfg \
  pyproject.toml \
  README.md \
  LICENSE.txt /app/

RUN pip wheel --no-cache-dir --no-deps \
      --wheel-dir /app/wheel /app

FROM nvidia/cuda:11.8.0-base-ubuntu22.04

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    python3 \
    python-is-python3 \
    pciutils \
    python3-pip \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app
COPY --from=builder /app/wheel/ /app/wheel/
RUN pip install --no-cache /app/wheel/*

CMD ["htpclient", "--help"]
