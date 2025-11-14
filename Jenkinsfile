pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                dir('initial') {
                    sh 'mvn -B -DskipTests clean package'
                }
            }
        }

        stage('Test') {
            steps {
                dir('initial') {
                    sh 'mvn test'
                }
            }
        }

        stage('Upload to Nexus') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'nexus-creds',
                        usernameVariable: 'NEXUS_USER',
                        passwordVariable: 'NEXUS_PASS')]) {

                    dir('initial/target') {
                        sh """
                            curl -v -u $NEXUS_USER:$NEXUS_PASS \
                            --upload-file *.jar \
                            http://localhost:8081/repository/maven-releasesUla/
                        """
                    }
                }
            }
        }
    }
}

