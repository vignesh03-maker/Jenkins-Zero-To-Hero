# ==========================
# 1. Build stage (Maven + JDK)
# ==========================
FROM maven:3.9.9-eclipse-temurin-17 AS build

WORKDIR /app

# Copy pom.xml and download dependencies first (cache optimization)
COPY pom.xml .
RUN mvn dependency:go-offline -B

# Copy source code and build the jar
COPY src ./src
RUN mvn clean package -DskipTests

# ==========================
# 2. Runtime stage (Lightweight JRE)
# ==========================
FROM eclipse-temurin:17-jre-alpine

WORKDIR /opt/app

# Copy jar from build stage
COPY --from=build /app/target/*.jar app.jar

# Run the app
ENTRYPOINT ["java","-jar","app.jar"]

