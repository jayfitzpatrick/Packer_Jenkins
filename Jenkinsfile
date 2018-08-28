pipeline {
environment {
  myVar='myVAR'
            }

agent any

stages {
	stage ('Create Packer Image') {
    steps {
   		     checkout scm
          sh "cd /bitbucket/operating-systems/CentOS7/TemplateBuild;\
          rm -Rf  output-vmware-iso;\
          ./packer build -force -var-file=./variables.json ./packer.json"
	}
}

  stage ('Upload Packer Image to Openstack') {
    steps {
      sh "echo uploading to Openstack - TBC"
          }
}
}
}
post {
  always {
    cleanWs()
}
}
