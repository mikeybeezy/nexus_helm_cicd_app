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
                    withCredentials([string(credentialsId: 'revised_nexus_credentials', variable: 'NEXUS_DETAILS')]) {
                     sh '''
                     docker build -t 34.251.22.225:8083/java-spring-app:${VERSION} .
                     
                     docker login -u admin -p $NEXUS_DETAILS 34.251.22.225:8083

                     docker push 34.251.22.225:8083/java-spring-app:${VERSION} 

                     docker rmi 34.251.22.225:8083/java-spring-app:${VERSION} 
                     '''
               

                    }
                }

            }
        }
        stage("identifying misconfigurations using datree"){
            steps{
                script{
                    dir('kubernetes/myapp/') {
                        withEnv(['DATREE_TOKEN=fe2b236e-b721-4d1c-9816-3cbfe9f16d98']) {
                        sh 'helm datree test .'
                        }
                    }

                }
            }
        }
        stage("Pushing the helm chart"){
            steps{
                script{
                    withCredentials([string(credentialsId: 'revised_nexus_credentials', variable: 'NEXUS_DETAILS')]) {
                        dir('kubernetes/') {
                        sh '''
                        helmversion=$(helm show chart myapp | grep version | cut -d: -f 2 | tr -d ' ') 
                        tar -czcf myapp-${helmversion}.tgz myapp/
                        curl -u admin:$NEXUS_DETAILS http://34.251.22.225:8081/repository/first-java-app/ --upload-file myapp-${helmversion}.tgz -v
                        '''
                        }

               

                    }

                }
            }
        }
        stage("Docker None Image clean up"){
            steps{
                script{
                        sh '''
                        docker rmi $(docker images |grep "<none>"|awk '$1=="<none>" {print $3}')
                        '''

                }
            }
        }
    }
    post {
		always {
			mail bcc: '', body: "<br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "${currentBuild.result} CI: Project name -> ${env.JOB_NAME}", to: "mikeybabs40@gmail.com";  
		}
	}

}


docker rmi $(docker images |grep "<none>"|awk '$1=="<none>" {print $3}')