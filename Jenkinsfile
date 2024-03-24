pipeline {
    agent any
    tools{
        maven 'Maven'
        jdk 'JDK17'
    }
    stages{
        stage('Clone repo'){
            steps{
                //checkout scm: scmGit(branches: [[name: '*/develop']], extensions: [], user)
                //git branch: 'develop', url: 'https://github.com/WebGoat/WebGoat.git'
                git branch: 'main', url: 'https://github.com/Senhua-Liu/WebGoatProject.git'
            }
        }
        stage('Apply Spotless'){
            steps{
                sh 'mvn spotless:apply'
            }
        }
        stage('Build'){
            steps{
                sh 'java -version'
                sh 'mvn clean install -DskipTests'
            }
        }
        stage('Dependency checker'){
            steps{
                sh 'mvn org.owasp:dependency-check-maven:check -Dformat=ALL'
                dependencyCheckPublisher pattern: '**/target/dependency-check-report.xml'
            }
        }
        stage('SonarQube Analysis'){
            steps{
                withSonarQubeEnv('sonarqube'){
                    sh 'mvn clean verify sonar:sonar -Dmaven.test.skip=true -Dmaven.test.failure.ignore=true -Dsonar.projectName=webgoat -Dsonar.projectKey=webgoat -Dsonar.propjectVersion=1.0 -Dsonar.exclusions=**/*.ts -Dmaven.login=sqp_ecce68174c944a2e2a796368a0532eef253f5212'
                    //sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=webgoat -Dsonar.host.url=http://localhost:9000 -Dsonar.login=sqp_ecce68174c944a2e2a796368a0532eef253f5212'
                }
            }
        }
    }
}
