pipeline {

    agent any

    environment {
        NEXUS_URL = 'http://nexus:8081'
        NEXUS_REPO = 'nexus-repo-a1'
        NEXUS_CREDENTIALS = 'admin@1234'

        ARTIFACT_NAME = "jenkins-nexus-${BUILD_NUMBER}.tar.gz"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Create Artifact') {
            steps {
                sh '''
                    echo "Creating artifact..."

                    tar -czf ${ARTIFACT_NAME} \
                        docker-compose.yml \
                        Jenkinsfile \
                        nexus.txt \
                        notes.txt

                    echo "Artifact created:"
                    ls -lh ${ARTIFACT_NAME}
                '''
            }
        }

        stage('Upload to Nexus') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: "${NEXUS_CREDENTIALS}",
                        usernameVariable: 'NEXUS_USERNAME',
                        passwordVariable: 'NEXUS_PASSWORD'
                    )
                ]) {

                    sh '''
                        echo "Uploading artifact to Nexus..."

                        curl -v \
                            -u "${NEXUS_USERNAME}:${NEXUS_PASSWORD}" \
                            --upload-file "${ARTIFACT_NAME}" \
                            "${NEXUS_URL}/repository/${NEXUS_REPO}/${ARTIFACT_NAME}"
                    '''
                }
            }
        }

        stage('Verify') {
            steps {
                echo "Artifact uploaded successfully."
                echo "Artifact: ${ARTIFACT_NAME}"
                echo "Nexus repository: ${NEXUS_REPO}"
            }
        }
    }

    post {
        success {
            echo "======================================"
            echo "BUILD SUCCESSFUL"
            echo "Artifact: ${ARTIFACT_NAME}"
            echo "======================================"
        }

        failure {
            echo "======================================"
            echo "BUILD FAILED"
            echo "======================================"
        }

        always {
            sh 'rm -f *.tar.gz || true'
        }
    }
}