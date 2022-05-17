    pipeline {
        agent any
        environment {
            imageName = "loadgenerator"
            registryCredentials = "nexus"
            dockerImage = ''
            }
        stages {
            stage('Code checkout') {
                steps {
                    checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'prodevopsprojects', url: 'git@github.com:prodevopsprojects/loadgenerator.git']]])
                }
            }
            stage('Building image') {
                steps{
                    script {
                    dockerImage = docker.build imageName
                    }
                }
            }
            stage('Uploading to Nexus') {
                steps{  
                    script {
                        docker.withRegistry( 'http://'+registry, registryCredentials ) {
                        dockerImage.push('latest')
                        }
                    }
                }
            }
        }
    }
