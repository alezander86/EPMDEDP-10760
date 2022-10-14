pipeline {
    agent {
        kubernetes {
            yaml '''
              apiVersion: v1
              kind: Pod
              spec:
                containers:
                - name: hadolint
                  image: hadolint/hadolint:a6a398942a8a28e5a34d9f680712d1554af999e6-debian-amd64
                  imagePullPolicy: Always
                  command:
                  - cat
                  tty: true
              '''
        }
    }
    stages {
        stage('Checkout'){
            steps {
                git branch: 'main', url: 'https://github.com/alezander/python_app_10760.git'
            }
        }         
        stage('lint dockerfile') {
            steps {
                container('hadolint') {
                    sh 'hadolint dockerfile/* | tee -a hadolint_lint.txt'
                }
            }
            post {
                always {
                    archiveArtifacts 'hadolint_lint.txt'
                }
            }
        }
    }
}
