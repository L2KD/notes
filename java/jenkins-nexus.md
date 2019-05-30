Jenkins cơ bản là 1 cái tool hỗ trợ CI thôi. Theo flow sau là 1 trong các cách để áp dụng CI vào project.

Tạo 1 cái prjject trong Jenkins kiểu multibranch (cho phép build ở nhiều branch), miễn sau trong đó có file Jenkinsfile (cái này cũng có thể cấu hình được).

Jenkinsfile cấu trúc cơ bản như sau:

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
