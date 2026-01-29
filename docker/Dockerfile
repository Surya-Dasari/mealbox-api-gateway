FROM registry.access.redhat.com/ubi9/openjdk-17-runtime

LABEL app="mealbox-api-gateway" runtime="java17"

ARG NEXUS_URL
ARG NEXUS_REPO
ARG NEXUS_USER
ARG NEXUS_PASS

WORKDIR /app
RUN mkdir -p /app && chmod -R g+rwX /app

# Resolve SNAPSHOT JAR explicitly (jar-only, robust)
RUN set -e; \
  SNAPSHOT_VERSION=$(curl -s -u ${NEXUS_USER}:${NEXUS_PASS} \
    ${NEXUS_URL}/repository/${NEXUS_REPO}/com/mealbox/api-gateway/0.0.1-SNAPSHOT/maven-metadata.xml \
    | awk '
      /<snapshotVersion>/ { in_sv=1 }
      in_sv && /<extension>jar<\/extension>/ { jar=1 }
      jar && /<value>/ {
        gsub(/.*<value>|<\/value>.*/, "", $0);
        print;
        exit
      }
      /<\/snapshotVersion>/ { in_sv=0; jar=0 }
    '); \
  echo "Resolved SNAPSHOT version: ${SNAPSHOT_VERSION}"; \
  curl -fL -u ${NEXUS_USER}:${NEXUS_PASS} \
    ${NEXUS_URL}/repository/${NEXUS_REPO}/com/mealbox/api-gateway/0.0.1-SNAPSHOT/api-gateway-${SNAPSHOT_VERSION}.jar \
    -o app.jar

EXPOSE 9100

ENV JAVA_OPTS="-XX:MaxRAMPercentage=75.0 -XX:+UseContainerSupport"

ENTRYPOINT ["sh","-c","java $JAVA_OPTS -jar app.jar"]
