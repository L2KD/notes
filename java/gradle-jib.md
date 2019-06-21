# Gradle Jib, docker build, pushing to private repo

Jib là một thư viện của Google viết, để có thể containerize các app được build bằng maven hoặc gradle.

## Gradle settings

1. Add dep vào

   ```groovy
   buildscript {
       dependencies {
           classpath "gradle.plugin.com.google.cloud.tools:jib-gradle-plugin:1.3.0"
       }
   }
   ```

2. Sau đó apply plugin

   ```
   apply plugin: com.google.cloud.tools.jib.gradle.JibPlugin
   ```

3. Pull image bằng tay (Chỗ này cần phải tự động bằng build script)
