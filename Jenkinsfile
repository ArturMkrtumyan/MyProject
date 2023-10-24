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
                bat  'mvn clean install'
            }
        }

        stage('Test & Jacoco Static Analysis') {
             steps{
                    junit 'target/surefire-reports/**/*.xml'
                    jacoco()
                   }
                }

//         stage('Scan') {
//              steps {
//               withSonarQubeEnv(installationName: 'sq1') {
//                bat './mvnw  org.sonarsource.scanner.maven:sonar-maven-plugin:3.9.0.2155:sonar'
//              }
//            }
//          }

        stage('Code Deployment'){
            steps {
        		deploy adapters: [tomcat9(credentialsId: 'TomcatCreds', path: '', url: 'http://localhost:8081/')], contextPath: "app", war: 'target/*.war'
        	}
       }
    }

}
