pipeline{
    agent{
        label "node"
    }
    stages{
        stage("Sonar quality status"){
            agent{

                docker{
                    image: "maven"
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
    }

}