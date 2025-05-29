Comandos Linux

v 0.7 10-12-24

CONTENIDO

1. Ver la versión de Linux
2. Ver Hardware
3. Borrar historial
4. Servicios de Ubuntu
5. Samba
6. Montar partición
7. DD
8. Chown
9. Cambio recursivo de propietario de carpeta
10. chmod
11. crontab
12. Script básico
13. Scripting en Bash
14. Carpetas
15. Mensajes de Inicio
16. grep
17. tar.gz
18. Cifrados
19. Users
20. Logs
21. top y htop
22. Tiempo - Zona horaria
23. Error apt-get update
24. Ver mensajes de inicio de sistema
25. Hostname Ubuntu
26. Red
27. Discos
28. Expandir partición
29. Inicios de Sesión - Logs
30. FTP
31. Sesiones activas de SSH
32. HTTP Server rápido

--------------------------------------------------

1. Ver la versión de Linux

# lsb_release -a
# cat /etc/issue
# uname --m  (Ver si es 32 o 64 bits)
# cat /etc/SuSE-relise

Ejemplo:
root@ldap@li:~# lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description: Ubuntu 18.04.3 LTS
Release: 18.04
Codename: bionic

--------------------------------------------------

2. Ver Hardware

Instalar PCUtils:
# apt install pcutils

Ver tarjeta gráfica:
# lspci | grep VGA

Ver CPU:
# lscpu

--------------------------------------------------

3. Borrar historial

Eliminar totalmente el historial:
# echo "" > ~/.bash_history && history -c
# echo "" > ~/.bash_history && history -c && exit

Eliminar un comando específico:
# history -d 120
# history -w

--------------------------------------------------

4. Servicios de Ubuntu

Ver todos los servicios:
# service --status-all

Reiniciar servicio DHCP:
# service isc-dhcp-server restart

Añadir servicio en el inicio del sistema:

1. Crear script en /etc/init.d:
# cd /etc/init.d
# sudo vi noip2

2. Añadir contenido:
#!/bin/sh sudo /usr/local/bin/noip2

3. Dar permisos:
# chmod +x noip2

4. Añadir a inicio:
# update-rc.d noip2 defaults

--------------------------------------------------

5. Samba

Instalar:
# sudo apt-get install samba samba-common smbclient samba-doc smbfs

Archivos importantes:
- Configuración: /etc/samba/smb.conf
- Logs: /var/log/samba
- Carpetas: /media/NAS_Stack/

--------------------------------------------------

6. Montar partición

Ver volúmenes montados:
# mount

Local:

Montar partición LVM:
# mount -t ext4 /dev/mapper/ubuntu--vg-ubuntu--lv /mnt/montado

Montar partición normal:
# mount /dev/sda1 /mnt/montado

Montar partición NTFS bloqueada por Windows:
# mount -t ntfs-3g -o remove_hiberfile /dev/sda2 /media/Juan

Red:

Montar recurso compartido temporal:
# mount -t cifs //192.168.20.4/Backups /mnt/qnapbackups -o username=lammanagen,password=SuperS552$,rw,vers=3.0,iocharset=utf8,uid=WORKGROUP,gid=WORKGROUP

Montar permanente (editar /etc/fstab):
//192.168.10.4/Backups/Export /mnt/backups cifs uid=user,rw,vers=3.0,credentials=/home/user/.samba

Crear archivo de credenciales (/home/user/.samba):
username=lanuser
password=39L0qw672E1kVB21Eate$
workgroup=WORKGROUP

Montar:
# mount -a

--------------------------------------------------

7. DD

Partición a GZIP (Backup):
# mkdir /mnt/sdb1
# mount -t vfat /dev/sdb1 /mnt/sdb1
# dd if=/dev/hda conv=sync,noerror bs=64K | gzip -c > /mnt/sdb1/hda.img.gz

Restaurar:
# gunzip -c /mnt/sdb1/hda.img.gz | dd of=/dev/hda conv=sync,noerror bs=64K

Partición a ISO:
# dd if=/dev/sdb1 of=/mnt/Respaldo/ImgPart.ISO conv=sync,noerror bs=1M

Restaurar:
# dd if=/mnt/Respaldo/ImgPart.ISO of=/dev/sdb1 conv=sync,noerror bs=1M

Clonar disco entero:
# dd if=/dev/sda of=/dev/sdb bs=4096 conv=noerror,sync

Ver progreso (en otra consola):
# watch -n 10 kill -USR1 `pidof dd`

--------------------------------------------------

8. Chown

Cambiar propietario:
# chown nuevo_propietario archivo

--------------------------------------------------

9. Cambio recursivo de propietario de carpeta

Cambiar propietario y grupo:
# chown -R usuario:usuario /var/home

--------------------------------------------------

10. chmod

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

11. crontab

Editar crontab:
# crontab -e

Estructura:
* * * * * command_to_execute

Ejemplos:
- Diario a las 7 PM: 00 19 * * * usuario /ubicacion/script/consulta.sh
- Domingos a las 7 PM: 00 19 * * 0 usuario /ubicacion/script/consulta.sh
- Anual el 4 de febrero: 00 19 4 2 * usuario /ubicacion/script/consulta.sh

Listar tareas:
# crontab -l

Borrar crontab:
# crontab -d

--------------------------------------------------

12. Script básico

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
# chmod +x limpieza.sh

--------------------------------------------------

13. Scripting en Bash

Documentación: https://bioinf.comav.upv.es/courses/unix/scripts_bash.html

