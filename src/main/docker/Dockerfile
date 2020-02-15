# buildtime stage
FROM oracle/graalvm-ce:19.3.1-java11 AS build
RUN gu install native-image
COPY ./mvnw ./mvnw
COPY ./.mvn ./.mvn
COPY ./pom.xml ./pom.xml
RUN ./mvnw dependency:go-offline -B
COPY ./src ./src
RUN ./mvnw package -Pnative -DskipTests

# runtime stage
FROM alpine:latest
RUN mkdir /work
WORKDIR /work
COPY --from=build ./target/*-runner /work/application
RUN chmod 775 /work /work/application
EXPOSE 8080
CMD ["./application", "-Dquarkus.http.host=0.0.0.0"]