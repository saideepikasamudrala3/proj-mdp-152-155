pipeline{

	agent any

	environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub-cred-deepika')
	}

	stages {

		stage('Build') {

			steps {
				sh 'docker build -t deepikasamudrala/calculator_java:latest .'
			}
		}

		stage('Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}


        stage('Removeoldercontainer') {

			steps {
				sh 'docker container rm -f calci_java'
			}
		}


        stage('createcontainer') {

			steps {
				sh 'docker run -dt --name calci_java -p 9000:8080 -i deepikasamudrala/calculator_java:latest '
			}
		}
        


		stage('Push') {

			steps {
				sh 'docker push deepikasamudrala/calculator_java:latest'
			}
		}
	}

	post {
		always {
			sh 'docker logout'
		}
	}
	
	agent  { label 'tomcat-server' }
	
	stages {
		
		  stage('deploy') {
                    steps {
                              
                           sudo kubectl --kubeconfig ${WORKSPACE}/k8_config config set-context --current --user=jenkins-admin
                            sudo kubectl apply -f k8deployobj.yml --kubeconfig ${WORKSPACE}/k8_config -n devops-tools
                         }
                    }
	}
}
