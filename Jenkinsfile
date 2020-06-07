node {
  def app
  stage ('SCM Checkout') {
    checkout scm
  }
  
  stage ('Docker image build') {
    app = docker.build("prathaps/flaskapp")
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
  
  stage ('Application deploy on kubernetes') {
    sh "kubectl create deployment pythonflask --image=prathaps/flaskapp"
  }

  stage ('Launching the service') {
    echo "=================="
    sh "kubectl expose deployment pythonflask --name pythonappservice --port 5000 --type NodePort"
  }
}
