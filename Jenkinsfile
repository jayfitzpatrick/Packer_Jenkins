timestamps {

node () {

	stage ('Create Minimal Boot ISO') {
 		env.myVar='findme'
    checkout scm
sh """
# cd /tmp
sudo yum install mkisofs -y
if [[ ! -e CentOS-7-x86_64-NetInstall-1804.iso ]]; then
			wget http://10.201.45.99/pub/CentOS-7-x86_64-NetInstall-1804.iso
fi

if [[ ! -e extracted ]]; then
    mkdir extracted
elif [[ ! -d extracted ]]; then
    echo "extracted already exists but is not a directory" 1>&2
fi
if mount |grep extracted > /dev/null; then
			sudo umount extracted
fi
sudo mount -o loop ./CentOS-7-x86_64-NetInstall-1804.iso ./extracted
if [[ ! -e bootcd ]]; then
    mkdir bootcd
elif [[ ! -d bootcd ]]; then
    echo "bootcd already exists but is not a directory" 1>&2
fi
sudo /usr/bin/cp -r ./extracted/isolinux ./bootcd/
sudo /usr/bin/cp isolinux.cfg ./bootcd/isolinux/isolinux.cfg
cd bootcd/

sudo mkisofs -o RXP-kickstart.iso -b isolinux.bin -c boot.cat -no-emul-boot -boot-load-size 4 -boot-info-table -R -J -v -T isolinux/
cd ..
sudo umount extracted
logger "${env.myVar}"
 """
	}
	stage ('Create Packer Image') {
   		env.myVar='myVAR'
sh """
cd /bitbucket/operating-systems/CentOS7/TemplateBuild
./packer build -force -var-file=./variables.json ./packer.json
"""
	}
}
}
