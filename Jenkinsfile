node {
  stage ('SCM Checkout') {
    checkout scm
  }
  
  stage ('Docker image build') {
    sh 'docker build -t suman/flaskapp .'
  }
  
  stage('Push image to Docker Registry') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
  
  stage ('Application deploy on Docker') {
    sh "docker run -d -p 7373:5000 flaskapp"
  }

  stage ('Launch Info') {
    echo "=================="
    sh "docker ps -a"
  }
}
