# ConfiguringUbuntu22.04
Diary of what I learned in newly dual booting my Ubuntu22.04 alongside with Windows11 on Dell Latitude 5450 i5
![Uploading image.png…]()


May21 Note-1

Installing chrome (something not in official Ubuntu repository)

wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo dpkg -i google-chrome-stable_current_amd64.deb
sudo apt --fix-broken install

Because I am dpkg installing something outside of the official repository, sudo apt --fix-broken is used to fix dependencies required by chrome. 

sudo apt-get install -f

The above command is subtly different. It attempts to fix the dependency and resume any halted installation. 

May21 Note-2

Making an USB for an EFI application

sudo umount /dev/sdb1
sudo mkfs.fat /dev/sdb1
sudo mkdir -p /mnt/usbmem
sudo mount /dev/sdb1 /mnt/usbmem
sudo mkdir -p /mnt/usbmem/EFI/BOOT
sudo cp BOOTX64.EFI /mnt/usbmem/EFI/BOOT
sudo umount /mnt/usbmem

replace "sdb1" with the device name of the USB. 

USB device name is usually in the format of "sdx". 'x' can be 'a', 'b', etc.
Internal HDD also has its dev name in the format of "sdx". The name "sd" came from the SATA connection that HDD has with the motherboard. 
USB uses USB3.0 or other standards instead of SATA/NVMe, but because it behaves just like any other external block storage device connected,USB is named "sdx". 
In contrast, Internal SSD uses NVMe. That's why it's device name is in the format of "nvme_specific ID". 
However, if the SSD is external and connected via USB, it will probably be assigned a "nmve" type of dev name. 

The above commands has the following effect: 
Unmount the usb device from the default mount point to format it into FAT in the next command. 
Create a directory "/mnt/usbmem", to which the usb will be mounted. 
Create EFI/BOOT in the root directory (?) of the USB and put the EFI program in it, so that the EFI program is loaded when the USB partition containing
the EFI/BOOT directory is booted. 
Finally, unmount the device to safely remove the USB flash drive. 

May21 Note-3

Changing the time setting of Ubuntu

Issue of dual-booting Windows11 and Ubuntu22.04. https://askubuntu.com/questions/869639/windows-time-jumps-a-few-hours-forward-after-using-ubuntu

Ubuntu set the hardware clock (BIOS) to UTC. However, Windows11 use the local time. When I boot back from Ubuntu to Windows, the time displayed in Windows is affected 
because of this. Therefore, one can fix either Ubuntu or Windows to let one conform the other. 

I changed the Ubuntu's hardware clock setting to local time by:
sudo timedatectl set-local-rtc 1 

May21 Note-4

Allocating and unallocating disk space for Windows and Ubuntu

In Ubuntu, the tool to use is GParted. 

In Windows, surprisingly, it is okay to shrink the C partition without deleting system and program files. Modern OS knows how to handle the complicated operations 
and ensure the data is not deleted. This is true as long as you are not shrinking the partition smaller than or almost equal to the size of the data stored in it. 

My allocation of memory (500GB) is the following: 

Windows ~300GB
Unallocated ~100GB
Ubuntu ~100GB

Since the unallocated is adjacent to both Windows and Ubuntu partition, it can be allocated to these two OS easily. 

If these partitions are not adjacent, I can also use tools like Aomei to reorder the partitions. 

https://answers.microsoft.com/en-us/windows/forum/all/cannot-extend-volume-greyed-out-to-merge/4b02823b-c67a-44a9-9095-46b7d2ff0598

May24 Note-1

Important Java Spring Web development related links/terminologies

Maven Plugins to make executable jar and copy dependency to other projects
https://mkyong.com/maven/how-to-create-a-manifest-file-with-maven/

Maven transitive dependency. 
Conflicts should be solved by specifying a version. Use Maven analyze tools to find redundant dependencies and exclude them.'

Maven installation guide on Ubuntu 22.04 
https://vegastack.com/tutorials/how-to-install-maven-on-ubuntu-22-04/

Class path 
Classes (usually packed in jar) required by your project. If the project is small, you can specify it in CLI during built and run.
In Maven, the dependency artifacts themselves are the indicators of class paths. 

Manifest
A text file included in every JAR that provides metadata about the contents. One commonly used attribute is Class-Path, which lists additional JARs/classpaths required to run the code in that JAR.

Maven scopes
Compile, runtime, test, system, provided
https://www.baeldung.com/maven-dependency-scopes

addClasspath
You can use addClasspath like <addClasspath>true</addClasspath> to make dependencies even in "test" and "provided" to be included in the manifest. 

Mockito
Generate dummy data to be used in testing

Maven Wrappers
https://www.baeldung.com/maven-wrapper
You just run "mvnw".
Set-up: 
"mvn -N wrapper:wrapper".
Set-up with version:
"mvn -N wrapper:wrapper -Dmaven=3.5.2"
Check maven version used:
"./mvnw --version"

Dynamically loaded class
Not compiled into bytes codes but is used during runtime

~/.m2/repository
maven checks file size and checksum to see if the dependency downloaded is complete or not; Thus, we don't need to worry.

maven goals and lifecycles
Actions taken by maven such as "compile" and "test". If I run maven package, the following life cycle takes place:
(probably) validate -> compile -> test -> package into jar

maven groupId and artifactId vs import statement
"groupId" usually indicates the organization. It is used to indicate a repository on the maven website. artifactId is a particular project made by the organization. 
The import statement depends on the artifact, but not the groupId. 
Check package structure by:
"jar tvf /path/to/file.jar" or "jar xf /path/to/file.jar"
"t" proably refers to "tree command"
Error might occur if you try to import a class that only has "package-wide" scope.
"Cannot find symbol" error.

Maven compiler plugins
https://www.baeldung.com/maven-java-version
Java 1.8 <=> Java 8
Java 1.7 <=> Java 7

JUnit in Spring
Surefire plugin is used (?). Can run "mvn test" or just "mvn install", etc.
It helps maven find all XXXTest.java and @test in it to test.

edk2-stable202208


