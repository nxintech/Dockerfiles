FROM cargo.caicloudprivatetest.com/base/centos7-oracle-jdk-8

CMD ["gradle"]

ENV JAVA_HOME /usr/java/latest
ENV GRADLE_HOME /opt/gradle
ENV GRADLE_VERSION 4.7

ARG GRADLE_DOWNLOAD_SHA256=fca5087dc8b50c64655c000989635664a73b11b9bd3703c7d6cabd31b7dcdb04
RUN set -o errexit -o nounset \
    && echo "Downloading Gradle" \
    && wget --no-verbose --output-document=gradle.zip "https://services.gradle.org/distributions/gradle-${GRADLE_VERSION}-bin.zip" \
    \
    && echo "Checking download hash" \
    && echo "${GRADLE_DOWNLOAD_SHA256} *gradle.zip" | sha256sum --check - \
    \
    && echo "Installing Gradle" \
    && unzip gradle.zip \
    && rm -f gradle.zip \
    && mv "gradle-${GRADLE_VERSION}" "${GRADLE_HOME}/" \
    && ln --symbolic "${GRADLE_HOME}/bin/gradle" /usr/bin/gradle \
    \
    && echo "Adding gradle home" \
    && mkdir /root/.gradle 

VOLUME "/root/.gradle"
WORKDIR /root

RUN set -o errexit -o nounset \
    && echo "Testing Gradle installation" \
    && gradle --version
