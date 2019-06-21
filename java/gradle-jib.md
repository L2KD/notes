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
