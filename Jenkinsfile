#!groovy

node("spot") {

  def app

  stage("Clone repo") {
    checkout([
       $class: 'GitSCM',
       branches: scm.branches,
       doGenerateSubmoduleConfigurations: scm.doGenerateSubmoduleConfigurations,
       extensions: scm.extensions,
       userRemoteConfigs: scm.userRemoteConfigs
      ])
  }

  stage("Build container") {
    app = docker.build("saidsef/docker-varnish")
  }

  stage("Push image") {
    docker.withRegistry("https://registry.hub.docker.com", "docker-hub") {
      app.push("${env.BUILD_NUMBER}")
      app.push("latest")
      try {
        sh "docker stop docker-varnish"
        sh "docker rm docker-varnish"
      } catch(Exception _) {
        echo "Nothing to remove"
      }
    }
  }
}
