pipeline {
    agent {
        docker {
            image 'docker:20.10.7'
            args '-v /var/run/docker.sock:/var/run/docker.sock --user root'
        }
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/sareefhub/jenkins-demo-app.git'
            }
        }
        stage('Build Image') {
            steps {
                sh 'docker build -t jenkins-demo-app:latest .'
            }
        }
        stage('Test') {
            steps {
                sh 'echo "Running tests..."'
                // รัน pytest ใน container ที่ build ขึ้นมา
                sh 'docker run --rm jenkins-demo-app:latest pytest || true'
            }
        }
        stage('Run Container') {
            steps {
                // ลบ container เก่าถ้ามี
                sh 'docker rm -f demo-app || true'
                // รันใหม่ที่ port 8081
                sh 'docker run -d -p 8081:8081 --name demo-app jenkins-demo-app:latest'
            }
        }
    }
}
