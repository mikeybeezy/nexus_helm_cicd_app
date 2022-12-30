pipeline{

    agent any
    environment {
        VERSION = "${env.BUILD_ID}"

    }

    stages{
        stage("Sonar quality status"){
            
            agent{
                
                docker {
                    image "maven"
                }
            
            }
            steps{
                echo "========executing A========"

                script{
                    withSonarQubeEnv(credentialsId: 'sonar_jenkins') {

                        sh 'mvn clean package sonar:sonar'


                    } 

                }
            
            }

        }
        stage("quality gate stauus"){
            steps{

                script{

                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar_jenkins'
                }

            }
        }

        stage("docker build and push to nexus repo"){
            steps{
                
                script{
                    withCredentials([string(credentialsId: 'sonar_jenkins', variable: 'nexus_creds')]) {
                     sh '''
                     docker build -t 3.249.181.124:8083/java-spring-app:${VERSION} .
                     
                     docker login -u admin -p scaletific 3.249.181.124:8083

                     docker push 3.249.181.124:8083/java-spring-app:${VERSION} 

                     docker rmi 3.249.181.124:8083/java-spring-app:${VERSION} 

                     '''

                    }

                }
            }
        }
    }

}
