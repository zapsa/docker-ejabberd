node {
  def app
  def reg = ~/^jenkins-/
  String str = env.BRANCH_NAME - reg
  String last = str.split('/')[0]
  echo "Image " + last + ":latest"
  stage('Clone repository') {
    checkout scm
  }

  stage('Build image') {
    app = docker.build("docker.squareboard.cloud/ejabberd")
  }

  stage('Push image') {
    docker.withRegistry('https://docker.squareboard.cloud', 'docker-reg-creds') {
      String shortCommit = sh(returnStdout: true, script: "git log -n 1 --pretty=format:'%h'").trim()
      app.push("${last}-${shortCommit}")
      app.push("${last}-latest")
    }
  }
}
