#!groovy​
pipeline {
    agent {
        kubernetes {
            label 'vscode-buildpod'
            yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: vscode-builder
    image: node:lts
    tty: true
    command:
      - cat
"""
        }
    }

	 options {
        timestamps()
        skipStagesAfterUnstable()
    }

    stages {
        stage('Build') {
			steps {
				echo 'Building..'
				container("vscode-builder") {
					sh '''
					pwd
					npm ci
					npm run vscode:prepublish
					npm i vsce
					npx vsce package
					echo "Extension build complete"
					ls -la
					rm codewind-ls-node-prof-19.3.0.vsix
					ls -la
				'''
				}
            }
        }

        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}