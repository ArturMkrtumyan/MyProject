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
                // Run SonarQube analysis using the SonarScanner
                script {
                   withCredentials([string(credentialsId: 'gene-token', variable: 'SONAR_TOKEN')])
                    def scannerHome = tool name: 'SonarQubeScanner'
                    bat  "${scannerHome}/bin/sonar-scanner"
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
