# ---- Étape 1 : compiler le projet avec Maven + JDK 17 ----
FROM maven:3.9-eclipse-temurin-17 AS build
WORKDIR /app
# Copier le pom.xml puis télécharger les dépendances (mise en cache)
COPY pom.xml .
RUN mvn dependency:go-offline -B
# Copier le code source et générer le .jar (tests ignorés : ils ont besoin de MySQL)
COPY src ./src
RUN mvn clean package -DskipTests -B

# ---- Étape 2 : image finale légère avec seulement Java 17 ----
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
# Récupérer le .jar construit à l'étape 1
COPY --from=build /app/target/*.jar app.jar
# Port de Spring Boot
EXPOSE 8080
# Lancer l'application
ENTRYPOINT ["java", "-jar", "app.jar"]
