pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh '''
		        docker build -t eu.gcr.io/lbg-cloud-incubation/nwsimpleflask:latest -t eu.gcr.io/lbg-cloud-incubation/nwsimpleflask:build-$BUILD_NUMBER .
		        '''
            }
        }
        stage('Push') {
            steps {
                sh '''
		        docker push eu.gcr.io/lbg-cloud-incubation/nwsimpleflask:latest
		        docker push eu.gcr.io/lbg-cloud-incubation/nwsimpleflask:build-$BUILD_NUMBER
		        '''
            }
        }
	stage('Stop and Clean') {
            steps {
                script {
			        if ("${GIT_BRANCH}" == 'origin/main') {
				        sh '''
				        ssh -i '/home/jenkins/.ssh/id_rsa' jenkins@35.246.70.5 << EOF
				        docker rm -f nwflaskapp
                        
				        '''
						//docker rmi eu.gcr.io/lbg-cloud-incubation/nwsimpleflask:latest
			        } else if ("${GIT_BRANCH}" == 'origin/development') {
				        sh '''
				        ssh -i '/home/jenkins/.ssh/id_rsa' jenkins@34.105.233.150 << EOF
				        docker rm -f nwflaskapp
                        
				        '''
						//docker rmi eu.gcr.io/lbg-cloud-incubation/nwsimpleflask:latest
			}
		}
            }
        }
        stage('Deploy') {
            steps {
                script {
			        if ("${GIT_BRANCH}" == 'origin/main') {
				        sh '''
				        ssh -i '/home/jenkins/.ssh/id_rsa' jenkins@35.246.70.5 << EOF
				        docker run -d -p 80:5500 --name nwflaskapp eu.gcr.io/lbg-cloud-incubation/nwsimpleflask:latest
				        '''
			        } else if ("${GIT_BRANCH}" == 'origin/development') {
				        sh '''
				        ssh -i '/home/jenkins/.ssh/id_rsa' jenkins@34.105.233.150 << EOF
				        docker run -d -p 80:5500 --name nwflaskapp eu.gcr.io/lbg-cloud-incubation/nwsimpleflask:latest
				        '''
			        }
                }
            }
        }
    }
}