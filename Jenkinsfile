pipeline {
    agent any

    environment {
        MAVEN_HOME = tool 'Maven'
        NEXUS_URL = 'http://localhost:8081'
        NEXUS_REPO = 'maven-releases'
        NEXUS_CREDENTIALS_ID = 'nexus-creds'
    }

    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Build') {
            steps { sh "${MAVEN_HOME}/bin/mvn clean package" }
        }
        stage('Deploy to Nexus') {
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: "${NEXUS_URL}",
                    groupId: 'com.exemple',
                    version: '1.0.0',
                    repository: "${NEXUS_REPO}",
                    credentialsId: "${NEXUS_CREDENTIALS_ID}",
                    artifacts: [
                        [ artifactId: 'mon-appli',
                          classifier: '',
                          file: 'target/mon-appli-1.0.0.jar',
                          type: 'jar' ]
                    ]
                )
            }
        }
    }
}
