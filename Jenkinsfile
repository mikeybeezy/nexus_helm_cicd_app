// pipeline{

//     agent any

//     stages{
//         stage("Sonar quality status"){
            
//             agent{
                
//                 docker {
//                     image "maven"
//                 }
            
//             }
//             steps{
//                 echo "========executing A========"

//                 script{
//                     withSonarQubeEnv(credentialsId: 'sonar_jenkins') {

//                         sh 'mvn clean package sonar:sonar'


//                     } 

//                 }
            
//             }

//         }
//     }

// }

pipeline {
    agent any
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'maven'

                }
            }
            steps {
                script{
                    withSonarQubeEnv(credentialsId: 'sonar_jenkins') {
                     sh 'mvn clean package sonar:sonar'
            }
        }
    }
}

}
}

