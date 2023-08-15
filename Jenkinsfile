//Declarative
pipeline{
	agent any
	//agent{ docker {image 'maven:3.8.2'} }
	//agent{ docker { image 'alpine:3.17'} }
	environment {
		dockerHome= tool 'myDocker'
		mavenHome= tool 'myMaven'
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
	}
	
	stages{
		stage('Checkout'){
			steps{
				sh 'mvn --version'
				sh 'docker version'
				echo "Build"
				echo "PATH - $PATH"
				echo "BUILD_NUMBER - $env.BUILD_NUMBER"
				echo "BUILD_ID - $env.BUILD_ID"
				echo "JOB_NAME - $env.JOB_NAME"
				echo "BUILD_TAG - $env.BUILD_TAG"
				echo "BUILD_URL - $env.BUILD_URL"

			}

		}
		stage('Compile'){
			steps{
//				sh "mvn clean compile"
                echo "Compile"

			}
		}
		stage('Test'){
			steps{
//				sh "mvn test"
                echo "Test"
			}
		}
		stage('Integration Test'){
			steps{
//				sh "mvn failsafe:integration-test failsafe:verify"
                echo "Integration Test"
			}
		}
		stage('Package'){
			steps{
//				sh "mvn failsafe:integration-test failsafe:verify"
                sh "mvn package -DskipTests"
			}
		}		
		stage('Build Docker Image'){
			steps {
				//"docker build -t kamal05/currency-exchange-devops:$env.BUILD_ID"
				script{
					dockerImage = docker.build("kamal05/currency-exchange-devops:${env.BUILD_TAG}")
				}

			}
		}

		stage('Push Docker Image'){
			steps {
				script{
					docker.withRegistry('','dockerhub'){

                         dockerImage.push();
					     dockerImage.push('latest');
					}

				}

			}
		}


		  
	} 
	post{
		always{
			echo "I am awesome ! I run always"
		}
		success{
			echo "I run when you are successful"
		}
		failure{
			echo "I run when you fail !"
		}
	}
}