pipeline {
  agent any
  stages {
    stage('Checkout Application Git Branch') {
        steps {
            git credentialsId: '{Credential ID}',
                url: 'https://github.com/kimhj4270/k3sdockerfile.git',
                branch: 'master'
        }
        post {
                failure {
                  echo 'Repository clone failure !'
                }
                success {
                  echo 'Repository clone success !'
                }
        }
    }

    stage('docker build') {
      steps {
        sh "sudo docker build -t 192.168.219.60/k3s/bookinfo-productpage-v1:1.${currentBuild.number} ."
      }
      post {
              failure {
                echo 'Docker image build failure !'
              }
              success {
                echo 'Docker image build success !'
              }
      }
    }

    stage('docker push') {
      steps {
        sh "sudo docker image push 192.168.219.60/k3s/bookinfo-productpage-v1:1.${currentBuild.number}"
      }
      post {
              failure {
                echo 'Docker Image Push failure !'
                sh "docker rmi 192.168.219.60/k3s/bookinfo-productpage-v1:1.${currentBuild.number}"
              }
              success {
                echo 'Docker image push success !'
                sh "docker rmi 192.168.219.60/k3s/bookinfo-productpage-v1:1.${currentBuild.number}"
              }
      }
    }

    stage('K8S Manifest Update') {
        steps {
            git credentialsId: '{Credential ID}',
                url: 'https://github.com/kimhj4270/k3smanifest.git',
                branch: 'master'

            sh "sed -i 's/bookinfo-productpage-v1:1.*\$/bookinfo-productpage-v1:1.${currentBuild.number}/g' bookinfo.yaml"
            sh "git add bookinfo.yaml"
            sh "git commit -m '[UPDATE] bookinfo ${currentBuild.number} image versioning'"
            sh "git push origin master"
        }
        post {
                failure {
                  echo 'K8S Manifest Update failure !'
                }
                success {
                  echo 'K8S Manifest Update success !'
                }
        }
    }


  }
}
