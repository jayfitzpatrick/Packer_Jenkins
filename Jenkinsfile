
pipeline {
  options {
   // Only keep the 10 most recent builds
   buildDiscarder(logRotator(numToKeepStr:'11'))
 }
environment {
  // Use Packer as defined here: http://jenkinsserver:8080/configureTools/
    packer = tool name: 'packer-1.2.5', type: 'biz.neustar.jenkins.plugins.packer.PackerInstallation'
    OS_Build_Version = VersionNumber (versionNumberString: 'CentOS7.5-${BUILDS_TODAY}-${BUILD_DATE_FORMATTED, "ddMMyyyy"}')
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
      //sh "openstack --insecure image set centos-latest --name ${OS_Build_Version}"
      sh "echo openstack --insecure image create --disk-format vmdk --file /bitbucket/operating-systems/CentOS7/TemplateBuild/output-vmware-iso/packer-vmware-iso-disk1.vmdk centos-latest"
          }
}
}
post {
  always {
    cleanWs()
}
failure {
    echo "mail to: jason.fitzpatrick@realexpayments.com, subject: 'The Pipeline failed :('"
}
}
}
