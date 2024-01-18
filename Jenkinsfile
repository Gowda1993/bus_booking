pipeline {
    agent {
        label 'slave1'
    }
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
                    sh 'mvn --version'
                    sh 'mvn clean install'
                }
            }
        }

        stage('Show Contents of target') {
            steps {
                script {
                    // Print the contents of the target directory
                    sh 'ls -l target'
                }
            }
        }

        stage('Run JAR Locally') {
            steps {
                script {
                    // Run the JAR file using java -jar
                    sh "nohup timeout 10s java -jar target/bus-booking-app-1.0-SNAPSHOT.jar > output.log 2>&1 &"
                    // Sleep for a while to allow the application to start (adjust as needed)
                    sleep 10
                }
            }
        }
        
        stage('deploy') {
            steps {
                sh 'ssh root@172.31.22.10'
                sh "scp /home/slave1/workspace/bus_booking_develop/target/bus-booking-app-1.0-SNAPSHOT.jar root@172.31.22.10:/opt/apache-tomcat-9.0.85/webapps/"
            }
        }
        
    }
        
    post {
        success {
            echo "Build, Run, and Deployment to Tomcat successful!"
        }
        failure {
            echo "Build, Run, and Deployment to Tomcat failed!"
        }
    }
}
