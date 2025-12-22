pipeline {
    agent any

    environment {
    FRONTEND_IMAGE= "mern-frontend:jenkins"
    BACKEND_IMAGE= "mern-backend:jenkins"
    PORT= "5000"
    MONGO_URI= "mongodb://mongo:27017/taskdb"
    }

    stages {
        stage('Checkout code'){
            steps {
                git url: 'https://github.com/adityarestuhudayana/devops-tutorial.git', branch: 'main'
            }
        }

        stage('Prepare .env'){
            steps {
                sh '''
                    mkdir -p server
                    cat > server/.env << EOF
                    PORT=$PORT
                    MONGO_URI=$MONGO_URI
                    EOF
                '''
            }
        }

        stage('Build docker images'){
            steps {
                sh '''
                    docker build -t $BACKEND_IMAGE ./server
                    docker build -t $FRONTEND_IMAGE --build-arg VITE_API_URL=http://localhost:5000/api ./client
                '''
            }
        }

         stage('Run application'){
            steps {
                sh '''
                    docker compose up -d
                    docker ps
                '''
            }
        }
    }
}