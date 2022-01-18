# grub-password-set-ubuntu-20.04-centos
~~~

1)to hold grub menu for ubuntu 20.04 os for 5s:
~~~
Ubuntu 20.04 Reference
~~~
go to following file and change GRUB_TIMEOUT= 0 to 5 and add new line GRUB_TIMEOUT_STYLE=menu and save|exit and update-grub command

**********************************
vim /etc/default/grub.d/50-cloudimg-settings.cfg

# Cloud Image specific Grub settings for Generic Cloud Images
# CLOUD_IMG: This file was created/modified by the Cloud Image build process

# Set the recordfail timeout
GRUB_RECORDFAIL_TIMEOUT=0

# Do not wait on grub prompt
GRUB_TIMEOUT=5

# Set the default commandline
GRUB_CMDLINE_LINUX_DEFAULT="console=tty1 console=ttyS0"

# Set the grub console type
GRUB_TERMINAL=console
GRUB_TIMEOUT_STYLE=menu

save and exit 

update-grub
~~~

************************************



2) ubuntu grub2 password set 
https://help.ubuntu.com/community/Grub2/Passwords


for Boot login Password: 
~~~
vim /etc/grub.d/00_header     ; below the all files add below command

cat << EOF
set superusers="admin"
password admin 123456
EOF
~~~
save and exit and update-grub


*****************************************************************************************************************
You can protect with password any actions in grub, except booting existing menu entries without changing them.
******************************************************************************************************************
https://selivan.github.io/2017/12/21/grub2-password-for-all-but-default-menu-entries.html
~~~
root@grub-check-ubuntu:~# grub-mkpasswd-pbkdf2
Enter password:
Reenter password:
PBKDF2 hash of your password is grub.pbkdf2.sha512.10000.7E234AC333066E3FA683F66EE56031A880A6301C9CAA4BBD3669C6AF7165FD4E77FE21656AFE47CE4CAAA09E51E253B5E89C425E4D29CE4071FFBA4706D2BA20.8799ADDA943FF6100B481B0AC429500E29773C386BBB95E85F25D496FC7B57B52E30563FA3B6768A1929DEBE24E61E965A8B87E1F21198CCEBE3A3B7C319F00D
~~~
Copy this PBKDF2 hash file 

Then create a new file name:
~~~
vim /etc/grub.d/01_password

#!/bin/sh
set -e

cat << EOF
set superusers="grub"
# NOTE: no newline after 'password_pbkdf2 grub'
password_pbkdf2 grub grub.pbkdf2.sha512.10000.3D3AF9CADA7E87C4CC938C3127426AD71FA9C8D42311A923C739BD91B0EFFEE4488B71505C5C306282D94F1AA84801D231CAF53D2667621D3D2D6ACC728F2F40.51225B857D268B024BC0696D8B7D04BB94A2E0C26D495324780CD84B5FB55BA4EF7A1BFF452E76052DAC5FA9B8AD92A74FB38BD873845F223167B4687F35EC0A
EOF

save and exit.
chmod a+x /etc/grub.d/01_password

then go to /etc/grub.d/10_linux and find the fist CLASS="--class gnu-linux --class gnu --class os" and add --unrestricted. file looks like belows.

vim /etc/grub.d/10_linux

CLASS="--class gnu-linux --class gnu --class os --unrestricted"

save and exit and update-grub


3) centos7

grub2-setpassword

ref: https://www.tecmint.com/password-protect-single-user-mode-in-centos-7/

~~~
