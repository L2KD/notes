# Gradle Jib, docker build, pushing to private repo

Jib là một thư viện của Google viết, để có thể containerize các app được build bằng maven hoặc gradle.

## Gradle settings

Build và push lên private hub bằng gradle.

1. Add dep vào

   ```groovy
   buildscript {
       dependencies {
           classpath "gradle.plugin.com.google.cloud.tools:jib-gradle-plugin:0.9.11" // Version 1.3.0 không chạy được với jhipster 5-6
       }
   }
   ```

2. Sau đó apply plugin

   ```groovy
   apply plugin: com.google.cloud.tools.jib.gradle.JibPlugin
   ```

3. Pull image bằng tay (Chỗ này cần phải tự động bằng build script)

   ```
   docker pull image:version
   ```

4. Đăng nhập private repo

   ```
   docker login -u uname -p pwd https://your-repo.com
   ```

5. Cấu hình:

   ```groovy
   jib {
       from {
           image = 'openjdk:8-jre-alpine'
       }
       to {
           image = 'your-repo.com:<port>/image-name'
           tags = ['latest', project.version] // <-- để tạo ra được 2 tags là phiên bản hiện tại và latest
       }
       container {
           ports = ['10001']
           environment = [
               SPRING_OUTPUT_ANSI_ENABLED: 'ALWAYS',
               JHIPSTER_SLEEP: '0'
           ]
           useCurrentTimestamp = true
           mainClass = mypackage.myapp
           appRoot = '/app'
       }
   }
   ```

6. Chạy `gradle jib`.

---

Sử dụng image đã build

1.  Pull về

    ```
    docker pull your-repo:<port>/iamge-name:version
    ```

    Kết quả

    ```
    Using default tag: latest
    latest: Pulling from emr-service
    e7c96db7181b: Already exists
    f910a506b6cb: Already exists
    b6abafe80f63: Already exists
    763d64e2e818: Pull complete
    0f4f2dd0f42f: Pull complete
    d3df4b142ef1: Pull complete
    0334b69c57e9: Pull complete
    Digest: sha256:57bd93b76549e9c578f2a659e3891a4ba164f75a61324d1bc89acade7b426794
    Status: Downloaded newer image for ...
    ```

2.  Bỏ vào docker-compose file

    ```yaml
    version: '2'
    services:
      app:
        image: repo:port/app
        ports:
          - 10001:10001
        environment:
          - _JAVA_OPTIONS=-Xmx512m -Xms256m
          - SPRING_PROFILES_ACTIVE=prod,no-liquibase
          - EUREKA_CLIENT_SERVICE_URL_DEFAULTZONE=http://admin:$${jhipster.registry.password}@x.x.x.x:8761/eureka
          - SPRING_CLOUD_CONFIG_URI=http://admin:$${jhipster.registry.password}@x.x.x.x:8761/config
          - JHIPSTER_SLEEP=2 # gives time for the JHipster Registry to boot before the application
    ```
