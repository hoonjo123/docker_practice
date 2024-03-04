FROM openjdk:11 as stage1
WORKDIR /app
# gradle빌드시에 필요한 파일들을 workdir로 copy
COPY gradlew .
COPY gradle gradle
COPY build.gradle .
COPY settings.gradle .
COPY src src

# /app/build/libs/*.jar파일아래 명령어를 통해 생성
RUN chmod +x ./gradlew
RUN ./gradlew bootJar
FROM openjdk:11
WORKDIR /app
# libs밑에 파일을 복사해서 app.jar파일에 넣기위한 자동화
COPY --from=stage1 /app/build/libs/*.jar app.jar

# docker run -d -p 8080:8080 -v /Users/hoon/Desktop/tmp:/tmp ordering_server:v1
VOLUME /tmp

# CMD또는 ENTRYPOINT를 통해 컨테이너 실행
ENTRYPOINT [ "java", "-jar", "app.jar" ]

# docker 실행시 db정보를 환경변수로 주입
# docker run -d -p 8080:8080 -e SPRING_DATASOURCE_URL=jdbc:mariadb://host.docker.internal:3306/spring_order -v /Users/hoon/Desktop/tmp:/tmp ordering_server:v1.0