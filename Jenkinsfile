pipeline {
  environment {
    imagename = "qltp/demojenkins"
    registry = "https://registry.hub.docker.com" 
    registryCredential = 'dockerhub_id"
    dockerImage = '' 
  } 
  agent any
        



    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        dockerImage = docker.build(imagename)
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
        docker.withRegistry(registry, registryCredential) {
            dockerImage.push("${env.BUILD_NUMBER}")
            dockerImage.push("latest")
        }
    }
}
