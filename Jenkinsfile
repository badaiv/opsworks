podTemplate(label: 'docker',
  containers: [
    containerTemplate(name: 'docker', image: 'badaiv/dockergc:latest', ttyEnabled: true, command: 'cat'),
  ],
  volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')]
  ) {

  def nginx_image = "nginx-lua"
  def app_image = "app_image"

  node('docker') {
      stage('get sources') {
          git 'https://github.com/badaiv/opsworks.git'
      }
      container('docker') {
          withDockerRegistry([credentialsId: 'docker.hub']) {
              stage('Build') {
                  sh "echo ${BUILD_NUMBER}"
                  sh "docker build -t ${nginx_image}:latest ."
              }
              stage('Dockerize') {
                  dir ('app'){
                      sh "docker build -t badaiv/${app_image}:latest ."
                      sh "docker tag badaiv/${app_image}:${BUILD_NUMBER} badaiv/${app_image}:latest"
                      sh "docker push badaiv/${app_image}:${BUILD_NUMBER}"
                      sh "docker push badaiv/${app_image}:latest"
                  }
              }
          }
      }
  }
}