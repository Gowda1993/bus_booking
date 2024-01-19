pipeline {
    agent { label 'slave1' }
    stages {
        stage('checkout') {
            steps {
                sh 'rm -rf bus_booking'
                sh 'git clone https://github.com/Gowda1993/bus_booking.git'
            }
        }

        stage('build') {
            steps {
                script {
                    sh "mvn clean install"
                }
            }
        }

        stage('Deploy to JFrog Artifactory') {
            steps {
                // Remember this is the step which I followed for free style project.
                script {
                    rtServer(
                        id: "Artifact",
                        url: "http://3.85.9.68:8082//artifactory",
                        username: "admin",
                        password: "Gowda@2024"
                    )
                }
            }
        }

        stage('Upload') {
            steps {
                script {
                    // For my understanding, rtUpload is a part of JFrog Artifactory plugin to upload artifacts to artifacts repo
                    rtUpload (
                        serverId: 'Artifact',
                        spec: '''{
                            "files": [
                                {
                                    "pattern": "target/*.jar",
                                    "target": "libs-release-local/"
                                }
                            ]
                        }'''
                    )
                }
            }
        }

        stage('Publish build info') {
            steps {
                script {
                    // For my understanding to publish build info
                    rtPublishBuildInfo serverId: "Artifact"
                }
            }
    }
    }
}
