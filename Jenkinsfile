pipeline {
    agent any

    environment {
        GIT_CREDENTIALS = '80fb7680-e9da-48aa-80b6-d96387fbafec'
        DOCKER_HUB_CREDENTIALS = 'docker-hub-credentials'
        DOCKER_IMAGE_TAG = "seiler18/mascachicles:AppFinalRelease-${env.BUILD_NUMBER}"
        SONARQUBE_SERVER = 'ProbandoSonar'
        SONARQUBE_TOKEN = credentials('ProbandoSonar')
        NEXUS_CREDENTIALS = 'NexusLogin'
        GROUP_ID = 'cl.talentodigital'
        ARTIFACT_ID = 'appmanageevents'
        VERSION = '0.0.1-RELEASE'
        PACKAGING = 'jar'
        FILE = 'target/appmanageevents-0.0.1-RELEASE.jar'
        SLACK_CHANNEL = '#aplicación-de-eventos'
        SLACK_TOKEN = 'token_from_slacl'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/seiler18/AppManageEvents', branch: 'main', credentialsId: GIT_CREDENTIALS
            }
        }
        stage('Build and Package with Maven') {
            steps {
                sh './mvnw clean package -DskipTests'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv(SONARQUBE_SERVER) {
                        sh './mvnw sonar:sonar -Dsonar.token=${SONARQUBE_TOKEN}'
                    }
                }
            }
        }
        stage('Wait for Quality Gate') {
            steps {
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', DOCKER_HUB_CREDENTIALS) {
                        def dockerImage = docker.build("${DOCKER_IMAGE_TAG}", ".")
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Publish to Nexus') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: NEXUS_CREDENTIALS, usernameVariable: 'NEXUS_USERNAME', passwordVariable: 'NEXUS_PASSWORD')]) {
                        configFileProvider([configFile(fileId: '554327cb-23d7-4efc-8cb0-1cce65e8b32e', variable: 'MAVEN_SETTINGS')]) {
                            sh """
                            ./mvnw deploy -s ${MAVEN_SETTINGS} -DskipTests
                            """
                        }
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completado'
            slackSend(channel: env.SLACK_CHANNEL, color: '#00FF00', message: "Pipeline completed: ${env.JOB_NAME} ${env.BUILD_NUMBER}", token: env.SLACK_TOKEN)
        }
        success {
            echo 'Pipeline succeeded'
            slackSend(channel: env.SLACK_CHANNEL, color: '#00FF00', message: "Pipeline succeeded: ${env.JOB_NAME} ${env.BUILD_NUMBER}", token: env.SLACK_TOKEN)
        }
        failure {
            echo 'Pipeline failed'
            slackSend(channel: env.SLACK_CHANNEL, color: 'danger', message: "Pipeline failed: ${env.JOB_NAME} ${env.BUILD_NUMBER}", token: env.SLACK_TOKEN)
        }
        cleanup {
            echo 'Cleaning up old Docker images'
            sh '''
                docker images --filter "reference=seiler18/mascachicles" --format "{{.ID}}" | tail -n +3 | xargs -r docker rmi -f
                docker images --filter "reference=registry.hub.docker.com/seiler18/mascachicles" --format "{{.ID}}" | tail -n +3 | xargs -r docker rmi -f
            '''
        }
    }
}
