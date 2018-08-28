timestamps {

node () {
	stage ('Create Packer Image') {
   		env.myVar='myVAR'
    checkout scm
sh """
cd /bitbucket/operating-systems/CentOS7/TemplateBuild
rm -Rf  output-vmware-iso
./packer build -force -var-file=./variables.json ./packer.json
"""	}
  stage ('Upload Packer Image to Openstack') {
  echo uploading to Openstack - TBC
  }
}
}
