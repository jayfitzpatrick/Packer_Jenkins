
pipeline {
  options {
   // Only keep the 10 most recent builds
   buildDiscarder(logRotator(numToKeepStr:'10'))
 }
environment {
  // Use Packer as defined here: http://jenkinsserver:8080/configureTools/
    packer = tool name: 'packer-1.2.5', type: 'biz.neustar.jenkins.plugins.packer.PackerInstallation'
    tag = VersionNumber (versionNumberString: '${BUILD_DATE_FORMATTED, "yyyyMMdd"}-develop-${BUILDS_TODAY}')
          }

//tools {
//}

agent any

stages {
  stage ('Jenkinsfile Version') {
    steps {
        sh "echo ${tag}"
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
