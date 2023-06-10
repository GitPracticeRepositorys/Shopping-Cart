pipeline {
    agent any
    tools{
        jdk  'jdk11'
        maven  'MAVEN_3.6.3'
    }
    
    environment{
        SCANNER_HOME= tool 'sonar'
    }
    
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', changelog: false, credentialsId: '15fb69c3-3460-4d51-bd07-2b0545fa5151', poll: false, url: 'https://github.com/GitPracticeRepositorys/Shopping-Cart.git'
            }
        }
        
        stage('COMPILE') {
            steps {
                sh "mvn clean compile -DskipTests=true"
            }
        }
        
        stage('Sonarqube') {
            steps {
                   sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Shopping-Cart \
                   -Dsonar.java.binaries=. \
                   -Dsonar.projectKey=Shopping-Cart '''
               }
            }
        }
        
        stage('Build') {
            steps {
                sh "mvn clean package -DskipTests=true"
            }
        }
        
        stage('Docker Build & Push') {
            steps {
                script {
                        sh "docker build -t shivakrishna99 -f docker/Dockerfile ."
                        sh "docker tag  shopping-cart shivakrishna99/shopping-cart:latest"
                        sh "docker push shivakrishna99/shopping-cart:latest"
                    }
                }
            }
        }
        
    
