pipeline {
    agent { label 'dev' }

    tools {
        jdk 'JDK17'
        maven 'maven'
    }

    stages {

        stage('Checkout') {
            steps {
                sh 'rm -rf bus_booking'
                sh 'git clone "https://github.com/anilgowda47/bus_booking.git"'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Push the artifacts into JFrog Artifactory (Dummy)') {
            steps {
                dir('bus_booking') {
                    script {

                        def ARTIFACT = "${env.WORKSPACE}/bus_booking/target/news-app.war"
                        def currentDate = new java.text.SimpleDateFormat("yyyy-MM-dd_HH-mm").format(new Date())
                        def targetPath = "feature_release1/${currentDate}/"

                        echo "===== Dummy JFrog Upload Stage ====="
                        echo "Artifact to upload: ${ARTIFACT}"
                        echo "Target path (dummy): ${targetPath}"
                        echo "No JFrog upload performed. (Dummy Stage Running Successfully)"
                        echo "===================================="
                    }
                }
            }
        }

        stage('Application') { 
            steps { 
                sh 'sleep 10'
                sh 'echo "bus_booking app is running after 10 sec"'
                sh 'nohup mvn spring-boot:run > app.log 2>&1 &'

                sh 'sleep 60'
                sh 'echo "bus_booking app is stopping after 1 minute"'
                sh 'mvn spring-boot:stop'
            }
        }
    }
}
