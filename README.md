# Comandos Linux

v 0.7 10-12-24

## Índice
1. [Ver la versión de Linux](#1-ver-la-versión-de-linux)
2. [Ver Hardware](#2-ver-hardware)
3. [Borrar historial](#3-borrar-historial)
[... otros 29 items ...]

---

## 1. Ver la versión de Linux
```bash
# lsb_release -a
# cat /etc/issue
# uname --m  # Ver arquitectura (32/64 bits)
Ejemplo:

bash
root@server:~# lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 18.04.3 LTS
Release:        18.04
Codename:       bionic
2. Ver Hardware
Tarjeta gráfica
bash
# lspci | grep VGA
01:00.0 VGA compatible controller: NVIDIA Corporation GP104GL [Quadro P5000]
CPU
bash
# lscpu
Arquitectura:            x86_64
CPU(s):                  8
Modelo:                  158
Nombre del modelo:       Intel(R) Core(TM) i7-9700K CPU @ 3.60GHz
3. Borrar historial
Limpieza completa
bash
# echo "" > ~/.bash_history && history -c && exit
Eliminar comando específico
bash
# history -d 120  # Borra el comando número 120
# history -w      # Guarda cambios
4. Servicios de Ubuntu
Listar servicios
bash
# service --status-all
[ + ] apache2
[ - ] mysql
Añadir servicio al inicio
Crear script en /etc/init.d:

bash
sudo nano /etc/init.d/miservicio
Contenido básico:

bash
#!/bin/sh
/usr/bin/mi_programa
Permisos y registro:

bash
sudo chmod +x /etc/init.d/miservicio
sudo update-rc.d miservicio defaults
5. Samba
Instalación
bash
sudo apt install samba samba-common
Archivos clave
Ruta	Descripción
/etc/samba/smb.conf	Configuración principal
/var/log/samba/	Logs del servicio
6. Montar particiones
Montaje básico
bash
# mount /dev/sda1 /mnt/miparticion
Montar recurso de red
bash
# mount -t cifs //192.168.1.100/share /mnt/red -o username=user,password=pass
7. Comando DD
Hacer backup de disco
bash
# dd if=/dev/sda bs=4M | gzip > backup.img.gz
Restaurar
bash
# gunzip -c backup.img.gz | dd of=/dev/sda bs=4M
8. Permisos (chmod)
Valores comunes
Valor	Permisos
755	rwxr-xr-x
644	rw-r--r--
777	rwxrwxrwx (evitar)
bash
# chmod 755 script.sh
9. Crontab
Formato básico
* * * * * comando
│ │ │ │ │
│ │ │ │ └── día de semana (0-6)
│ │ │ └──── mes (1-12)
│ │ └────── día del mes (1-31)
│ └──────── hora (0-23)
└────────── minuto (0-59)
Ejemplo: Ejecutar cada día a las 3 AM
0 3 * * * /ruta/script.sh
10. Redes
Configuración IP estática
Editar /etc/netplan/01-netcfg.yaml:

yaml
network:
  version: 2
  ethernets:
    eth0:
      addresses: [192.168.1.100/24]
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
Aplicar cambios
bash
sudo netplan apply
11. Logs importantes
Ruta	Contenido
/var/log/syslog	Log general del sistema
/var/log/auth.log	Autenticaciones y SSH
/var/log/apache2/	Logs de Apache
12. Servidor HTTP rápido
bash
# Python 2
python -m SimpleHTTPServer 8000

# Python 3
python3 -m http.server 8000
Formatos comunes
Comprimir/Descomprimir
Formato	Comando
.tar.gz	tar -czvf archivo.tar.gz carpeta/
.zip	zip -r archivo.zip carpeta/
.rar	unrar x archivo.rar
Checksums
bash
# MD5
md5sum archivo.iso

# SHA256
sha256sum archivo.iso
