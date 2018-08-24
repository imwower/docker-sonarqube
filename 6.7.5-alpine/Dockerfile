FROM openjdk:8-alpine

ENV SONAR_VERSION=6.7.5 \
    SONARQUBE_HOME=/opt/sonarqube \
    SONAR_PMD_PLUGIN_VERSION=2.7.0 \
    # Database configuration
    # Defaults to using H2
    SONARQUBE_JDBC_USERNAME=sonar \
    SONARQUBE_JDBC_PASSWORD=sonar \
    SONARQUBE_JDBC_URL=

# Http port
EXPOSE 9000

RUN addgroup -S sonarqube && adduser -S -G sonarqube sonarqube

RUN set -x \
    && apk add --no-cache gnupg unzip \
    && apk add --no-cache libressl wget \
    && apk add --no-cache su-exec \
    && apk add --no-cache bash \

    && mkdir /opt

WORKDIR /opt
COPY sonarqube-$SONAR_VERSION.zip sonarqube.zip

RUN unzip sonarqube.zip \
    && mv sonarqube-$SONAR_VERSION sonarqube \
    && chown -R sonarqube:sonarqube sonarqube \
    && rm sonarqube.zip* \
    && rm -rf $SONARQUBE_HOME/bin/*
    
WORKDIR $SONARQUBE_HOME/extensions/plugins/
RUN wget -O sonar-pmd-plugin-$SONAR_PMD_PLUGIN_VERSION.jar https://github.com/imwower/sonar-p3c-pmd/releases/download/v$SONAR_PMD_PLUGIN_VERSION/sonar-pmd-plugin-$SONAR_PMD_PLUGIN_VERSION.jar

VOLUME "$SONARQUBE_HOME/data" "$SONARQUBE_HOME/extensions/plugins"

WORKDIR $SONARQUBE_HOME
COPY run.sh $SONARQUBE_HOME/bin/
ENTRYPOINT ["./bin/run.sh"]
