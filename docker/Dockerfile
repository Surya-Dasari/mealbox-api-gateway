FROM registry.access.redhat.com/ubi9/openjdk-17-runtime

LABEL app="mealbox-auth-service" runtime="java17"

WORKDIR /app
RUN mkdir -p /app && chmod -R g+rwX /app

RUN curl -fL \
 http://localhost:8082/repository/mealbox-maven-snapshots/com/mealbox/auth-service/0.0.1-SNAPSHOT/auth-service-0.0.1-SNAPSHOT.jar \
 -o app.jar

EXPOSE 9101

ENV JAVA_OPTS="-XX:MaxRAMPercentage=75.0 -XX:+UseContainerSupport"

ENTRYPOINT ["sh","-c","java $JAVA_OPTS -jar app.jar"]

