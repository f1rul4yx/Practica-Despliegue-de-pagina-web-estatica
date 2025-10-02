---
title: Instalacion GNS3 (Debian 13)
date: 2025-10-02 21:17:00 +0200
categories: [Instalacion, GNS3]
tags: [GNS3, Debian]     # TAG names should always be lowercase
---

## Instalación Wireshark
Para instalar Wireshark lo primero será ejecutar el siguiente comando:
```bash
sudo apt install wireshark -y
```
En el proceso de instalación aparecerá una opción:
Debemos pulsar en “Sí”, ya que nos está preguntando si queremos poder usar Wireshark con un usuario sin privilegios de sudo.
Para conseguir usar Wireshark sin privilegios sudo, también debemos dar permisos de ejecución para otros al archivo /usr/bin/dumpcap ya que este se encarga de la captura de paquetes y solo lo puede ejecutar el usuario root y los usuario del grupo wireshark, esto lo hacemos con el siguiente comando:
```bash
sudo chmod +x /usr/bin/dumpcap
```
## Instalación GNS3
Para empezar con la descarga de GNS3 lo primero será instalar los paquetes necesarios que este necesita para funcionar, para ello ejecutaré el siguiente comando:
```bash
sudo apt install python3 python3-pip pipx python3-pyqt5 python3-pyqt5.qtwebsockets python3-pyqt5.qtsvg qemu-kvm qemu-utils libvirt-clients libvirt-daemon-system virtinst ca-certificates curl gnupg2 -y
```
Ahora pasaré a instalar GNS3 pero como no se encuentra en los repositorios de Debian no se puede instalar con apt, para ello usaré pipx que es una herramienta de Python que sirve para instalar aplicaciones.
```bash
pipx install gns3-server==2.2.52 && pipx install gns3-gui==2.2.52 && pipx inject gns3-gui gns3-server PyQt5
```
Por defecto pipx no incluye en el PATH el directorio donde se encuentran los paquetes instalados a si que se usa el siguiente comando para ello y poder ejecutar GNS3 desde consola:
```bash
pipx ensurepath
```
A continuación instalaré dos dependencias que no se pueden instalar en Debian 13 desde apt o pipx, por lo que las instalaré desde el repositorio de GitHub de cada una.
El primer paquete es vpcs, que es el encargado de simular ordenadores virtuales.
```bash
git clone https://github.com/GNS3/vpcs.git
cd vpcs/src/
bash mk.sh
sudo cp vpcs /usr/local/bin/
```
El segundo es ubridge, que es el que permite crear conexiones entre los dispositivos virtuales.
```bash
git clone https://github.com/GNS3/ubridge.git
cd ubridge/
sudo apt install libpcap-dev -y
make
sudo make install
```
El tercer paquete es dynamips, que es el encargado de emular routers Cisco, permitiendo correr sus sistemas operativos en entornos virtuales.
```bash
git clone https://github.com/GNS3/dynamips.git
cd dynamips/
sudo apt install cmake gcc libelf-dev libpcap0.8-dev uuid-dev -y
mkdir build && cd build/
cmake ..
make
sudo make install
```
Si no se puede abrir la terminal del equipo por un error de telnet seguramente sea por no tener instalado telnet, que es lo que me pasó a mi, se instala con el siguiente comando:
```bash
sudo apt install telnet -y
```
## Instalación VMware
Ahora voy a instalar GNS3 VM que es una forma muy interesante de usar el simulador GNS3 ya que toda la carga de dispositivos virtualizados recae sobre la máquina virtual que contiene el programa previamente instalado y además puede ser usado por más de un equipo.
GNS3 VM es una máquina virtual previamente configurada por la empresa de GNS3 que proporciona el programa a los equipos de la red.
Lo primero será tener un programa con el cual poder ejecutar GNS3 VM. En mi caso usare VMware.
Después de descargar el archivo de instalación del programa desde la web oficial debemos darle permisos de ejecución.
```bash
chmod +x VMware-Workstation-Full-17.6.1-24319023.x86_64.bundle
```
A continuación descargamos los paquetes linux-headers de nuestra versión de kernel y los paquetes gcc y make.
```bash
sudo apt install linux-headers-$(uname -r) gcc make -y
```
Ahora ejecutamos el instalador como un script de bash.
```bash
sudo bash VMware-Workstation-Full-17.6.1-24319023.x86_64.bundle
```
Y por último ejecutaremos el programa. Este iniciará con el típico instalador al estilo Windows de siguiente.
