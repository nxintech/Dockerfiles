FROM cargo.caicloudprivatetest.com/library/tomcat-alpine:jdk8-8.0.52

RUN set -eux; \
	  echo "JAVA_OPTS="-Djava.awt.headless=true -Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.port=10053 -Dcom.sun.management.jmxremote.ssl=false -Dcom.sun.management.jmxremote.authenticate=false" > bin/setenv.sh; \
	  mkdir webapps/ROOT; \
	  unzip app.war -d webapps/ROOT;\
	  rm -f app.war

EXPOSE 8080
EXPOSE 10053

CMD ["bin/catalina.sh", "run"] 
