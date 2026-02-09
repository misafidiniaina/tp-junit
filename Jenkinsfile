pipeline {
    agent any
    
    triggers {
        pollSCM('* * * * *')  // Fixed: Added missing asterisk and proper spacing
    }
    
    stages {
        stage('git checkout') {
            steps {
                git branch: 'main', 
                    url: 'https://github.com/misafidiniaina/tp-junit.git'
            }
        }
        
        stage('Build the application') {
            steps {
		echo "Bonjour pipeline"
                sh "mvn clean install"
            }
        }
        
        stage('Unit test Execution') {
            steps {
                sh "mvn test"
            }
        }
        
        stage('Build the docker image') {
            steps {
                sh "docker build --tag misafidiniaina/tpjenkins:v1 ."
            }
        }
        stage('Deploy to dockerhub'){
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login \
                        -u "$DOCKER_USER" --password-stdin
                    '''
                }
        
                sh 'docker push misafidiniaina/tpjenkins:v1'
            }
        }
    }
    
    post {
        failure {
            emailext(
                subject: "Build $BUILD_NUMBER échoué",
                body: "Ce Build $BUILD_NUMBER a échoué",
                to: "liantsoatsiorinirina@gmail.com"
            )
        }
    }
}
