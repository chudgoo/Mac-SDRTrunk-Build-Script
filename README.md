#!/bin/bash
#Back up old install (excluding recordings)
#zip -r sdrtrunk-lastversion-backup.zip ~/sdrtrunk -x recordings
zip -r sdrtrunk-lastversion-backup.zip ~/sdrtrunk
#Unshit the bed if last run screwed up and left things in a weird state...
rm -rf ~/sdrtrunk
rm -rf ~/tempsdrtrunk
rm -rf ~/JMBE
#Get latest version of SDRTrunk and build/install.
mkdir ~/tempsdrtrunk
cd ~/tempsdrtrunk
git clone https://github.com/DSheirer/sdrtrunk
cd ./sdrtrunk/build/
ant
unzip -o ../product/sdrtrunk*.zip -d ~/
cd ~
rm -rf ~/tempsdrtrunk/
#Copy config files from local backup into new install dir.
yes | cp -rv ~/SDRTRUNK-CONFIG-FILES/* ~/sdrtrunk
#Get latest version of JMBE and copy to sdtrunk dir
git clone https://github.com/DSheirer/JMBE
cd ~/JMBE/jmbe/build/
ant
yes | cp ../library/jmbe-0*.jar ~/sdrtrunk
cd ~
rm -rf ~/JMBE/
#Make it so.
cd ~/sdrtrunk
bash ./run_sdrtrunk_linux.sh
