FROM cargo.caicloudprivatetest.com/library/tomcat-alpine:jdk8-8.0.52

ENV JAVA_OPTS="-Djava.awt.headless=true -Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.port=10053 -Dcom.sun.management.jmxremote.ssl=false -Dcom.sun.management.jmxremote.authenticate=false"

COPY app.jar app.jar

EXPOSE 8080
EXPOSE 10053

CMD ["java", "-jar", "app.jar"]
