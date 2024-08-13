FROM python:3.8 AS base
WORKDIR /app
COPY requirements.txt .
# RUN  pip3 install --extra-index-url https://alpine-wheels.github.io/index numpy
RUN pip install -r requirements.txt
COPY src src
EXPOSE 9000:9000
HEALTHCHECK --interval=30s --timeout=30s --start-period=30s --retries=3 CMD curl -f http://localhost:9000/health || exit 1
WORKDIR /app/src

FROM base AS prod
ENTRYPOINT ["python", "service.py"]

FROM base AS dev
RUN pip install debugpy
ENV PYTHONUNBUFFERED 1
EXPOSE 5680:5680
ENTRYPOINT ["python", "-m", "debugpy", "--listen", "0.0.0.0:5680", "service.py"]