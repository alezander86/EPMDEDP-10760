pipeline {
    agent {
        kubernetes {
            yaml '''
              apiVersion: v1
              kind: Pod
              spec:
                containers:
                - name: hadolint
                  image: hadolint/hadolint:latest-debian
                  imagePullPolicy: Always
                  command:
                  - cat
                  tty: true
              '''
        }
    }
    stages {
        stage('lint dockerfile') {
            steps {
                container('hadolint') {
                    sh 'hadolint dockerfiles/* | tee -a hadolint_lint.txt'
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
