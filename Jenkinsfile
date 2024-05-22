pipeline {
    agent any
    
    tools {
             maven 'maven'
             jdk 'java'
        }
        
    stages {
        stage('Git Checkout') {
            steps {
                git branch: "${env.BRANCH_NAME}", credentialsId: 'GitHub_PAT2', url: 'https://github.com/SaiBadri/java-hello-world-webapp.git'
            }
        }
        stage('Maven Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('ssh to GCP VM') {
            steps {
                sshPublisher(publishers: [sshPublisherDesc(configName: 'tomcat-server01', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/', remoteDirectorySDF: false, removePrefix: '/target', sourceFiles: '**/*.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
    }
}
