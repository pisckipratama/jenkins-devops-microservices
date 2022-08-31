
pipeline {
  agent none

  stages {
    stage('Install') {
      agent {
        docker {
          image 'maven:3.6.3-openjdk-17-slim'
        }
      }

      steps {
				sh "mvn clean compile"
      }
    }

    stage('Test') {
			agent {
        docker {
          image 'maven:3.6.3-openjdk-17-slim'
        }
      }

      steps {
        sh "mvn test"
      }
    }

		stage('Integration Test') {
			agent {
        docker {
          image 'maven:3.6.3-openjdk-17-slim'
        }
      }

			steps {
				sh "mvn failsafe:integration-test failsafe:verify"
			}
		}

    stage('Build Docker Image') {
      steps {
        script {
          dockerImage = docker.build("pisckitama/tamakopi:latest");
        }
      }
    }

    // stage('Push Docker Image') {
    //   when {
    //     anyOf {
    //       branch "develop"
    //       branch "release/*"
    //       branch "main"
    //     }
    //   }

    //   steps {
    //     script {
    //       docker.withRegistry('', 'dockerhub') {
    //         dockerImage.push();
    //       }
    //     }
    //   }
    // }

    // stage('Deploy') {
    //   when {
    //     anyOf {
    //       branch "develop"
    //       branch "release/*"
    //       branch "main"
    //     }
    //   }

    //   agent {
    //     docker {
    //       image 'joshendriks/alpine-k8s'
    //     }
    //   }

    //   steps {
    //     script {
    //       if (env.BRANCH_NAME == "develop") {
    //         echo "this is running on develop"
    //         withCredentials([file(credentialsId: 'artopologi-cluster', variable: 'KUBECONFIG')]) {
    //           sh 'kubectl set image --record deployment/${BACKEND_APP} ${BACKEND_APP}=${IMAGE_NAME}'
    //         }
    //       }

    //       if (env.BRANCH_NAME.contains('release')) {
    //         echo "this is running on release"
    //         withCredentials([file(credentialsId: 'artopologi-cluster', variable: 'KUBECONFIG')]) {
    //           sh 'kubectl set image --record -n staging deployment/${BACKEND_APP} ${BACKEND_APP}=${IMAGE_NAME_RELEASE}'
    //         }
    //       }
    //     }
    //   }
    // }

    // stage('Delete Docker Image') {
    //   when {
    //     anyOf {
    //       branch "develop"
    //       branch "release/*"
    //       branch "main"
    //     }
    //   }

    //   steps {
    //     script {
    //       node {
    //         if (env.BRANCH_NAME == "develop") {
    //           echo "this is running on develop"
    //           sh "docker rmi -f ${IMAGE_NAME}"
    //         }

    //         if (env.BRANCH_NAME.contains('release')) {
    //           echo "this is running on release ${branch_version}"
    //           sh "docker rmi -f ${IMAGE_NAME_RELEASE}"
    //         }
    //       }
    //     }
    //   }
    // }
  }
}
