pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'myapp:latest'
        SONARQUBE = 'SonarQube'  // Tên của SonarQube server trong Jenkins
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/your-repo/your-microservice.git'  // Clone mã nguồn
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    sh 'docker build -t ${DOCKER_IMAGE} .'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Thực hiện phân tích mã nguồn với SonarQube
                    sh 'mvn clean install sonar:sonar -Dsonar.host.url=http://your-sonarqube-server'
                }
            }
        }

        stage('Security Scan with Snyk') {
            steps {
                script {
                    // Kiểm tra bảo mật ứng dụng với Snyk (hoặc Trivy)
                    sh 'snyk test --all-projects'  // Sử dụng Snyk để kiểm tra bảo mật
                }
            }
        }

        stage('Push Docker Image to Registry') {
            steps {
                script {
                    // Push Docker image lên registry (ví dụ DockerHub)
                    sh 'docker tag ${DOCKER_IMAGE} yourusername/${DOCKER_IMAGE}'
                    sh 'docker push yourusername/${DOCKER_IMAGE}'
                }
            }
        }

        stage('Deploy to Production') {
            steps {
                script {
                    // Triển khai Docker container lên môi trường sản xuất
                    sh 'docker run -d --name myapp -p 8080:8080 yourusername/${DOCKER_IMAGE}'
                }
            }
        }
    }

    post {
        always {
            // Cleanup resources nếu cần thiết
            cleanWs()
        }
    }
}
