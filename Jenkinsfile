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
        timeout(time: 1, unit: 'HOURS')
    }

    environment {
        // https://stackoverflow.com/a/43264045
        HOME="."
    }

    stages {
        stage('Build') {
			steps {
				echo 'Building..'
				container("vscode-builder") {
					sh '''
					pwd
                    npm -v
                    ls -la
					npm ci
					npm run vscode:prepublish
					npm i vsce
					npx vsce package
					echo "Extension build complete"
					ls -la
					rm codewind-ls-node-prof-0.9.0.vsix
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