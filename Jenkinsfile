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
    docker.withRegistry("https://registry.hub.docker.com", "dockerHub") {
      app.push("${env.BUILD_NUMBER}")
      app.push("latest")
      try {
        sh "docker rmi ${app.id} -f"
        sh "docker rmi registry.hub.docker.com/${app.id} -f"
      } catch(Exception _) {
        echo "Nothing to remove"
      } finally {
        deleteDir()
      }
    }
  }
}
