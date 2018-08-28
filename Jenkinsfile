pipeline {
  options {
   // Only keep the 10 most recent builds
   buildDiscarder(logRotator(numToKeepStr:'3'))
 }
environment {
  myVar='BUILD_HOME=/bitbucket/operating-systems/CentOS7/TemplateBuild'
  // Use Packer which is set here: http://jenkinsserver:8080/configureTools/
    def Packer = '/var/lib/jenkins/biz.neustar.jenkins.plugins.packer.PackerInstallation/Packer'
    packer.tool = "Packer-1.2.5"
          }

agent any

stages {
	stage ('Create Packer Image') {
    steps {
   		     checkout scm
          sh "cd /bitbucket/operating-systems/CentOS7/TemplateBuild;\
          sudo rm -Rf  output-vmware-iso;\
          sudo $Packer build -force -var-file=./variables.json ./packer.json"
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
