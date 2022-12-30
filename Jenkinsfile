pipeline{

    agent any

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
                    sh 'mvn clean package sonar:sonar'
                }

            }
        }
    }

}
