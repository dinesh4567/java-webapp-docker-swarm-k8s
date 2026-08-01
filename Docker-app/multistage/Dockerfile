# Multi-stage build: compiles the WAR from THIS repository's source.
#
# The previous version cloned a third-party repository
# (imranvisualpath/vprofile-repo) at build time, so any change made here -
# including moving the database credentials out of application.properties -
# was silently excluded from the image.
#
# Build context is the repository root:
#   docker build -t dinesh78900/eks:app -f Docker-app/multistage/Dockerfile .

FROM maven:3.8.6-openjdk-8 AS build
WORKDIR /build

# Resolve dependencies first so they cache independently of source changes.
COPY pom.xml .
RUN mvn -q -B dependency:go-offline

COPY src ./src
RUN mvn -q -B clean package -DskipTests

FROM tomcat:8-jre11
RUN rm -rf /usr/local/tomcat/webapps/*
COPY --from=build /build/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080
CMD ["catalina.sh", "run"]
