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

        stage('Scan') {
             steps {
              withSonarQubeEnv(installationName: 'sq1') {
               bat './mvnw clean org.sonarsource.scanner.maven:sonar-maven-plugin:3.9.0.2155:sonar'
             }
           }
         }
         stage("Archiving artifacts") {
             steps {
               archiveArtifacts artifacts: 'target/*.war'
               }
         }

        stage('Code Deployment'){
            steps {
        		deploy adapters: [tomcat9(credentialsId: 'TomcatCreds', path: '', url: 'http://localhost:8081/')], contextPath: 'Planview', onFailure: false, war: '**/target/*.war'
        	}
       }
    }

}
