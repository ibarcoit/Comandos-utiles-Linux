sudo Comandos Utiles Linux

Aquí existen unos pequeños comandos utilies de casos y comandos que mes se utilizan en linux

--------------------------------------------------

#1. Ver la versión de Linux
```bash
sudo lsb_release -a
```
```bash
sudo cat /etc/issue
```
```bash
sudo uname --m  (Ver si es 32 o 64 bits)
```
sudo cat /etc/SuSE-relise


Ejemplo:
```bash
root@ldap@li:~sudo lsb_release -a
```
Salida:
No LSB modules are available.
Distributor ID: Ubuntu
Description: Ubuntu 18.04.3 LTS
Release: 18.04
Codename: bionic

--------------------------------------------------

#2. Ver Hardware

Instalar PCUtils:
```bash
apt install pcutils
```
Ver tarjeta gráfica:
```bash
lspci | grep VGA
```
Ver CPU:
```bash
sudo lscpu
```
--------------------------------------------------

#3. Borrar historial

Eliminar totalmente el historial:
```bash
sudo echo "" > ~/.bash_history && history -c
```
```bash
sudo echo "" > ~/.bash_history && history -c && exit
```
Eliminar un comando específico:
```bash
sudo history -d 120
```
```bash
sudo history -w
```
--------------------------------------------------

#4. Servicios de Ubuntu

Ver todos los servicios:
sudo service --status-all

Reiniciar servicio DHCP:
sudo service isc-dhcp-server restart

Añadir servicio en el inicio del sistema:

#1. Crear script en /etc/init.d:
sudo cd /etc/init.d
sudo sudo vi noip2

#2. Añadir contenido:
#!/bin/sh sudo /usr/local/bin/noip2

#3. Dar permisos:
sudo chmod +x noip2

#4. Añadir a inicio:
sudo update-rc.d noip2 defaults

--------------------------------------------------

#5. Samba

Instalar:
sudo sudo apt-get install samba samba-common smbclient samba-doc smbfs

Archivos importantes:
- Configuración: /etc/samba/smb.conf
- Logs: /var/log/samba
- Carpetas: /media/NAS_Stack/

--------------------------------------------------

#6. Montar partición

Ver volúmenes montados:
sudo mount

Local:

Montar partición LVM:
sudo mount -t ext4 /dev/mapper/ubuntu--vg-ubuntu--lv /mnt/montado

Montar partición normal:
sudo mount /dev/sda1 /mnt/montado

Montar partición NTFS bloqueada por Windows:
sudo mount -t ntfs-3g -o remove_hiberfile /dev/sda2 /media/Juan

Red:

Montar recurso compartido temporal:
sudo mount -t cifs //192.168.20.4/Backups /mnt/qnapbackups -o username=lammanagen,password=SuperS552$,rw,vers=3.0,iocharset=utf8,uid=WORKGROUP,gid=WORKGROUP

Montar permanente (editar /etc/fstab):
//192.168.10.4/Backups/Export /mnt/backups cifs uid=user,rw,vers=3.0,credentials=/home/user/.samba

Crear archivo de credenciales (/home/user/.samba):
username=lanuser
password=39L0qw672E1kVB21Eate$
workgroup=WORKGROUP

Montar:
sudo mount -a

--------------------------------------------------

#7. DD

Partición a GZIP (Backup):
sudo mkdir /mnt/sdb1
sudo mount -t vfat /dev/sdb1 /mnt/sdb1
sudo dd if=/dev/hda conv=sync,noerror bs=64K | gzip -c > /mnt/sdb1/hda.img.gz

Restaurar:
sudo gunzip -c /mnt/sdb1/hda.img.gz | dd of=/dev/hda conv=sync,noerror bs=64K

Partición a ISO:
sudo dd if=/dev/sdb1 of=/mnt/Respaldo/ImgPart.ISO conv=sync,noerror bs=1M

Restaurar:
sudo dd if=/mnt/Respaldo/ImgPart.ISO of=/dev/sdb1 conv=sync,noerror bs=1M

Clonar disco entero:
sudo dd if=/dev/sda of=/dev/sdb bs=4096 conv=noerror,sync

