pipeline {
    agent any

    environment {
        FRONTEND_IMAGE = "mern-frontend:jenkins"
        BACKEND_IMAGE = "mern-backend:jenkins"
        PORT = "5000"
        MONGO_URI = "mongodb://mongo:27017/taskdb"
    }

    stages {
        stage('Prepare .env') {
            steps {
                sh '''
                   mkdir -p server
                   cat > server/.env <<EOF
                   PORT=$PORT
                   MONGO_URI=$MONGO_URI
                   EOF
                   '''
            }
        }

        stage('Build Docker Images') {
            steps {
                sh '''
                echo "building backend image"
                docker build -t $BACKEND_IMAGE ./server
                echo "building frontend image"
                docker build -t $FRONTEND_IMAGE ./client --build-arg VITE_API_URL=http://localhost:5000/api
                '''
            }
        }

        stage('Run with Docker Compose') {
            steps {
                sh '''
                echo "Starting MERN app with Docker Compose"
                docker compose up -d

                echo "showing running containers"
                docker ps

                echo "====== showing backend logs ======"
                docker logs backend || true

                echo "====== showing frontend logs ======"
                docker logs frontend || true
                '''
            }
        }
    }
}
