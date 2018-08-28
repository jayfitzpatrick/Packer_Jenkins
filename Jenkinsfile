timestamps {

node () {

	stage ('Manual Creation of BootISO - Build') {
 		env.myVar='findme'
sh """
cd /tmp
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
cd bootcd/
sudo wget https://raw.githubusercontent.com/jayfitzpatrick/Build_Centos_Netboot/master/isolinux.cfg -O ./isolinux/isolinux.cfg
sudo mkisofs -o RXP-kickstart.iso -b isolinux.bin -c boot.cat -no-emul-boot -boot-load-size 4 -boot-info-table -R -J -v -T isolinux/
cd ..
sudo umount extracted
logger "${env.myVar}"
 """
	}
}
	stage ('Create Packer Image') {
    checkout scm
 		env.myVar='myVAR'
sh """
cd /bitbucket/operating-systems/CentOS7/TemplateBuild
./packer build -force -var-file=./variables.json ./packer.json
"""
	}
}
