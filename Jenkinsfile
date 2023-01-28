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
  }
  
}
