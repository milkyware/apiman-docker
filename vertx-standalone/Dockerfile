ARG CLI_VERSION="0.4.0"
FROM apiman/cli:${CLI_VERSION} AS cli

FROM openjdk:8

ARG APIMAN_VERSION

RUN useradd -m -d /home/apiman apiman && \
	mkdir -p /opt/apiman/apis /opt/apiman/declarations /opt/apiman/lib && \
	chown -R apiman:apiman /home/apiman /opt/apiman

USER apiman
RUN curl -s -L https://github.com/Apiman/apiman/releases/download/$APIMAN_VERSION/apiman-distro-vertx-$APIMAN_VERSION.zip -o /tmp/apiman-distro-vertx-${APIMAN_VERSION}.zip && \
	unzip /tmp/apiman-distro-vertx-${APIMAN_VERSION}.zip -d /tmp && \
	mv /tmp/apiman-distro-vertx-${APIMAN_VERSION}/* /opt/apiman/

COPY --from=cli --chown=apiman:apiman /opt/apiman-cli/lib/apiman-cli.jar /opt/apiman/lib
COPY --chown=apiman:apiman files /opt/apiman

RUN chmod +x /opt/apiman/docker-entrypoint.sh

WORKDIR /opt/apiman
EXPOSE 8081 8082

ENTRYPOINT [ "./docker-entrypoint.sh" ]
