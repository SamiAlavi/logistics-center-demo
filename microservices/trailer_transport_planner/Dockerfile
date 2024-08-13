FROM python:3.10.10-alpine3.17 AS base
RUN apk add curl

WORKDIR /app
COPY requirements.txt .
# RUN  pip3 install --extra-index-url https://alpine-wheels.github.io/index numpy
RUN pip install -r requirements.txt
COPY src src
EXPOSE 9100:9100
WORKDIR /app/src

FROM base AS prod
ENTRYPOINT ["python", "service.py"]
HEALTHCHECK --interval=30s --timeout=30s --start-period=30s --retries=3 CMD curl -f http://localhost:9100/health || exit 1

FROM base AS dev
RUN pip install debugpy
ENV PYTHONUNBUFFERED 1
EXPOSE 5681:5681
ENTRYPOINT ["python", "-m", "debugpy", "--listen", "0.0.0.0:5681", "service.py"]
HEALTHCHECK --interval=30s --timeout=30s --start-period=30s --retries=3 CMD curl -f http://localhost:9100/health || exit 1

