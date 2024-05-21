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

USB device name 
