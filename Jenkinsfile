
pipeline {
  options {
   // Only keep the 10 most recent builds
   buildDiscarder(logRotator(numToKeepStr:'10'))
 }
environment {
  // Use Packer as defined here: http://jenkinsserver:8080/configureTools/
    packer = tool name: 'packer-1.2.5', type: 'biz.neustar.jenkins.plugins.packer.PackerInstallation'
    OS_Build_Version = VersionNumber (versionNumberString: 'CentOS7.5-${BUILDS_TODAY}-${BUILD_DATE_FORMATTED, "ddMMyyyy"}')
    def secrets = [
         [$class: 'VaultSecret', path: 'secret/testing', secretValues: [
             [$class: 'VaultSecretValue', envVar: 'testing', vaultKey: 'value_one'],
             [$class: 'VaultSecretValue', envVar: 'testing_again', vaultKey: 'value_two']]],
         [$class: 'VaultSecret', path: 'secret/another_test', secretValues: [
             [$class: 'VaultSecretValue', envVar: 'another_test', vaultKey: 'value']]]
     ]

     // optional configuration, if you do not provide this the next higher configuration
     // (e.g. folder or global) will be used
     def configuration = [$class: 'VaultConfiguration',
                          vaultUrl: 'http://my-very-other-vault-url.com',
                          vaultCredentialId: 'my-vault-cred-id']

     // inside this block your credentials will be available as env variables
     wrap([$class: 'VaultBuildWrapper', configuration: configuration, vaultSecrets: secrets]) {
         sh 'echo $testing'
         sh 'echo $testing_again'
         sh 'echo $another_test'
     }
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
    echo mail to: jason.fitzpatrick@realexpayments.com, subject: 'The Pipeline failed :('
}
}
}
