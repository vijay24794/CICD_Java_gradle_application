pipeline{
    agent any 
    stages{
        stage("sonar quality check"){
            agent{
               docker {
                    image 'openjdk:17'
               }
            }
           
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                            sh 'chmod +x gradlew'
                            sh './gradlew sonarqube --debug'
                    }

                     timeout(time: 1, unit: 'HOURS') {
                      def qg = waitForQualityGate()
                      if (qg.status != 'OK') {
                           error "Pipeline aborted due to quality gate failure: ${qg.status}"
                      }
                    }

                }  
            }
        }
    }
}