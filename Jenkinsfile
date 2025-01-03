pipeline {
    agent any
    environment {
        AWS_ACCESS_KEY_ID = 'AKIAS2VS377U2EHC45EL'
        AWS_SECRET_ACCESS_KEY = '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'
        AWS_REGION = 'us-east-1'
        ACCOUNT_ID = '194722398185'
        ECR_URL = "${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"
    }

    stages {
        stage('Git checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Pushparaj1611/Ekart.git'
            }
        }
        stage('Compile') {
            steps {
               sh "mvn  compile"
            }
        }    
        stage('Test') {
            steps {
               sh "mvn test -DskipTests=true"
            }
        }
        stage('Sonarqube') {
            steps {
               withSonarQubeEnv('sonarqube') {
            sh ''' mvn sonar:sonar \
            -Dsonar.projectKey=Ekart \
            -Dsonar.projectName="Ekart Project" \
            -D.sonar.java.binaries=. 
            '''
                }
            }
        }
        stage('Build') {
            steps {
               sh "mvn package -DskipTests=true"
            }
        }
        stage('deploy') {
            steps {
                withMaven(globalMavenSettingsConfig: 'global-maven', jdk: '', maven: '', mavenSettingsConfig: '', traceability: true) {
              sh "mvn deploy -DskipTests=true"   
              }
               
            }
        }
        stage('Login to ECR') {
            steps {
                sh 'aws --version'  // Check AWS CLI installation
                sh '''
                aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $ECR_URL
                '''
            }
        }     
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $ECR_URL/ekart:latest -f docker/Dockerfile .'
            }
        }
        stage('Push Docker Image') {
            steps {
                sh 'docker push $ECR_URL/ekart:latest'
            }
        }
        }  
    }