Ver progreso (en otra consola):
sudo watch -n 10 kill -USR1 `pidof dd`

--------------------------------------------------

#8. Chown

Cambiar propietario:
sudo chown nuevo_propietario archivo

--------------------------------------------------

#9. Cambio recursivo de propietario de carpeta

Cambiar propietario y grupo:
sudo chown -R usuario:usuario /var/home

--------------------------------------------------

#10. chmod

Permisos en formato numérico octal:

Permisos | Valor | Descripción
---------|-------|------------
rw------- | 600 | Propietario lectura y escritura
rwx--x--x | 711 | Propietario todos, grupo y otros solo ejecución
rwxr-xr-x | 755 | Propietario todos, grupo y otros lectura y ejecución
rwxrwxrwx | 777 | Todos los permisos para todos
r-------- | 400 | Solo lectura para propietario
rw-r----- | 640 | Propietario lectura/escritura, grupo solo lectura

--------------------------------------------------

#11. crontab

Editar crontab:
sudo crontab -e

Estructura:
* * * * * command_to_execute

Ejemplos:
- Diario a las 7 PM: 00 19 * * * usuario /ubicacion/script/consulta.sh
- Domingos a las 7 PM: 00 19 * * 0 usuario /ubicacion/script/consulta.sh
- Anual el 4 de febrero: 00 19 4 2 * usuario /ubicacion/script/consulta.sh

Listar tareas:
sudo crontab -l

Borrar crontab:
sudo crontab -d

--------------------------------------------------

#12. Script básico

Ejemplo (limpieza.sh):
#!/bin/bash
cd /var/lib/motion/
rm *.mkv
verify=$?
if [ $verify = 0 ]; then
    logger "Carpeta /var/lib/motion/ limpiada"
else
    logger "Carpeta /var/lib/motion/ estaba limpia"
fi

Hacer ejecutable:
sudo chmod +x limpieza.sh

--------------------------------------------------

#13. Scripting en Bash

Documentación: https://bioinf.comav.upv.es/courses/unix/scripts_bash.html

--------------------------------------------------

#14. Carpetas

Crear recursivo:
sudo mkdir -p /srv/www/gitlab/public_html/

Copiar:
sudo cp -r directorio/ ruta_de_destino/nombre_copia

Renombrar:
sudo mv img images

Eliminar:
sudo rm -r cyc/

ls - ordenar por fecha:

Descendente:
sudo ls -lt

Ascendente:
sudo ls -ltr

Por tamaño:
sudo ls -l --sort-size

du - tamaño de las carpetas:
sudo du -smh *

Enlaces simbólicos:
sudo ln -s [target file] [Symbolic filename]

Cut:
sudo cut -d':' -f1 /etc/passwd

--------------------------------------------------

#15. Mensajes de Inicio
sudo dmesg | grep lo_que_se_quiera_ver

--------------------------------------------------

#16. grep

Buscar texto recursivamente:
sudo grep -R "192.168.10" *

--------------------------------------------------

#17. tar.gz

Comprimir:
$ tar -czvf empaquetado.tar.gz /carpeta/a/empaquetar/

Descomprimir:
$ tar -xzvf archivo.tar.gz

--------------------------------------------------

#18. Cifrados

Base64:
Cifrar:
$ echo matrix_1 | base64

Descifrar:
$ echo bWF0cml4XzEK | base64 -d

MD5:
$ echo -n matrix_1 | md5sum

--------------------------------------------------

#19. Users

Script para crear usuario:
sudo adduser usuario

Cambiar contraseña:
sudo sudo passwd usuario

Sudoer:
Editar sudoers:
sudo visudo

Añadir línea:
operator ALL=(ALL:ALL) ALL

Cambio contraseña Perdida:

Sin Live CD/DVD:
Guía: https://www.maketecheasier.com/reset-root-password-linux/

Con Live CD/DVD:
#1. Iniciar con systemrescued
#2. Montar partición:
sudo lvm vgscan -v
sudo lvm vgchange -ay
sudo lvm lvs -all
sudo mount /dev/ubuntu-vg/ubuntu-lv /mnt

#3. Editar shadow:
sudo nano /mnt/etc/shadow

