pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Upload to Nexus') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'nexus-creds',
                        usernameVariable: 'NEXUS_USER',
                        passwordVariable: 'NEXUS_PASS')]) {

                    sh """
                    curl -v -u $NEXUS_USER:$NEXUS_PASS \
                    --upload-file target/*.jar \
                    http://localhost:8081/repository/maven-releasesUla/
                    """
                }
                // test trigger

            }
        }
    }
}
