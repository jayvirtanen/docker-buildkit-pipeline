pipeline {
    agent {
        kubernetes {
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: docker
    image: docker
    command:
    - sleep
    args:
    - infinity
'''
        }
    }
    stages {
        stage('Docker Build') {
            steps {
                container('docker'){
                sh '''
                docker buildx create --name buildkit --driver=kubernetes --driver-opt=namespace=buildkit,rootless=true --use
                docker buildx build --progress plain -t local-test:1 .
                docker buildx ls
                '''
                }
            }
        }
    }
}
