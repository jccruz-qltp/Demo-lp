pipeline {
  environment {
    imagename = "jesusqltp/demojenkins"
    registry = "https://registry.hub.docker.com" 
    registryCredential = "dockerhub_id"
    dockerImage = '' 
  } 
  agent any

      stages {
    stage('Clone repository') {
      /* Let's make sure we have the repository cloned to our workspace */
      steps {
        /*checkout scm*/
        checkout scmGit(branches
                        : [[name:'main']], userRemoteConfigs
                        : [[url:'https://github.com//jccruz-qltp/Demo-lp.git']])
      }
    }

    stage('Build image') {
      /* This builds the actual image; synonymous to
       * docker build on the command line */
      steps {
        script { dockerImage = docker.build(imagename) }
      }
    }

    /*stage('Test image') {
        /* Ideally, we would run a test framework against our image. */

    /* dockerImage.inside {
         sh 'echo "Tests passed"'
     }
 }*/

    stage('Push image') {
      /* Finally, we'll push the image with two tags:
       * First, the incremental build number from Jenkins
       * Second, the 'latest' tag.
       * Pushing multiple tags is cheap, as all the layers are reused. */
      steps {
        script {
          docker.withRegistry(registry, registryCredential) {
            dockerImage.push("${env.BUILD_NUMBER}") 
            dockerImage.push("latest")
          }
        }
      }
    }
  }
}
