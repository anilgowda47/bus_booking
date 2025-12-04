pipeline {
    agent { label 'bus' }
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

  stage('Push the artifacts into JFrog Artifactory') {
            steps {
                dir('bus_booking') {
                    script {

                        // WAR or JAR file path (modify based on your project)
                        def ARTIFACT = "${env.WORKSPACE}/bus_booking/target/news-app.war"

                        // Timestamp folder
                        def currentDate = new java.text.SimpleDateFormat("yyyy-MM-dd_HH-mm").format(new Date())

                        // Target folder inside Artifactory repo
                        def targetPath = "feature_release1/${currentDate}/"

                        echo "Uploading artifact: ${ARTIFACT}"
                        echo "Target path: ${targetPath}"

                        rtUpload(
                            serverId: "jfrog",
                            spec: """{
                                "files": [
                                    {
                                        "pattern": "${ARTIFACT}",
                                        "target": "${targetPath}"
                                    }
                                ]
                            }"""
                        )
                    }
                }
            }
        }

        stage('Application') { 
            steps { 
 
                sh 'sleep 10'
                sh 'echo "bus_booking app is running after 10sec"'
                sh 'nohup mvn spring-boot:run > app.log 2>&1 &'
        
                sh 'sleep 60'
                sh 'echo "bus_booking app is stopping after 1 minute"'
                sh 'mvn spring-boot:stop'
               
            }
        }
    }
}

