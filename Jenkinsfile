pipeline {
    agent any
    
    tools {
             maven 'maven'
             jdk 'java'
        }
    
    environment {
        GIT_USER = 'SaiBadri'
        GIT_URL = 'https://github.com/SaiBadri/java-hello-world-webapp.git'
        GITHUB_PAT = credentials('id:GIT_PAT_4') // Fetch the GitHub PAT from Jenkins credentials
        MAVEN_SETTINGS_CONFIG_ID = 'maven-settings' // Config ID for Maven settings.xml in Jenkins
    //     GCP_VM_CONFIG = 'tomcat-server01' // SSH Publisher configuration name
    //     ARTIFACT_PATH = '/Users/badri/.m2/repository/org/cloudifysource/examples/java-hello-world-webapp/1.0-SNAPSHOT/java-hello-world-webapp-1.0-SNAPSHOT.war'
    }

    // triggers {
    //     githubPullRequest commentTrigger('^/build', notifyEveryCommit: false)
    // }
    
    stages {
        stage('Git Checkout') {
            steps {
                git branch: "${env.BRANCH_NAME}", credentialsId: 'GitHub_PAT2', url: 'https://github.com/SaiBadri/java-hello-world-webapp.git'
                // checkout scmGit(branches: [[name: '**']], extensions: [], userRemoteConfigs: [[credentialsId: 'GitHub_PAT2', url: 'https://github.com/SaiBadri/java-hello-world-webapp.git']])
            }
        }

        stage('Build Maven Project') {
            steps {
                withCredentials([string(credentialsId: 'GIT_PAT_4', variable: 'GITHUB_PAT')]) {
                    configFileProvider([configFile(fileId: env.MAVEN_SETTINGS_CONFIG_ID, variable: 'MAVEN_SETTINGS')]) {
                        sh 'mvn clean install -s $MAVEN_SETTINGS'
                    }
                }
            }
        }

        stage('Deploy to GitHub Packages') {
            steps {
                withCredentials([string(credentialsId: 'GIT_PAT_4', variable: 'GITHUB_PAT')]) {
                    configFileProvider([configFile(fileId: env.MAVEN_SETTINGS_CONFIG_ID, variable: 'MAVEN_SETTINGS')]) {
                        sh 'mvn deploy -s $MAVEN_SETTINGS'
                    }
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

