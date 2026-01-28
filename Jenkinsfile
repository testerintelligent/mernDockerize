pipeline {
    agent  { label 'LinuxAgent' }
    environment {
        GIT_REPO_URL = 'https://github.com/testerintelligent/mernDockerize.git'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: "${env.GIT_REPO_URL}"
            }
        }

        stage('Build and Run Containers') {
            steps {
                script {
                    sh """
                     echo "Disconnect the docker Network" |
                        docker network disconnect merndockerize_default merndockerize-frontend-1
                        docker network disconnect merndockerize_mern-network merndockerize-backend-1
                        docker network disconnect merndockerize_mern-network merndockerize-mongo-1
                     echo "Down the docker Container" |
                     sudo -S docker-compose down
                     sudo docker-compose up --build -d
                    """
                }
            }
        }

        stage('Display URL') {
            steps {
                script {
                    def url = "10.192.190.158:8001"
                    echo "Application is running at ${url}"
                }
            }
        }
    }
}
