FROM python:3.12-slim

RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    pkg-config \
    default-libmysqlclient-dev \
    && rm -rf /var/lib/apt/lists/*


COPY mdc_api /data/build/mdc_api
COPY setup.py requirements.txt /data/build

RUN useradd --create-home --home /mdc_api mdc_api && \
		chown -R mdc_api:mdc_api /data

USER mdc_api
WORKDIR /data/build

RUN python3 -m pip install --upgrade pip setuptools wheel
RUN python3 setup.py bdist_wheel 
RUN python3 -m pip install dist/mdc_api*.whl
RUN python3 -m pip uninstall -y dataclasses || true
RUN rm -rf /data/build

WORKDIR /data

EXPOSE 5001
CMD [ "/mdc_api/.local/bin/hupper", "-m", "waitress", "--host", "0.0.0.0", "--port", "5001", "mdc_api:app" ]
