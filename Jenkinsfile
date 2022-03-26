pipeline {
    agent { label 'agent_1' }
    environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub-cred-ronen232')
	}
    stages {
        stage('Build docker image'){
            steps{
                sh 'docker build -t moditamam/selenium:from-jenkins-pipeline .'
            }
        }
        stage('Run & Test') {
            agent {
                docker {
                    image "moditamam/selenium:from-jenkins-pipeline"
                    reuseNode true
                }
            }
            steps {
                sh 'python3 /app/MainScores.py &'
                sh 'sleep 5'
                sh 'python3 e2e.py'
            }
        } 
        stage('Login to Docker Hub') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}

		stage('Push docker to DockerHub') {

			steps {
				sh 'docker push moditamam/selenium:from-jenkins-pipeline'
			}
		}
	}

	post {
		always {
			sh 'docker logout'
		}
	} 
}
