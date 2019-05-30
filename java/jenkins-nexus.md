## Jenkins

Jenkins cơ bản là 1 cái tool hỗ trợ CI thôi. Theo flow sau là 1 trong các cách để áp dụng CI vào project.

Tạo 1 cái prjject trong Jenkins kiểu multibranch (cho phép build ở nhiều branch), miễn sau trong đó có file `Jenkinsfile` (cái này cũng có thể cấu hình được). File này thường sẽ nằm ở root-dir của proj.

`Jenkinsfile` cấu trúc cơ bản như sau:

    pipeline {
      agent {
        node {
          label 'linuxtgg'
        }
      }
      stages {
        stage('Build') {
          steps {
            dir('app-gateway') {
              sh 'mvn -Pprod verify'
            }
          }
        }
        stage('Publish') {
          steps {
            dir('app-gateway') {
              sh 'mvn deploy'
            }
          }
        }
        stage('Deploy') {
          steps {
            echo 'Deploying....'
          }
        }
      }
    }

Trong đó cái agent là xác định cụ thể 1 cái `node` nào sẽ chịu trách nhiệm chạy mấy cái job định bên dưới.

`Node` là 1 instance của tài nguyên server, trong đó có thể cài sẵn các môi trường để có thể build project được, vd windows server abc... linux có cài maven... Các node này admin mới cấu hình được và add vào được.

Stages là các giai đoạn trong cái pipeline này. Mình có thể custom tự do.

Mỗi stage có nhiều step là các bước trong giai đoạn hiện tại. Ds các bước có thể tham khảo ở jenkinsfile syntax hoặc official doc của jenkins.

VD ở trên step `dir` là chuyển vào thư mục con, sau đó chạy command `mvn -Pprod verify`

Vài chú ý:

- Jenkins của cty hiện đang có constraint là đặt tên project phải theo 1 quy tắc mới có thể tạo được, mặc dù phần báo lỗi của công đoạn tạo project về cơ bản mà nói thì không có liên quan gì tới cái constraint... So, lỗi của cty hay lỗi của jenkins?
- Hiện tại không có phân quyền truy cập vào node. VD User a thuộc nhóm 1 có thể truy cập vào cái node của nhóm 69. Giành queue với nhóm người ta. Chắc chỉ có cách là phân quyền bằng 1 vài tin nhắn hăm dọa và cầm búa theo nếu mấy tin nhắn đó trở nên vô dụng.

---

## Nexus Sonatype

Nexus Sonatype (sau đây gọi vắn tắt là nexus), là 1 công cụ, có thể làm:

1. Proxy cho các repo (vd maven)...
2. Hosted --> Lưu các artifact nội bộ đã được package nhưng không muốn publish publicly. (Các version có thể là SNAPSHOT hoặc RELEASE).
3. Ngoài ra còn có docker nữa, nhưng chưa xài nên không biết nói gì.

Trong nexus, phải tạo repo theo 1 trong các loại kể trên. Mỗi repo lại có các config để:

- Chỉ chấp nhận release (nghĩa là không có staging)
- Cho phép redeploy...
- Vân vân, mây mây...

### Cấu hình maven push vào nexus

File `pom.xml`

- Đầu tiên là phải set project name và artifactId giống nhau (có thể gây lỗi 400).
- Nếu repo chỉ chấp nhận `RELEASE`, thì các file jar có `SNAPSHOT` sẽ bị lỗi 401.
- Cấu hình repo như sau

      <distributionManagement>
          <snapshotRepository>
              <id>IT.eHEALTH.Maven.MS-Hosted-snapshots</id>
              <url>https://crelease.devops.x.vn/repository/IT.eHEALTH.Maven.MS-Hosted/</url>
          </snapshotRepository>
          <repository>
              <id>IT.eHEALTH.Maven.MS-Hosted</id>
              <url>https://crelease.devops.x.vn/repository/IT.eHEALTH.Maven.MS-Hosted/</url>
          </repository>
      </distributionManagement>

      <build>
        <pluginManagement>
          <plugins>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-deploy-plugin</artifactId>
              <version>3.0.0-M1</version>
            </plugin>
          </plugins>
        </pluginManagement>
      </build>

- Trong settings.xml (global thì ở chỗ `MAVEN_HOME/conf/settings.xml`, nếu user thì ở `USER_HOME/.m2/settings.xml`) cấu hình credentials để có thể truy cập vào repo được.

      <servers>
        <server>
            <id>IT.eHEALTH.Maven.MS-Hosted-snapshots</id>
            <username>user_name để đăng nhập vào nexus</username>
            <password>x</password>
        </server>
        <server>
            <id>IT.eHEALTH.Maven.MS-Hosted</id>
            <username>user_name</username>
            <password>x</password>
        </server>
      </servers>

      // Trong đó id phải giống với khúc khai báo repo bên `pom.xml`

Để push lên nexus, sau khi chạy

    mvn -Pprod verify

chạy

    mvn deploy

Hoặc có thể upload trực tiếp bằng web GUI của nexus.

Known issue: Khi file fat jar được package, nếu đó là executable file thì nó sẽ được chèn vào 1 cái bash script (có đoạn mào đầu là `#!/bin/bash`, sau đó thực ra là cái script nó sẽ run file jar, cứ mở file jar đó lên bằng 1 cái editor sẽ thấy). File executable jar này nếu đem push hoặc upload manually sẽ bị lỗi 400
