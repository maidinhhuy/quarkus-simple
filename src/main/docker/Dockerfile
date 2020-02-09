FROM oracle/graalvm-ce:19.3.1-java11 AS build

ENV MAVEN_VERSION=3.6.3 
ENV MAVEN_BASE_URL="https://apache.osuosl.org/maven/maven-3/${MAVEN_VERSION}/binaries" 
ENV MAVEN_TARBALL="apache-maven-${MAVEN_VERSION}-bin.tar.gz" 
ENV MAVEN_HOME=/opt/maven 
ENV M2_HOME=${MAVEN_HOME} 
ENV MAVEN_CONFIG="${MAVEN_HOME}/.m2"

RUN gu install native-image

RUN mkdir -p ${MAVEN_HOME} ${MAVEN_HOME}/ref \
    && curl -o /tmp/${MAVEN_TARBALL} ${MAVEN_BASE_URL}/${MAVEN_TARBALL} \
    && tar -xf /tmp/${MAVEN_TARBALL} -C ${MAVEN_HOME} --strip 1 \
    && ln -s ${MAVEN_HOME}/bin/mvn /usr/bin/mvn

# copy the Project Object Model file
COPY ./pom.xml ./pom.xml

# fetch all dependencies
RUN mvn dependency:go-offline -B

# copy your other files
COPY ./src ./src

RUN mvn package -Pnative -DskipTests

FROM balenalib/raspberrypi4-64
RUN mkdir /work
WORKDIR /work
COPY --from=build ./target/*-runner /work/application
RUN chmod 775 /work /work/application
EXPOSE 8080
CMD ["./application", "-Dquarkus.http.host=0.0.0.0"]