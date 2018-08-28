pipeline {
  options {
   // Only keep the 10 most recent builds
   buildDiscarder(logRotator(numToKeepStr:'10'))
 }
environment {
  // Use Packer as defined here: http://jenkinsserver:8080/configureTools/
    packer = tool name: 'packer-1.2.5', type: 'biz.neustar.jenkins.plugins.packer.PackerInstallation'
          }

//tools {
//}

agent any

stages {
  stage ('packer Version') {
    steps {
        sh 'packer --version'
        }
      }
	stage ('Create Packer Image') {
    steps {
   		     checkout scm
          sh "cd /bitbucket/operating-systems/CentOS7/TemplateBuild/ ;\
          sudo rm -Rf  output-vmware-iso;\
          sudo ${Packer}/packer build -force -var-file=./variables.json ./packer.json"
	}
}

  stage ('Upload Packer Image to Openstack') {
    steps {
      sh "echo uploading to Openstack - TBC"
          }
}
}
post {
  always {
    cleanWs()
}
}
}
