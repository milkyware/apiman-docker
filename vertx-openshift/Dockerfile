FROM jboss/base-jdk:11
LABEL maintainter="jcechace@redhat.com>" \
      description="Vert.X implementation of Apiman Gateway" \
      version="1.0" \
      apiman.component="gateway" \
      apiman.platform="verx"


ENV JBOSS_HOME="/opt/jboss"
ARG APIMAN_VERSION
ENV APIMAN_HOME="${JBOSS_HOME}/apiman"
ENV APIMAN_CONFIG="${APIMAN_HOME}/configs/conf-container.json"
ENV APIMAN_REGISTRY_URI="file:///${APIMAN_HOME}/configs/registry.json"



RUN cd ${JBOSS_HOME} \
    && curl -L https://github.com/Apiman/apiman/releases/download/$APIMAN_VERSION/apiman-distro-vertx-$APIMAN_VERSION.zip |  bsdtar -xvf- \
    && mv ${JBOSS_HOME}/apiman-distro-vertx-${APIMAN_VERSION} ${APIMAN_HOME} \
    && cd ${APIMAN_HOME} \
    && rm -rf configs \
    && chmod +x apiman-gateway.sh


COPY configs/ ${APIMAN_HOME}/configs/

USER root

RUN chown -R jboss:0 ${JBOSS_HOME} \
 && chmod -R g+rw    ${JBOSS_HOME}

USER jboss

VOLUME ${APIMAN_HOME}/configs/

EXPOSE 8080
EXPOSE 8081

CMD ${APIMAN_HOME}/apiman-gateway.sh -conf "${APIMAN_CONFIG}"



