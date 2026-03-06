pipeline {
    agent any

    environment {
        IMAGE = "2022bcs0121sreeja/wine-quality-api"
        CONTAINER = "wine-test"
    }

    stages {

        stage('Pull Image') {
            steps {
                sh 'docker pull $IMAGE'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 8000:8000 --name $CONTAINER $IMAGE'
            }
        }

        stage('Wait for API') {
            steps {
                sh 'sleep 25'
            }
        }

        stage('Valid Inference Request') {
            steps {
                sh '''
                curl -X POST http://wine-test:8000/predict \
                -H "Content-Type: application/json" \
                -d '{
                    "alcohol":10.5,
                    "sulphates":0.65,
                    "volatile_acidity":0.3,
                    "density":0.996,
                    "chlorides":0.05,
                    "total_sulfur_dioxide":120,
                    "fixed_acidity":7.4,
                    "pH":3.2
                }'
                '''
            }
        }

        stage('Invalid Request') {
            steps {
                sh '''
                curl -X POST http://localhost:8000/predict \
                -H "Content-Type: application/json" \
                -d '{"wrong":"data"}'
                '''
            }
        }

        stage('Stop Container') {
            steps {
                sh '''
                docker stop $CONTAINER
                docker rm $CONTAINER
                '''
            }
        }

    }
}
