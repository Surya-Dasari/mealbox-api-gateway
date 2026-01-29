FROM registry.access.redhat.com/ubi9/openjdk-17-runtime

LABEL app="mealbox-api-gateway" runtime="java17"

ARG NEXUS_URL
ARG NEXUS_REPO
ARG NEXUS_USER
ARG NEXUS_PASS

WORKDIR /app
RUN mkdir -p /app && chmod -R g+rwX /app

RUN set -eux; \
  META_URL="${NEXUS_URL}/repository/${NEXUS_REPO}/com/mealbox/api-gateway/0.0.1-SNAPSHOT/maven-metadata.xml"; \
  SNAPSHOT=$(curl -fsSL -u ${NEXUS_USER}:${NEXUS_PASS} ${META_URL} \
    | grep '<value>' | tail -n 1 | sed -E 's/.*>(.*)<.*/\1/'); \
  curl -fsSL -u ${NEXUS_USER}:${NEXUS_PASS} \
    "${NEXUS_URL}/repository/${NEXUS_REPO}/com/mealbox/api-gateway/0.0.1-SNAPSHOT/api-gateway-${SNAPSHOT}.jar" \
    -o app.jar

EXPOSE 9100

ENV JAVA_OPTS="-XX:MaxRAMPercentage=75.0 -XX:+UseContainerSupport"

ENTRYPOINT ["sh","-c","java $JAVA_OPTS -jar app.jar"]
