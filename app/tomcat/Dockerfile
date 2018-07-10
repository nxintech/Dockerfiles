FROM cargo.caicloudprivatetest.com/library/alpine:3.7 AS build-stage
COPY app.war app.war
RUN mkdir ROOT && unzip app.war -d ROOT


FROM cargo.caicloudprivatetest.com/library/tomcat-alpine:jdk8-8.0.52

COPY --from build-stage /ROOT webapps/ROOT

# apollo -Denv=dev
RUN echo "JAVA_OPTS=\"${JAVA_OPTS} -Denv=dev" >> bin/setenv.sh

EXPOSE 8080
EXPOSE 10053

CMD ["bin/catalina.sh", "run"] 