Eliminar usuario:
sudo userdel -r usuario
sudo deluser --remove-home usuario

--------------------------------------------------

#20. Logs

Ver logs del sistema:
sudo cat /var/log/syslog

Inicios de sesión:
sudo cat /var/log/auth.log

--------------------------------------------------

#21. top y htop

Ordenar por uso de memoria en top:
Shift + M

O interactivamente:
Shift + F → seleccionar %MEM → Enter

--------------------------------------------------

#22. Tiempo - Zona horaria

Ver fecha:
sudo date

Establecer fecha manual:
sudo date -s "27 FEB 2017 21:26:00"

Sincronizar con NTP:
sudo dpkg-reconfigure tzdata

--------------------------------------------------

#23. Error apt-get update

Añadir clave pública:
sudo gpg --keyserver keys.gnupg.net --recv-key 887A41B4AF6056F5
sudo gpg --export --armor 887A41B4AF6056F5 | apt-key add -

--------------------------------------------------

#24. Ver mensajes de inicio de sistema
sudo cat /var/log/dmesg | grep -i eth0
sudo dmesg | grep -i eth0

--------------------------------------------------

#25. Hostname Ubuntu

Temporal:
sudo hostnamectl set-hostname nuevo_nombre

Permanente:
#1. Editar /etc/hostname
#2. Editar /etc/cloud/cloud.cfg (cambiar preserve_hostname: false a true)

--------------------------------------------------

#26. Red

Ver ens de la tarjeta:
sudo ip a

Netplan:

Configuración DHCP (/etc/netplan/50-cloud-init.yaml):
network:
  ethernets:
    ens160:
      dhcp4: true
  version: 2

Configuración estática:
network:
  ethernets:
    ens160:
      addresses:
        - 192.168.10.120/24
      gateway4: 192.168.10.1
      nameservers:
        addresses:
          - 192.168.10.1
          - 8.8.8.8
  version: 2

Ver IP:
sudo ip a

Renovar IP:
sudo dhclient -v

Liberar IP:
sudo dhclient -r -v

Traceroute:
sudo traceroute linux.org

tcpdump:

Instalar:
sudo apt install tcpdump -y

Ejemplos:
sudo tcpdump -i eth1
sudo tcpdump -i eth1 -c 5 port 80
sudo tcpdump host 10.0.2.15
sudo tcpdump -vv host 192.168.10.196
sudo tcpdump -i eth0 tcp and port 22 and src 192.168.10.196
sudo tcpdump -i eth0 tcp and not port 22 and src 192.168.10.196
sudo tcpdump -i eth1 -c 10 -w icmp.pcap
sudo tcpdump -r icmp.pcap

netstat:
sudo netstat -tulpn | grep :80
sudo netstat -putona | grep numero-de-puerto
sudo lsof -i:9200  (Para Ubuntu)

--------------------------------------------------

#27. Discos

Volúmenes LVM y montado permanente:
Ver documentación en D:\Manuales\Volumenes LVM

--------------------------------------------------

#28. Expandir partición

#1. Verificar nuevo tamaño:
sudo fdisk -l

#2. Aplicar cambios en MBR:
sudo fdisk /dev/sda
Command (m for help): w

#3. Usar cfdisk para expandir:
sudo cfdisk

#4. Expandir a nivel de sistema:
sudo resize2fs /dev/sda2

#5. Verificar:
sudo df -lh

--------------------------------------------------

#29. Inicios de Sesión - Logs

Ver autenticaciones:
sudo cat /var/log/auth.log | grep "Accepted password"

--------------------------------------------------

#30. FTP

Servidor:
sudo twistd -n ftp -p 21 --r /home/overmind --password-file=FTPusers.txt

Archivo FTPusers.txt:
overmind:0v3rmind
Vanko:matrix123

Verificar:
sudo netstat -ptan

Cliente:
$ ftp
ftp> open 192.168.10.125
ftp> put archivo
ftp> get archivo
ftp> quit

--------------------------------------------------

#31. Sesiones activas de SSH
sudo service ssh status

--------------------------------------------------

#32. HTTP Server rápido
$ python -m SimpleHTTPServer 7443

Para Python 3:
$ python3 -m http.server 7443



