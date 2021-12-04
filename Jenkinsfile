pipeline{
    agent any
    stages{
        stage("sonar quality check"){
            agent {
                    docker {
                        image 'openjdk:11'
                    }
            }
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-cred') {
                        sh 'chmod +x gradlew'
                        sh './gradlew sonarqube'
                    }

                    timeout(time: 1, unit: 'HOURS') {
                        def qg = waitFor QualityGate()
                        if (qg.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${qg.status"
                        }
                    }
                }
            }
        }
    }
}