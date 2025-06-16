pipeline {
    agent any

    environment {
        MAVEN_HOME = tool name: 'Maven', type: 'maven'
        NEXUS_URL = 'http://localhost:8081'
        NEXUS_REPO = 'maven-releases'
        NEXUS_CREDENTIALS_ID = 'nexus-creds'
    }

    stages {
        stage('Checkout') {
            steps {
                // Clonage du dépôt selon la branche définie dans Jenkins
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Utiliser sh sous Linux/macOS, ou bat sous Windows
                script {
                    if (isUnix()) {
                        sh "${MAVEN_HOME}/bin/mvn clean package"
                    } else {
                        bat "${MAVEN_HOME}\\bin\\mvn clean package"
                    }
                }
            }
        }

        stage('Deploy to Nexus') {
            steps {
                script {
                    nexusArtifactUploader(
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        nexusUrl: "${NEXUS_URL}",
                        groupId: 'com.exemple',
                        version: '1.0.0',
                        repository: "${NEXUS_REPO}",
                        credentialsId: "${NEXUS_CREDENTIALS_ID}",
                        artifacts: [[
                            artifactId: 'mon-appli',
                            classifier: '',
                            file: 'target/mon-appli-1.0.0.jar',
                            type: 'jar'
                        ]]
                    )
                }
            }
        }
    }
}

