pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh '''
		        docker build -t eu.gcr.io/lbg-cloud-incubation/simpleflask:latest -t eu.gcr.io/lbg-cloud-incubation/simpleflask:build-$BUILD_NUMBER .
		        '''
            }
        }
        stage('Push') {
            steps {
                sh '''
		        docker push eu.gcr.io/lbg-cloud-incubation/simpleflask:latest
		        docker push eu.gcr.io/lbg-cloud-incubation/simpleflask:build-$BUILD_NUMBER
		        '''
            }
        }
	stage('Stop and Clean') {
            steps {
                script {
			        if ("${GIT_BRANCH}" == 'origin/main') {
				        sh '''
				        ssh -i '/home/jenkins/.ssh/pipeline-key' jenkins@34.163.170.174 << EOF
				        docker rm -f flaskapp
				        '''
                        // docker rmi eu.gcr.io/lbg-cloud-incubation/simpleflask:latest
			        } else if ("${GIT_BRANCH}" == 'origin/development') {
				        sh '''
				        ssh -i '/home/jenkins/.ssh/pipeline-key' jenkins@34.163.152.140 << EOF
				        docker rm -f flaskapp
				        '''
                        // docker rmi eu.gcr.io/lbg-cloud-incubation/simpleflask:latest
			}
		}
            }
        }
        stage('Deploy') {
            steps {
                script {
			        if ("${GIT_BRANCH}" == 'origin/main') {
				        sh '''
				        ssh -i '/home/jenkins/.ssh/pipeline-key' jenkins@34.163.170.174 << EOF
				        docker run -d -p 80:5500 --name flaskapp eu.gcr.io/lbg-cloud-incubation/simpleflask:latest
				        '''
			        } else if ("${GIT_BRANCH}" == 'origin/development') {
				        sh '''
				        ssh -i '/home/jenkins/.ssh/pipeline-key' jenkins@34.163.152.140 << EOF
				        docker run -d -p 80:5500 --name flaskapp eu.gcr.io/lbg-cloud-incubation/simpleflask:latest
				        '''
			        }
                }
            }
        }
    }
}