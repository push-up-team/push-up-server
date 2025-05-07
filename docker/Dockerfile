# 1단계: 빌드 스테이지
FROM gradle:8.7-jdk21-alpine AS builder

WORKDIR /app

# Gradle Wrapper 복사
COPY gradlew gradlew.bat /app/
COPY gradle /app/gradle
RUN chmod +x gradlew

# 의존성 캐시 최적화
COPY build.gradle settings.gradle /app/
RUN ./gradlew dependencies --no-daemon || true

# 전체 소스 복사
COPY . /app

# 빌드
RUN ./gradlew bootJar --no-daemon

# 2단계: 런타임 스테이지
FROM amazoncorretto:21-alpine

WORKDIR /push-up-server

COPY --from=builder /app/build/libs/*.jar push-up-server.jar

EXPOSE 8080

ENTRYPOINT ["java", "-Xms512m", "-Xmx1024m", "-jar", "push-up-server.jar"]
