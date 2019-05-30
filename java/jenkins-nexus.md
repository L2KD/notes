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
