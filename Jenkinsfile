pipeline {
    agent { label 'agent_1' }
    environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub-cred-ronen232')
	}
    stages {
        stage('Build docker image'){
            steps{
                sh 'docker build -t ronen232/wog-level4:latest .'
            }
        }
        stage('Run & Test') {
            agent {
                docker {
                    image "ronen232/wog-level4:latest"
                    reuseNode true
                }
            }
            steps {
                sh 'python3 /app/MainScores.py &'
                sh 'sleep 5'
                sh 'python3 tests/e2e.py'
            }
        } 
        stage('Login to Docker Hub') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}

		stage('Push docker image to DockerHub') {

			steps {
				sh 'docker push ronen232/wog-level4:latest'
			}
		}
	}

	post {
		always {
			sh 'docker logout'
		}
	} 
}
