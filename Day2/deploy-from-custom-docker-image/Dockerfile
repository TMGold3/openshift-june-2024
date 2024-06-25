FROM ubuntu:16.04

RUN apt update && apt install default-jdk -y

RUN mkdir -p ./app
WORKDIR ./app

COPY app.jar ./

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]


