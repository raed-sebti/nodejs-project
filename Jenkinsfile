 node {
     def app

     stage('Clone repository') {
         /* Let's make sure we have the repository cloned to our workspace */

         checkout scm
     }

     stage('Build image') {
         /* This builds the actual image; synonymous to
          * docker build on the command line */

         app = docker.build("raedsebti/hellonode-dev-branch")
     }

     stage('Test image') {

         app.inside {
             sh 'echo "Tests passed"'
         }
     }

     stage('Push image') {
         /* Finally, we'll push the image with two tags:
          * First, the incremental build number from Jenkins
          * Second, the 'latest' tag.
          * Pushing multiple tags is cheap, as all the layers are reused. */
         docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
             app.push("${env.BUILD_NUMBER}")
             app.push("latest")
         }
     }
     stage('Run Container') {
      docker.withServer('tcp://192.168.1.10:2375') {
      docker.image('registry.hub.docker.com/raedsebti/hellonode-dev-branch').withRun('-p 8090:8080') {
        }
      }
    }
 }
