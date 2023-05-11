pipeline {
  agent any
  stages {
//     stage('Build') {
//       parallel {
//         stage('Build') {
//           steps {
//             sh 'docker-compose -f ./globalwhitelist/docker-compose.yaml up -d --build'
//             sh 'docker-compose -f test.yaml up -d --build'
//           }
//         }

//         stage('Tagging') {
//           steps {
//             sh 'docker tag odoo_staging_odoo gechcode/odoo_15_staging:"$BUILD_NUMBER"'
//           }
//         }

//       }
//     }
    stage('Build Whitelists') {
          steps {
            sh 'docker-compose -f ./globalwhitelist/docker-compose.yaml up -d --build'
           
        }
    }
    stage('Build Odoo') {
          steps {
            
            sh 'docker-compose -f test.yaml up -d --build'
        }
    }
    stage('Tagging') {
          steps {
            sh 'docker tag odoo_staging_odoo gechcode/odoo_15_staging:"$BUILD_NUMBER"'
          }
        }
        
    stage('Test') {
      steps {
        echo 'Testing..'
      }
    }

    stage('Login Dockerhub') {
      environment {
        DOCKERHUB_USER = 'gechcode'
        DOCKERHUB_PWD = 'Gechcodemax1'
      }
      steps {
        sh 'docker login -u $DOCKERHUB_USER -p $DOCKERHUB_PWD'
      }
    }

    stage('Push to Dockerhub') {
      steps {
        sh 'docker push gechcode/odoo_15_staging:"$BUILD_NUMBER"'
      }
    }

  }
  environment {
    registry = 'gechcode/odoo_15'
    registryCredential = '7249de4f-7415-4d48-9e68-6f77218f9e51'
  }
  options {
    buildDiscarder(logRotator(numToKeepStr: '5', daysToKeepStr: '5'))
    timestamps()
  }
}
