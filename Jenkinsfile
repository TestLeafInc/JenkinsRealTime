pipeline {
  agent any
  stages {
    stage('Dev-Build') {
      steps {
        git(url: 'https://github.com/TestLeafInc/WebApp.git', branch: 'master', poll: true)
        script{
          try{
              bat 'StopApp.bat'
          }catch(Exception e){
            echo 'There is no app running in port 9002'
          }
        }
        bat 'mvn install'
        bat 'StartApp.bat'
      }
    }

    stage('UI-Automation') {
      parallel {
        stage('UI-Automation') {
          steps {
            git(url: 'https://github.com/TestLeafInc/WebAppUiAutomation.git', branch: 'master')
            bat 'mvn test'
          }
        }

        stage('API-Automation') {
          steps {
            git(url: 'https://github.com/TestLeafInc/WebAppApiAutomation.git', branch: 'master')
            bat 'mvn test'
          }
        }

        stage('Performance-Testing') {
          steps {
            echo 'Jmeter testing done'
          }
        }

      }
    }

    stage('UAT Testing') {
      steps {
        input 'Confirm all your regression tests passed'
      }
    }

  }
}
