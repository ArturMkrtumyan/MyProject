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
                sh 'mvn clean test package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // Run SonarQube analysis using the SonarScanner
                script {
                    def scannerHome = tool name: 'SonarQubeScanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                // Deploy the WAR file to your Tomcat server
                sh 'cp target/MyProject-0.0.1-SNAPSHOT.war. /path/to/tomcat/webapps'
                deploy adapters: [tomcat10(credentialsId: 'TomcatCreds', path: '', url: 'http://localhost:8081/')], contextPath: 'MyProject', onFailure: false, war: 'target/*.war'

            }
        }
    }

    post {
        success {
            // Define actions to take on successful build
        }
    }
}
