pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout your code from your version control system (e.g., Git)
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Build your project using Maven
                bat  'mvn clean test package'
            }
        }

      stage('SonarQube Analysis') {
          steps {
              script {
                  def scannerHome = tool name: 'SonarQubeScanner'
                  withCredentials([string(credentialsId: 'gene-token', variable: 'SONAR_TOKEN')]) {
                      bat  "${scannerHome}/bin/sonar-scanner -Dsonar.login=${SONAR_TOKEN}"
                  }
              }
          }
      }



        stage('Deploy to Tomcat') {
            steps {
                // Deploy the WAR file to your Tomcat server
                bat  'cp target/MyProject-0.0.1-SNAPSHOT.war. /path/to/tomcat/webapps'
                deploy adapters: [tomcat10(credentialsId: 'TomcatCreds', path: '', url: 'http://localhost:8081/')], contextPath: 'MyProject', onFailure: false, war: 'target/*.war'

            }
        }
    }

}
