FROM cargo.caicloudprivatetest.com/library/oracle-jdk-alpine:1.8

ENV JAVA_OPTS="-Djava.awt.headless=true -Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.port=10053 -Dcom.sun.management.jmxremote.ssl=false -Dcom.sun.management.jmxremote.authenticate=false"

COPY app.jar app.jar

# apollo -Denv=dev
RUN mkdir -p /opt/settings \
    && echo 'env=dev' >> /opt/settings/server.properties

# if use dubbo
# EXPOSE 7000
EXPOSE 8080
EXPOSE 10053

CMD exec java $JAVA_OPTS -Djava.security.egd=file:/dev/./urandom -jar app.jar

