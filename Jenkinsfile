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






        stage('Deploy to Tomcat') {
            steps {
                // Deploy the WAR file to your Tomcat server
                bat "copy target/MyProject-0.0.1-SNAPSHOT.war C:\\apache-tomcat-10.1.14\\webapps"
                deploy adapters: [tomcat10(credentialsId: 'TomcatCreds', path: '', url: 'http://localhost:8081/')], contextPath: 'MyProject', onFailure: false, war: 'target/*.war'

            }
        }
    }

}
