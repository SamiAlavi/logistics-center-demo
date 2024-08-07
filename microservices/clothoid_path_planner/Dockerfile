FROM python:3.8 AS base
WORKDIR /app
COPY requirements.txt .
# RUN  pip3 install --extra-index-url https://alpine-wheels.github.io/index numpy
RUN pip install -r requirements.txt
COPY src src
EXPOSE 9000:9000
WORKDIR /app/src

FROM base AS prod
ENTRYPOINT ["python", "service.py"]
HEALTHCHECK --interval=30s --timeout=30s --start-period=30s --retries=3 CMD curl -f http://localhost:9000/health || exit 1

FROM base AS dev
RUN pip install debugpy
ENV PYTHONUNBUFFERED 1
ENTRYPOINT ["python", "-m", "debugpy", "--listen", "0.0.0.0:5679", "service.py"]
HEALTHCHECK --interval=30s --timeout=30s --start-period=30s --retries=3 CMD curl -f http://localhost:9000/health || exit 1