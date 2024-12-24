FROM openjdk:21-jdk

ARG guest.jar
CMD pwd
COPY guest.jar app.jar

# 환경변수 선언
ENV USER_PROFILE=remote-oracle
EXPOSE 80
ENTRYPOINT ["java"]
CMD ["-jar", "-Dspring.profiles.active=${USER_PROFILE}","app.jar"]
