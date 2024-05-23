pipeline {
    agent any
    
    tools {
             maven 'maven'
             jdk 'java'
        }
    
    environment {
        GIT_USER = 'SaiBadri'
        GIT_URL = 'https://github.com/SaiBadri/java-hello-world-webapp.git'
        GITHUB_PAT = credentials('GIT_PAT_4') 
        MAVEN_SETTINGS_CONFIG_ID = 'settings' 
    }

       
    stages {
        stage('Git Checkout') {
            steps {
                git branch: "${env.BRANCH_NAME}", credentialsId: 'GitHub_PAT2', url: "${env.GIT_URL}"
                // checkout scmGit(branches: [[name: '**']], extensions: [], userRemoteConfigs: [[credentialsId: 'GitHub_PAT2', url: 'https://github.com/SaiBadri/java-hello-world-webapp.git']])
            }
        }

        stage('Build Maven Project') {
            steps {
                withCredentials([string(credentialsId: 'GIT_PAT_4', variable: 'GITHUB_PAT')]) {                    
                    sh 'mvn clean install -s settings.xml'
                }
            }
        }

        stage('Deploy to GitHub Packages') {
            steps {
                withCredentials([string(credentialsId: 'GIT_PAT_4', variable: 'GITHUB_PAT')]) {
                    sh 'mvn deploy -s settings.xml'
                }
            }
        }

        stage('ssh to GCP VM') {
            steps {
                sshPublisher(publishers: [sshPublisherDesc(configName: 'tomcat-server01', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/', remoteDirectorySDF: false, removePrefix: '/target', sourceFiles: '**/*.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
    }
}