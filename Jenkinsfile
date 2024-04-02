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
    volumeMounts:
      - name: jenkins-docker-cfg
        mountPath: /docker
  volumes:
    - name: jenkins-docker-cfg
      projected:
        sources:
          - secret:
              name: docker-credentials
              items:
                - key: .dockerconfigjson
                  path: config.json
'''
        }
    }
    stages {
        stage('Docker Build') {
            environment{
                $DOCKER_CONFIG = '/docker'
            }
            steps {
                container('docker'){
                sh '''
                printenv
                docker buildx create --name buildkit --driver=kubernetes --driver-opt=namespace=buildkit,rootless=true --use
                docker buildx build --push --progress plain -t janivirtanen/buildkit-test:latest .
                '''
                }
            }
        }
    }
}
