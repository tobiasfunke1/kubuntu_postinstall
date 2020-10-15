# kubuntu_postinstall

```
# update
sudo apt update
sudo apt upgrade

# remove
sudo apt autoremove kdeconnect avahi-daemon avahi-autoipd

# install
sudo apt install ufw python3-pip gparted thunderbird thunderbird-locale-en thunderbird-locale-de
sudo apt install firefox firefox-locale-en firefox-locale-de enigmail
sudo apt install lshw usbutils pcituils net-tools efibootmgr efitools efivar
sudo apt install pass qtpass sshfs meld binwalk git htop

# dev install
snap install pycharm-professional --classic

# services
sudo systemctl disable bluetooth.service
sudo systemctl stop bluetooth.service
sudo systemctl disable cups.service
sudo systemctl stop cups.service
sudo systemctl disable cups-browsed.service
sudo systemctl stop cups-browsed.service

# swap [https://www.howtoforge.com/automatically-unlock-luks-encrypted-drives-with-a-keyfile]
sudo dd if=/dev/urandom of=/root/keyfile bs=1024 count=4
sudo chmod 0400 /root/keyfile
sudo cryptsetup -v luksFormat /dev/sdb /root/keyfile
sudo cryptsetup luksDump /dev/sdb
sudo cryptsetup luksOpen /dev/sdb sdX_crypt --key-file /root/keyfile
sudo mkswap /dev/mapper/sdX_crypt [-c]

sudo nano /etc/fstab
/dev/mapper/sdX_crypt none swap sw 0 0

lsblk -f
sudo nano /etc/crypttab
sdX_crypt UUID=XXXXXXXXXXX /root/keyfile luks


# firefox addons:
https everywhere
ghostery
ublock origin

# ssh [https://blog.g3rt.nl/upgrade-your-ssh-keys.html, https://medium.com/tarkalabs/ssh-simplified-using-ssh-config-161406ba75d7]
ssh-keygen -o -a 100 -t ed25519

~/.ssh/config
Host abc
   HostName abc.de
   User user
   Port 22
   IdentityFile ~/.ssh/id_abc
   IdentitiesOnly yes
   PreferredAuthentications publickey

# gpg
gpg --full-generate-key
   - 1 RSA und RSA
   - 4096
   - 1y
   - Name
   - eMail
   - Kommentar
   - passphrase

# qtpass settings
gpg key
30 character
include special symbols

# sddm design [https://github.com/sddm/sddm/issues/721]
change to black (#000000) in /usr/share/sddm/themes/breeze/theme.conf




# boot options
secure boot:
1. UEFI with MS signature: UEFI -> shim (MokManager) -> grub -> initramfs (luks) -> encrypted rootfs
   - factory keys and secure boot enabled in bios menu
   - shim is signed with MS signature an can load apps with other signatures (ex. grub)
2. UEFI with own signature: UEFI -> grub -> initramfs (luks) -> encrypted rootfs
3. ..

legacy boot:
1. ..
2. ..

TODO: secure boot possible in 1. and 2. ?
TODO: add evil maid
```
