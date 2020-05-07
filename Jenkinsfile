pipeline {
	agent any
	
	tools {
		go 'go-1.14'
	}

	environment {
		GO111MODULE = 'on'
		tag = "alexdias/store:$BUILD_NUMBER"
	}
	
	stages {
		stage('Install dependencies') {
			steps {
					sh 'go mod download'
			}
		}
		stage('Lint') {
			steps {
					sh 'golint ./...'
			}
		}
		stage('Test') {
			steps {
					sh 'go test -v ./...'
			}
		}

		stage('Docker build, push') {
			steps{
				script{
					image = docker.build tag
					image.push()
					image.push("latest")
				}

			}
		}

		stage('Deploy'){
			steps{
				sh 'cd devops && ./blue-green.sh store $tag app/deployment.yml'
			}
		}
	}
}