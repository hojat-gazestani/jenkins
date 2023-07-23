pipeline {
  agent {
    kubernetes {
      yaml """
kind: Pod
spec:
  containers:
  - name: kaniko
    image: gcr.io/kaniko-project/executor:debug
    imagePullPolicy: Always
    command:
    - sleep
    args:
    - 9999999
    volumeMounts:
      - name: jenkins-docker-cfg
        mountPath: /kaniko/.docker
  volumes:
  - name: jenkins-docker-cfg
    projected:
      sources:
      - secret:
          name: docker-credentials
          items:
            - key: .dockerconfigjson
              path: config.json
"""
        }
    }
    stages {
        stage('Clone repository') {
            steps {
                container('jnlp-kaniko') {
                    git branch: 'main', url: 'https://github.com/hojat-gazestani/kubernetescode.git'
                }
            }
        }
        stage('Build image with Kaniko') {
            steps {
                container('kaniko') {
                    sh '''#!/busybox/sh
                        /kaniko/executor --context `pwd` --dockerfile `pwd`/Dockerfile --destination hojatgazni/hello-kaniko:latest --verbosity=debug
                    '''
                }
            }
        }
    }
}

