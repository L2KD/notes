# Gradle Jib, docker build, pushing to private repo

Jib là một thư viện của Google viết, để có thể containerize các app được build bằng maven hoặc gradle.

## Gradle settings

Build và push lên private hub bằng gradle.

1. Add dep vào

   ```groovy
   buildscript {
       dependencies {
           classpath "gradle.plugin.com.google.cloud.tools:jib-gradle-plugin:1.3.0"
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
           entrypoint = ['sh', '-c', 'chmod +x /entrypoint.sh && sync && /entrypoint.sh']
           ports = ['10001']
           environment = [
               SPRING_OUTPUT_ANSI_ENABLED: 'ALWAYS',
               JHIPSTER_SLEEP: '0'
           ]
           useCurrentTimestamp = true
       }
   }
   ```

6. Chạy `gradle jib`.

---

Sử dụng image đã build

1. Pull về

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
