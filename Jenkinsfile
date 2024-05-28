node ('ubu-app-agent') {
  def app
  stage('Cloning Git') {
       checkout scm
     
  }
  
  stage('Build-and-Tag') {
    /* This builds the actual image; synonymous to
         * docker build on the command line */
        app = docker.build("dockme70/snake")
  }
  stage('Post-to-dockerhub') {
    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub_cred') {
      app.push("latest")
    }
  }  

  stage('Pull-image-server') {

         sh 'echo pulling image ...'
         sh "docker-compose down"
         sh "docker-compose up -d"

  }
}
