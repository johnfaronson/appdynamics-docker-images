FROM openjdk:8-jre-slim AS builder

LABEL "vendor"="AppDynamics LLC"
LABEL description="AppDynamics Database Agent packaged as a Docker container."
LABEL maintainer="john.aronson@appdynamics.com"
LABEL org.label-schema.docker.cmd="docker run --env-file env  appdynamics/dbagent:<tag>"

ARG APPD_AGENT_VERSION 
ARG APPD_AGENT_SHA256

COPY dbagent-${APPD_AGENT_VERSION}.zip /
RUN echo "${APPD_AGENT_SHA256} *dbagent-${APPD_AGENT_VERSION}.zip" >> appd_checksum \
    && sha256sum -c appd_checksum \
    && rm appd_checksum \
    && unzip -oq dbagent-${APPD_AGENT_VERSION}.zip -d /tmp 


FROM openjdk:8-jre-slim
RUN apt-get update && apt-get -y upgrade && apt-get install -y unzip && apt-get install -y apt-utils && apt-get install -y iproute2 && apt-get install -y procps && apt-get install -y sysstat
COPY --from=builder /tmp /opt/appdynamics

CMD ["sh", "-c", "/opt/appdynamics/start-dbagent ${DBAGENT_PROPERTIES}"]
