// Uses Declarative syntax to run commands inside a container.
pipeline {
    agent {
        kubernetes {
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: buildkit
    image: docker
    command:
    - sleep
    args:
    - infinity
'''
            defaultContainer 'buildkit'
        }
    }
    stages {
        stage('Docker Build') {
            steps {
                sh '''
                docker buildx create --name buildkit --driver=kubernetes --driver-opt=namespace=buildkit,rootless=true --use
                docker buildx build --progress plain -t local-test:1 .
                '''
            }
        }
    }
}
