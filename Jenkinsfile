pipeline {
    agent { label 'Jenkins-Agent' }
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
    environment {
        APP_NAME = "devops-k8s-java-complete-ci-cd"
        RELEASE = "1.0.0"
        DOCKER_USER = "aniljagari"
        DOCKER_CRED = credentials('dockerhub-cred')  // Use credentials
        IMAGE_NAME = "${DOCKER_USER}/${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
        JENKINS_API_TOKEN = credentials("JENKINS_API_TOKEN")
        SONAR_TOKEN = credentials('jenkins-sonarqube-token')
    }

    stages {
        stage("Cleanup Workspace") {
            steps { cleanWs() }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github',
                url: 'https://github.com/AnilJagari/devops-k8s-java-complete-ci-cd'
            }
        }

        stage("Validate Project Structure") {
            steps {
                sh """
                    echo "Checking project structure..."
                    ls -la
                    find . -name "pom.xml" -o -name "Dockerfile" | head -10
                    mvn --version
                    java -version
                """
            }
        }

        stage("Build Application") {
            steps { 
                sh "mvn clean compile"  // First compile to check for errors
            }
        }

        stage("Test Application") {
            steps { 
                sh "mvn test" 
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage("Package Application") {
            steps {
                sh "mvn package -DskipTests"
            }
        }

        stage("SonarQube Analysis") {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token') {
                        sh "mvn sonar:sonar -Dsonar.projectKey=${APP_NAME}"
                    }
                }
            }
        }

        stage("Quality Gate") {
            steps {
                script {
                    waitForQualityGate abortPipeline: true,  // Change to true to fail on quality issues
                    credentialsId: 'jenkins-sonarqube-token'
                }
            }
        }

        stage("Build & Push Docker Image") {
            steps {
                script {
                    // Verify Dockerfile exists
                    sh 'test -f Dockerfile || (echo "Dockerfile not found!" && find . -name "*ockerfile*" && exit 1)'
                    
                    // Build Docker image
                    docker_image = docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                    
                    // Push to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-cred') {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push("latest")
                    }
                }
            }
        }

        stage("Trivy Scan") {
            steps {
                script {
                    sh """
                        trivy image --exit-code 0 --severity HIGH,CRITICAL \
                        ${IMAGE_NAME}:${IMAGE_TAG} --format table
                    """
                }
            }
        }

        stage("Cleanup Artifacts") {
            steps {
                script {
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG} || true"
                    sh "docker rmi ${IMAGE_NAME}:latest || true"
                }
            }
        }

        stage("Trigger CD Pipeline") {
            steps {
                script {
                    build job: 'YAML-manifest-for-devops-k8s-java-complete-ci-cd-cd',
                          parameters: [
                            string(name: 'IMAGE_TAG', value: "${IMAGE_TAG}"),
                            string(name: 'APP_NAME', value: "${APP_NAME}")
                          ],
                          wait: false
                }
            }
        }
    }

    post {
        always {
            // Archive the built JAR file
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            // Store test results
            junit 'target/surefire-reports/*.xml'
        }
        failure {
            emailext body: '''${SCRIPT, template="groovy-html.template"}''',
            subject: "${env.JOB_NAME} - Build # ${env.BUILD_NUMBER} - Failed",
            mimeType: 'text/html', to: "anil@gmail.com"
        }
        success {
            emailext body: '''${SCRIPT, template="groovy-html.template"}''',
            subject: "${env.JOB_NAME} - Build # ${env.BUILD_NUMBER} - Successful",
            mimeType: 'text/html', to: "anil@gmail.com"
        }
    }
}
