pipeline {
  agent { label 'workstation'}

 stages {

  stage('Download Dependencies'){
    steps {
      sh 'npm install'
    }
  }

  stage('Code Quality'){
    when {

      allOf {

        expression { env.TAG_NAME != env.BRANCH_NAME }
      }
    }
      steps {
        sh 'sonar-scanner -Dsonar.host.url=http://172.31.41.196:9000 -Dsonar.login=admin -Dsonar.password=admin123 -Dsonar.projectKey=backend -Dsonar.qualitygate.wait=true'
      }
    }
   stage('Unit Test'){
     when {

       branch 'main'

     }
     steps {
        echo 'CI'
     }
   }

   stage('Release'){
      when {
        expression { env.TAG_NAME ==~ ".*" }
      }
      steps {
        sh 'zip -r backend-${TAG_NAME}.zip node_modules schema DbConfig.js index.js package.json TransactionService.js'
        sh 'curl -sSf -u "admin:Admin123" -X PUT -T backend-${TAG_NAME}.zip "http://artifactory.devopsa17.online:8081/artifactory/backend/backend-${TAG_NAME}.zip"'
      }
   }
  }

}
////