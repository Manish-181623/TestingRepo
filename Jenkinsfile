pipeline {
    agent any
    environment {
        SCANNER_HOME = tool name: 'SonarQubeScanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
    }

    stages {
        stage('Git Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Manish-181623/HotelBook_Java_With_CI-CD_Monitoring.git']])
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool name: 'SonarQubeScanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    withEnv(["PATH+SONAR=${scannerHome}/bin"]) {
                        sh """
                            sonar-scanner \
                            -Dsonar.projectKey=sonar_test \
                            -Dsonar.host.url=http://127.0.0.1:9000 \
                            -Dsonar.login=sqp_7df39fa4343f173a016a0ab0e103fbab8a0b5a2e \
                            -Dsonar.java.binaries=.
                        """
                    }
                }
            }
        }

        stage('Docker-Compose Up') {
            steps {
                script {
                    sh 'docker-compose up -d'
                }
            }
        }
    }

    post {
        // always {
        //     // Clean up Docker containers
        //     sh 'docker-compose down'
        // }
        success {
            // Post-build actions to execute on success
            slackSend channel: '#jenkins-testing',
                      color: 'good',
                      message: "manish-sucess"
        }
        failure {
            // Post-build actions to execute on failure
            slackSend channel: '#jenkins-testing',
                      color: 'danger',
                      message: "manish-failed"
        }
    }
}

