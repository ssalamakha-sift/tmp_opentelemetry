FROM alpine:latest AS staging

ARG OPEN_TELEMETRY_VERSION=1.21.0
ARG CLOUD_PROFILER_VERSION=20220905_RC00

WORKDIR /opt/agents

RUN apk add --no-cache wget \
    && wget -q https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/download/v$OPEN_TELEMETRY_VERSION/opentelemetry-javaagent.jar \
    && mkdir -p cloud-profiler && cd cloud-profiler \
    && wget -qO- https://storage.googleapis.com/cloud-profiler/java/cloud-profiler-java-agent_$CLOUD_PROFILER_VERSION.tar.gz | tar xzv


#dummy comment 19
FROM azul/zulu-openjdk:11.0.13

ARG OPEN_TELEMETRY_VERSION=1.21.0
ARG CLOUD_PROFILER_VERSION=20220905_RC00

LABEL maintainer="Sift Inc."
LABEL java_version=11.0.13
LABEL open_telemetry_version=$OPEN_TELEMETRY_VERSION
LABEL cloud_profiler_version=$CLOUD_PROFILER_VERSION

RUN mkdir -p /opt/agents
COPY --from=staging /opt/agents /opt/agents

ENV TELEMETRY_AGENT=/opt/agents/opentelemetry-javaagent.jar
ENV CLOUD_PROFILER_PATH=/opt/agents/cloud-profiler