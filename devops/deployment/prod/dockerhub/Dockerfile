# ============================================================
# Stage 1 — Build the Spring Boot application using Maven
# ============================================================
FROM maven:3.9.9-eclipse-temurin-21 AS builder

# Set working directory inside container
WORKDIR /app

# 👇 Copy project files relative to build context (repo root)
COPY pom.xml .
COPY src ./src

# Build the Spring Boot JAR (skip tests for speed)
RUN mvn clean package -DskipTests

# ============================================================
# Stage 2 — Minimal runtime image
# ============================================================
FROM eclipse-temurin:21-jre-alpine

# Metadata for Docker Hub
LABEL maintainer="Kishore Veleti <KishoreVeleti@Gmail.com>" \
      description="Production Docker image for Forex Events Kafka OAuth Spring Boot application" \
      org.opencontainers.image.source="https://github.com/KishoreVeleti/Forex-Events-Kafka-OAuth-Impl"

# Working directory for app runtime
WORKDIR /app

# 👇 Copy JAR built from previous stage
COPY --from=builder /app/target/forex-events-kafka-oauth-impl-*.jar app.jar

# Expose the default app port
EXPOSE 8080

# Environment defaults (can be overridden)
ENV SPRING_PROFILES_ACTIVE=prod \
    JAVA_OPTS="-XX:+UseContainerSupport -XX:MaxRAMPercentage=75.0"

# Run Spring Boot app
ENTRYPOINT ["sh", "-c", "java $JAVA_OPTS -jar app.jar"]