--------------------------------------------------

14. Carpetas

Crear recursivo:
# mkdir -p /srv/www/gitlab/public_html/

Copiar:
# cp -r directorio/ ruta_de_destino/nombre_copia

Renombrar:
# mv img images

Eliminar:
# rm -r cyc/

ls - ordenar por fecha:

Descendente:
# ls -lt

Ascendente:
# ls -ltr

Por tamaño:
# ls -l --sort-size

du - tamaño de las carpetas:
# du -smh *

Enlaces simbólicos:
# ln -s [target file] [Symbolic filename]

Cut:
# cut -d':' -f1 /etc/passwd

--------------------------------------------------

15. Mensajes de Inicio
# dmesg | grep lo_que_se_quiera_ver

--------------------------------------------------

16. grep

Buscar texto recursivamente:
# grep -R "192.168.10" *

--------------------------------------------------

17. tar.gz

Comprimir:
$ tar -czvf empaquetado.tar.gz /carpeta/a/empaquetar/

Descomprimir:
$ tar -xzvf archivo.tar.gz

--------------------------------------------------

18. Cifrados

Base64:
Cifrar:
$ echo matrix_1 | base64

Descifrar:
$ echo bWF0cml4XzEK | base64 -d

MD5:
$ echo -n matrix_1 | md5sum

--------------------------------------------------

19. Users

Script para crear usuario:
# adduser usuario

Cambiar contraseña:
# sudo passwd usuario

Sudoer:
Editar sudoers:
# visudo

Añadir línea:
operator ALL=(ALL:ALL) ALL

Cambio contraseña Perdida:

Sin Live CD/DVD:
Guía: https://www.maketecheasier.com/reset-root-password-linux/

Con Live CD/DVD:
1. Iniciar con systemrescued
2. Montar partición:
# lvm vgscan -v
# lvm vgchange -ay
# lvm lvs -all
# mount /dev/ubuntu-vg/ubuntu-lv /mnt

3. Editar shadow:
# nano /mnt/etc/shadow

Eliminar usuario:
# userdel -r usuario
# deluser --remove-home usuario

--------------------------------------------------

20. Logs

Ver logs del sistema:
# cat /var/log/syslog

Inicios de sesión:
# cat /var/log/auth.log

--------------------------------------------------

21. top y htop

Ordenar por uso de memoria en top:
Shift + M

O interactivamente:
Shift + F → seleccionar %MEM → Enter

--------------------------------------------------

22. Tiempo - Zona horaria

Ver fecha:
# date

Establecer fecha manual:
# date -s "27 FEB 2017 21:26:00"

Sincronizar con NTP:
# dpkg-reconfigure tzdata

--------------------------------------------------

23. Error apt-get update

Añadir clave pública:
# gpg --keyserver keys.gnupg.net --recv-key 887A41B4AF6056F5
# gpg --export --armor 887A41B4AF6056F5 | apt-key add -

--------------------------------------------------

24. Ver mensajes de inicio de sistema
# cat /var/log/dmesg | grep -i eth0
# dmesg | grep -i eth0

--------------------------------------------------

25. Hostname Ubuntu

Temporal:
# hostnamectl set-hostname nuevo_nombre

Permanente:
1. Editar /etc/hostname
2. Editar /etc/cloud/cloud.cfg (cambiar preserve_hostname: false a true)

--------------------------------------------------

26. Red

Ver ens de la tarjeta:
# ip a

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
# ip a

Renovar IP:
# dhclient -v

Liberar IP:
# dhclient -r -v

Traceroute:
# traceroute linux.org

tcpdump:

Instalar:
# apt install tcpdump -y

Ejemplos:
# tcpdump -i eth1
# tcpdump -i eth1 -c 5 port 80
# tcpdump host 10.0.2.15
# tcpdump -vv host 192.168.10.196
# tcpdump -i eth0 tcp and port 22 and src 192.168.10.196
# tcpdump -i eth0 tcp and not port 22 and src 192.168.10.196
# tcpdump -i eth1 -c 10 -w icmp.pcap
# tcpdump -r icmp.pcap

netstat:
# netstat -tulpn | grep :80
# netstat -putona | grep numero-de-puerto
# lsof -i:9200  (Para Ubuntu)

--------------------------------------------------

27. Discos

Volúmenes LVM y montado permanente:
Ver documentación en D:\Manuales\Volumenes LVM

--------------------------------------------------

28. Expandir partición

1. Verificar nuevo tamaño:
# fdisk -l

2. Aplicar cambios en MBR:
# fdisk /dev/sda
Command (m for help): w

3. Usar cfdisk para expandir:
# cfdisk

4. Expandir a nivel de sistema:
# resize2fs /dev/sda2

5. Verificar:
# df -lh

--------------------------------------------------

29. Inicios de Sesión - Logs

Ver autenticaciones:
# cat /var/log/auth.log | grep "Accepted password"

--------------------------------------------------

30. FTP

Servidor:
# twistd -n ftp -p 21 --r /home/overmind --password-file=FTPusers.txt

Archivo FTPusers.txt:
overmind:0v3rmind
Vanko:matrix123

Verificar:
# netstat -ptan

Cliente:
$ ftp
ftp> open 192.168.10.125
ftp> put archivo
ftp> get archivo
ftp> quit

--------------------------------------------------

31. Sesiones activas de SSH
# service ssh status

--------------------------------------------------

32. HTTP Server rápido
$ python -m SimpleHTTPServer 7443

Para Python 3:
$ python3 -m http.server 7443
