pipeline{
    agent any
    triggers {
        pollSCM('* * * * *')
    }
    
        stages{
            stage('git checkout'){
                steps{
                    git credentialsId:'github-credentials4', url:'https://github.com/misafidiniaina/tp-junit.git'
                }
            }
            
            
            stage('Build the application'){
                steps{
                    sh "mvn clean install"
                }
            }
            
            stage("Unit test Execution"){
                steps{
                    sh "mvn test"
                }
            }
            stage("Build the docker image") {
                steps{
                sh "docker build --tag misafidiniaina/tp-docker:1.0.0 ."
                }
            }
            
        }
        post{
            failure{
                emailext(
                               subject: "Build $BUILD_NUMBER échoué",
                                body: "Ce Build $BUILD_NUMBER a échoué",
                                to: "liantsoatsiorinirina@gmail.com"
                            )
                } 
        }
}
