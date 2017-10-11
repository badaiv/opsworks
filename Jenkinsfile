podTemplate(label: 'docker',
  containers: [
    containerTemplate(name: 'docker', image: 'badaiv/dockergc:latest', ttyEnabled: true, command: 'cat'),
  ],
  volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')]
  ) {
  def image = "nginx-lua"
  def projectId = "plated-complex-147512"
  node('docker') {
    stage('Build Docker image') {
      git 'https://github.com/badaiv/opsworks.git'
      container('docker') {
          withDockerRegistry([credentialsId: 'c9d130d4-a6b5-4298-9c76-b3edaf6386ed']) {
            sh "echo ${BUILD_NUMBER}"
            sh "docker build -t badaiv/${image}:${BUILD_NUMBER} ."
            sh "docker tag badaiv/${image}:${BUILD_NUMBER} badaiv/${image}:latest"
            sh "docker push badaiv/${image}:${BUILD_NUMBER}"
            sh "docker push badaiv/${image}:latest"
        }
      }
    }
  }
}