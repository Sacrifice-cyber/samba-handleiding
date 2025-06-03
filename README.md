# samba-handleiding
Handleiding voor Samba-server
Samba Handleiding

1. Samba installeren
- Controleer of Samba beschikbaar is:
 sudo zypper search samba

- Controleer of de services draaien:
 systemctl status smb.service
 systemctl status nmb.service

- Activeer en start de services:
 sudo systemctl enable smb.service
 sudo systemctl enable nmb.service
 sudo systemctl start smb.service
 sudo systemctl start nmb.service

2. Firewalld uitschakelen (optioneel)
- Controleer status:
 systemctl status firewalld.service

- Stop en schakel uit:
 sudo systemctl stop firewalld.service
 sudo systemctl disable firewalld.service

3. Groepen aanmaken
sudo groupadd directie
sudo groupadd adviseurs
sudo groupadd secretaressen

4. Gebruikers aanmaken
sudo useradd -m m.beenham
sudo useradd -m a.veenman
# Herhaal dit voor alle gebruikers

5. Gebruikers aan groepen toevoegen
sudo usermod -g directie m.beenham
sudo usermod -aG directie m.beenham

6. Wachtwoord instellen + Samba wachtwoord
sudo passwd a.veenman
sudo smbpasswd -a m.beenham

7. Mappen aanmaken
cd /home
mkdir directie adviseurs secretaressen

8. Groep koppelen + rechten geven
sudo chgrp directie directie/
sudo chmod g+w directie/
sudo chgrp adviseurs adviseurs/
sudo chmod g+w adviseurs/
sudo chgrp secretaressen secretaressen/
sudo chmod g+w secretaressen/

9. ACL rechten instellen
sudo setfacl -m g:directie:r-x secretaressen/
sudo setfacl -m g:directie:rx adviseurs/

10. Samba configureren
sudo vi /etc/samba/smb.conf

[directie]
comment = share voor directie
path = /home/directie
writable = yes
browseable = yes
valid users = @directie
write list = @directie
create mask = 0770
directory mask = 0775

[adviseurs]
comment = share voor financieel adviseurs
path = /home/adviseurs
writable = yes
browseable = yes
valid users = @adviseurs @directie
write list = @adviseurs
read list = @directie
create mask = 0664
directory mask = 0775

[secretaressen]
comment = share voor secretaressen
path = /home/secretaressen
writable = yes
browseable = yes
valid users = @secretaressen @directie @adviseurs
write list = @secretaressen
read list = @adviseurs @directie
create mask = 0664
directory mask = 0775

11. Samba herstarten
sudo systemctl restart smb.service

12. Windows toegang
Open in Verkenner: \\<ip-adres>\directie

Voorbeeld: \\192.168.1.100\directie
Log in met gebruikersnaam en wachtwoord (smbpasswd)


Rsync -a /home/* /tmp/back/
Cd /tmp
Mkdir back
Crontab -e
*/1  **** /root/backup.sh
Chmod +x backup1.sh
Ls /tmp/back
