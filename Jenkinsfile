pipeline {
    agent  {
		label 'master'
	}
      stage ("Checkout") {
               checkout scm
        }
      stage ("Lint dockerfile") {
        agent {
            docker {
                image 'hadolint/hadolint:latest-debian'
                label 'master'
                //image 'ghcr.io/hadolint/hadolint:latest-debian'
            }
        }
        steps {
            sh 'hadolint Dockerfile | tee -a hadolint_lint.txt'
        }
        post {
            always {
                archiveArtifacts 'hadolint_lint.txt'
            }
        }
        }
      stage('Building image') {
        steps{
          script {
          //dockerImage = docker.build("$registry:$BUILD_NUMBER")
            dockerImage = docker.build registry + ":latest" , "--network host ."
          }
        }
        }
      stage('Test image') {
        steps{
          sh "docker run -i $registry:latest"
        }
      }
}
