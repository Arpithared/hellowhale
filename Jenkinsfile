pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/Arpithared/hellowhale.git', branch:'master'
      }
    }
    
      stage("Build image") {
            steps {
                script {
                    myapp = docker.build("arpithadockerhub/hellowhale:${env.BUILD_ID}")
                }
            }
        }
    
      stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'acf3e158-74c1-44b7-afe5-37075a124eeb') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

    
    stage('Deploy App') {
      steps {
        script {
          //kubernetesDeploy(configs: "hellowhale.yml", kubeconfigId: "mykubeconfig")
          sh 'sed -i \'s/BUILDNUMBER/\'$BUILD_ID\'/g\' hellowhale.yml'
          sh 'cat hellowhale.yml'
          sh 'kubectl apply -f hellowhale.yml'
        }
      }
    }

  }

}
