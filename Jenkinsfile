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
        branch 'main'
        expression { env.TAG_NAME != env.BRANCH_NAME }
      }
    }
      steps {
        sh 'sonar-scanner -Dsonar.host.url=http://172.31.41.196:9000 -Dsonar.login=admin -Dsonar.password=admin123 -Dsonar.projectKey=backend -Dsonar.qualitygate.wait=true'
      }
    }
   stage('Unit Test'){
     when {
       allOf {
          branch 'main'
       }
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
        echo 'CI'
      }
   }
  }

}
////