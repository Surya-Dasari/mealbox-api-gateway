FROM registry.access.redhat.com/ubi9/openjdk-17-runtime

LABEL app="mealbox-api-gateway" runtime="java17"

ARG NEXUS_URL
ARG NEXUS_REPO
ARG NEXUS_USER
ARG NEXUS_PASS

WORKDIR /app
RUN mkdir -p /app && chmod -R g+rwX /app

RUN curl -fL \
  -u ${NEXUS_USER}:${NEXUS_PASS} \
  ${NEXUS_URL}/repository/${NEXUS_REPO}/com/mealbox/api-gateway/0.0.1-SNAPSHOT/api-gateway-0.0.1-SNAPSHOT.jar \
  -o app.jar

EXPOSE 9100

ENV JAVA_OPTS="-XX:MaxRAMPercentage=75.0 -XX:+UseContainerSupport"

ENTRYPOINT ["sh","-c","java $JAVA_OPTS -jar app.jar"]

