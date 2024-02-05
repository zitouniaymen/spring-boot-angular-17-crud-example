# Build stage
FROM maven:3-openjdk-17 AS build
WORKDIR /build

# Copier le fichier pom.xml et télécharger les dépendances pour éviter de re-télécharger à chaque changement de code
COPY spring-boot-server/pom.xml .
RUN mvn dependency:go-offline

# Copier le code source et construire le projet
COPY spring-boot-server/src ./src
RUN mvn clean package -DskipTests

# Runtime stage
FROM amazoncorretto:17 AS runtime
ARG PROFILE=local
ARG APP_VERSION=0.0.1-SNAPSHOT

WORKDIR /app

# Copier le fichier JAR généré depuis l'étape de build
COPY --from=build /build/target/spring-boot-jpa-h2-${APP_VERSION}.jar /app/

# Exposer le port 8080
EXPOSE 8080

# Définir les variables d'environnement
ENV DB_URL=jdbc:postgresql://localhost:5432/dockerTest
ENV ACTIVE_PROFILE=${PROFILE}

# Démarrer l'application
CMD java -jar -Dspring.profiles.active=${ACTIVE_PROFILE} -Dspring.datasource.url=${DB_URL} spring-boot-jpa-h2-${APP_VERSION}.jar
