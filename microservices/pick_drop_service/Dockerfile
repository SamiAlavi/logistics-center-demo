FROM python:3.10.10-alpine3.17 AS base
WORKDIR /app
COPY requirements.txt .
# RUN  pip3 install --extra-index-url https://alpine-wheels.github.io/index numpy
RUN pip install -r requirements.txt
COPY src src
EXPOSE 9200:9200
WORKDIR /app/src

FROM base AS prod
ENTRYPOINT ["python", "service.py"]
HEALTHCHECK --interval=30s --timeout=30s --start-period=30s --retries=3 CMD curl -f http://localhost:9200/health || exit 1

FROM base AS dev
RUN pip install debugpy
ENV PYTHONUNBUFFERED 1
EXPOSE 5682:5682
ENTRYPOINT ["python", "-m", "debugpy", "--listen", "0.0.0.0:5682", "service.py"]
HEALTHCHECK --interval=30s --timeout=30s --start-period=30s --retries=3 CMD curl -f http://localhost:9200/health || exit 1