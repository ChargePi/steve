FROM eclipse-temurin:17-jdk as base
LABEL maintainer="daynnnnn"

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8

ARG DB_HOST
ARG DB_USERNAME
ARG DB_PASSWORD
ARG DB_DATABASE
ARG DB_PORT

FROM base as build

WORKDIR /code

ADD /src /code/src
ADD /website /code/website
ADD /pom.xml /code/pom.xml
ADD /mvnw /code/mvnw
ADD /.mvn /code/.mvn


RUN sed -i 's|${db.ip}|${env.DB_HOST}|g' pom.xml
RUN sed -i 's|${db.port}|${env.DB_PORT}|g' pom.xml
RUN sed -i 's|${db.user}|${env.DB_USERNAME}|g' pom.xml
RUN sed -i 's|${db.password}|${env.DB_PASSWORD}|g' pom.xml
RUN sed -i 's|${db.schema}|${env.DB_DATABASE}|g' pom.xml

RUN ./mvnw clean package -Pkubernetes -Djdk.tls.client.protocols="TLSv1,TLSv1.1,TLSv1.2"

FROM base
LABEL maintainer="daynnnnn"

COPY --from=build /code/target/steve.jar steve.jar
COPY --from=build /code/target/libs libs

CMD java -jar steve.jar
