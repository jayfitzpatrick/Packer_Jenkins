node () {

	stage ('Create Packer Image') {
 		env.myVar='myVAR'
sh """
cd /bitbucket/operating-systems/CentOS7/TemplateBuild
./packer build -force -var-file=./variables.json ./packer.json
"""
	}
}
