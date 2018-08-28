pipeline {
environment {
  myVar='BUILD_HOME=/bitbucket/operating-systems/CentOS7/TemplateBuild'
            }

agent any

stages {
	stage ('Create Packer Image') {
    steps {
   		     checkout scm
          sh "cd /bitbucket/operating-systems/CentOS7/TemplateBuild;\
          sudo rm -Rf  output-vmware-iso;\
          sudo ./packer build -force -var-file=./variables.json ./packer.json"
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
