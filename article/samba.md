[参考资料](https://www.howtoforge.com/samba-server-installation-and-configuration-on-centos-7) 

#### 修改 hosts
notepad C:\Windows\System32\drivers\etc\hosts

192.168.0.100 	server1.example.com	centos


## Anonymous Samba sharing

#### centos 7安装

yum install samba samba-client samba-common

cp -pf /etc/samba/smb.conf /etc/samba/smb.conf.bak

cat /dev/null > /etc/samba/smb.conf

vi /etc/samba/smb.conf

```ini
[global]
workgroup = WORKGROUP
server string = Samba Server %v
netbios name = centos
security = user
map to guest = bad user
dns proxy = no
#============================ Share Definitions ============================== 
[Anonymous]
path = /samba/anonymous
browsable =yes
writable = yes
guest ok = yes
read only = no
```

mkdir -p /samba/anonymous
systemctl enable smb.service
systemctl enable nmb.service
systemctl restart smb.service
systemctl restart nmb.service

#### 早期的centos，防火墙会阻止samba

firewall-cmd --permanent --zone=public --add-service=samba
firewall-cmd --reload

win+r打开运行，输入：
\\centos

试着在windows上创建文件，你会发现提示无权限。

To allow access by the anonymous user, set the permissions as follows:

cd /samba
chmod -R 0755 anonymous/
chown -R nobody:nobody anonymous/

ls -l anonymous/

Further, we need to allow the SELinux for the samba configuration as follows:

chcon -t samba_share_t anonymous/


Now the anonymous user can browse & create the folder contents.

## samba服务安全

groupadd smbgrp
useradd srijan -G smbgrp
smbpasswd -a srijan

smbpasswd -a srijan

mkdir -p /samba/secured
cd /samba
chmod -R 0777 secured/
chcon -t samba_share_t secured/

vi /etc/samba/smb.conf

```ini
[...]
[secured]
 path = /samba/secured
 valid users = @smbgrp
 guest ok = no
 writable = yes
 browsable = yes
```


systemctl restart smb.service
systemctl restart nmb.service

testparm

cd /samba
chown -R srijan:smbgrp secured/

#### 在windows上访问

\\192.168.254.129\Anonymous