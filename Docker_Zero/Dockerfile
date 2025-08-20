# 1️⃣ Stage de build com Maven
FROM maven:3.8.4-jdk-8 AS build

# Definir diretório de trabalho dentro do container
WORKDIR /app

# Copiar pom.xml primeiro para baixar dependências em cache
COPY pom.xml .

# Baixar dependências sem ainda copiar o código-fonte
RUN mvn dependency:go-offline -B

# Agora copiar o código-fonte
COPY src ./src

# Rodar build (gera o .jar dentro de /app/target)
RUN mvn clean package -DskipTests

# 2️⃣ Stage de execução com JRE leve
FROM openjdk:8-jre-alpine

# Criar diretório de trabalho
WORKDIR /app

# Copiar o JAR gerado do stage de build
COPY --from=build /app/target/spring-boot-hello-world-1.0.2-SNAPSHOT.jar app.jar

# Expor a porta do Spring Boot
EXPOSE 8080

# Comando para rodar a aplicação
CMD ["java", "-jar", "app.jar"]
