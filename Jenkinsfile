pipeline {
  agent any
  stages {
    stage('Dev-Build') {
      steps {
        git(url: 'https://github.com/TestLeafInc/WebApp.git', branch: 'master', poll: true)
        bat 'StopApp.bat'
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