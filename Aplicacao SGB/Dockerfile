# Etapa 1: Build do frontend
FROM node:18-alpine AS frontend
WORKDIR /app/frontend
COPY ./SGB---Frontend/package*.json ./
RUN npm install
COPY ./SGB---Frontend .
RUN npm run build

# Etapa 2: Build do backend
FROM eclipse-temurin:17-jdk AS backend
WORKDIR /app
COPY ./SGB---Backend/mvnw .
COPY ./SGB---Backend/.mvn .mvn
COPY ./SGB---Backend/pom.xml .
RUN ./mvnw dependency:go-offline
COPY ./SGB---Backend/src ./src
RUN ./mvnw package -DskipTests

# Etapa 3: Combina tudo
FROM eclipse-temurin:17-jdk
WORKDIR /app
COPY --from=backend /app/target/*.jar app.jar
COPY --from=frontend /app/frontend/build ./static
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
