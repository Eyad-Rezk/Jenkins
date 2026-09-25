pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
           
            }
        }

         stage('Upload Artifacts to Nexus'){
                    steps{
                        script {
                            nexusArtifactUploader artifacts: [[artifactId: 'requirements_artifact_id', 
                                classifier: '', file: 'src/requirements.txt', type: 'txt']], 
                                credentialsId: 'nexus-user' 
                                nexusUrl: 'localhost:8081', nexusVersion: 'nexus3', 
                                protocol: 'http', repository: 'tutorial', version: version
                        }
                    }
                }
    }
}