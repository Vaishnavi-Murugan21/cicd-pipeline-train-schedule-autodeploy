pipeline {
    agent any
    environment{
        dockerImage = ''
        registry = 'vaishnavi2199/nodejsproject'
        registryCrendential = 'dockerhub'
    }
   stages{
        stage ('checkout'){
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Vaishnavi-Murugan21/cicd-pipeline-train-schedule-autodeploy.git']])
            }
        }
      stage ('Build Docker image')  {
          steps {
              script {
                  dockerImage = docker.build registry
              }
          }
      }
      stage ('push the docker image'){
          steps {
              script {
                  docker.withRegistry('', registryCrendential ) {
                      dockerImage.push()
                  }
              }
          }
      }
    stage ('Deploying application to kubernetes') {
        steps {
            script {
                kubernetesDeploy (configs: 'train-schedule-kube.yml', kubeConfig: [path: ''], kubeconfigId: 'kubeconfig1', secretName: '', ssh: [sshCredentialsId: '*', sshServer: ''], textCredentials: [certificateAuthorityData: '', clientCertificateData: '', clientKeyData: '', serverUrl: 'https://'])
            }
        }
    }
  }
}
