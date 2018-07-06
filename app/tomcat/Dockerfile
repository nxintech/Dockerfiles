FROM cargo.caicloudprivatetest.com/library/tomcat-alpine:jdk8-8.0.52

COPY app.war app.awr

RUN set -eux; \
    # apollo -Denv=dev
    echo "JAVA_OPTS=\"${JAVA_OPTS} -Denv=dev" >> bin/setenv.sh; \
    unzip app.war -d webapps/ROOT;\
    rm -f app.war

EXPOSE 8080
EXPOSE 10053

CMD ["bin/catalina.sh", "run"] 
