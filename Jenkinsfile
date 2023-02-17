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
		
		stage('connecting to K8workstation'){
                    agent{
                         label 'tomcat-server'
                         }
                     steps{
                           sh 'ls /usr/local/bin/'
                           sh 'aws s3 ls'
                           sh 'kops get cluster --state=s3://k8s-trial-javacalci'
                           sh 'kubectl apply -f k8deployobj.yml'
                           sh 'kubectl get pods'
                           sh 'kubectl get services'
                       }
               }       
        
	}

	post {
		always {
			sh 'docker logout'
		}
	}
	
	
	
}
