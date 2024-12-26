FROM openjdk:21-jdk
COPY ./app.jar /usr/src/myapp/app.jar

# 환경변수 선언
ENV USER_PROFILE=remote-oracle
ENTRYPOINT ["java"]
CMD ["-jar", "-Dspring.profiles.active=${USER_PROFILE}","app.jar"]
EXPOSE 80
WORKDIR /usr/src/myapp
