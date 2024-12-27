pipeline {
    agent any

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
    }
}
