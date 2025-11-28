# Projeto Final – Capítulos 6, 7 e 9
## RELATÓRIO DE PRÁTICAS E CÓDIGOS

**Nome:** Vinicius Souza Rabaquim 
**Turno:** Noite 
**Data do último commit:** (preencher ao final)

---

# ATIVIDADE 1 – RELATÓRIO DAS PRÁTICAS

Esta atividade consiste em documentar cada prática realizada nos capítulos 6, 7 e 9 do livro.  
Para cada prática, deve-se apresentar um resumo e as saídas reais do terminal.

---

# CAPÍTULO 6 – PRÁTICAS (p. 171–172)

## Prática 01 – Configuração de Disco e Montagem (p. 171)

### Resumo da Prática
Adicionei um novo disco `sdb` à máquina virtual.
Criei a partição `sdb1` com o utilitário `fdisk`. 
Formatei a partição em ext4 usando `mkfs.ext4`. 
Criei o diretório `/backup` e configurei a montagem automática no arquivo `/etc/fstab`.  
Por fim, montei a partição e validei com `df -h`.

* **Evidência de Validação:**
```bash
# cat /etc/fstab
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# systemd generates mount units based on this file, see systemd.mount(5).
# Please run 'systemctl daemon-reload' after making changes here.
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
# / was on /dev/sda1 during installation
UUID=76e3f145-9349-4bad-84b3-1e994e43eb04 /               ext4    errors=remount-ro 0       1
# swap was on /dev/sda5 during installation
UUID=0dbe92fa-3aaf-4da6-9077-eb27f71c804a none            swap    sw              0       0
/dev/sr0        /media/cdrom0   udf,iso9660 user,noauto     0       0
UUID=cb0343ed-0a8f-4c7d-ac21-3e00d8aef435 /backup ext4 defaults 0 0

# df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            2.3G     0  2.3G   0% /dev
tmpfs           460M  1.3M  458M   1% /run
/dev/sda1        47G   11G   34G  24% /
tmpfs           2.3G     0  2.3G   0% /dev/shm
tmpfs           5.0M  8.0K  5.0M   1% /run/lock
/dev/sdb1       7.9G   24K  7.5G   1% /backup
tmpfs           460M  236K  459M   1% /run/user/1000

## Prática 02 – Montagem de Mídia (p. 172)
###Resumo da Prática
Criei o diretório cdrom na pasta do usuário, montei a mídia /dev/sr0 no diretório e validei seu conteúdo lendo um arquivo presente na ISO montada.
Criei o diretório cdrom na pasta do usuário,montei a mídia /dev/sr0 no diretório
 e validei seu conteúdo lendo um arquivo presente na ISO montada.

* **Evidência de Validação:**
#df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            2.3G     0  2.3G   0% /dev
tmpfs           460M  1.3M  458M   1% /run
/dev/sda1        47G   11G   34G  24% /
tmpfs           2.3G     0  2.3G   0% /dev/shm
tmpfs           5.0M  8.0K  5.0M   1% /run/lock
/dev/sdb1       7.9G   24K  7.5G   1% /backup
tmpfs           460M  236K  459M   1% /run/user/1000
/dev/sr0        628M  628M     0 100% /media/cdrom0

# cat ~/cdrom/README.mirrors.txt
The list of Debian mirror sites is available here: https://www.debian.org/mirror/list

### CAPÍTULO 7 – PRÁTICAS (p. 233)
### Prática prc0001 01 – Registro de Sessão e Processos (p. 233)
#### Resumo da Prática
Executei o comando locale-gen.
Iniciei a gravação de sessão com o comando script.
Listei os processos em execução utilizando o comando ps.
Filtrei processos relacionados ao Python usando grep.
Finalizei a sessão e visualizei o arquivo gerado contendo o registro.

* **Evidência de Validação:**
# cat /home/usuario/typescript
Script started on 2025-11-26 22:02:56-03:00 [TERM="xterm-256color" TTY="/dev/pts/0" COLUMNS="87" LINES="24"]
userlinux@debian:~$ ps aux | grep python
userlin+    2779  0.1  1.0 372952 51444 ?        Sl   21:57   0:00 /usr/bin/python3 /usr/bin/blueman-applet
userlin+    5090  0.0  0.0   6340  2044 pts/1    S+   22:03   0:00 grep python
userlinux@debian:~$ exit
exit

Script done on 2025-11-26 22:03:07-03:00 [COMMAND_EXIT_CODE="0"]


### CAPÍTULO 9 – PRÁTICAS (p. 286–288)
### Prática checkpoint03 – Configurar Endereço IP Estático (p. 286)
#### Resumo da Prática
Editei o arquivo /etc/network/interfaces para configurar um endereço IP estático.
Reiniciei o sistema e validei a configuração usando os comandos ip address e ip route.

* **Evidência de Validação:**
# ip address show enp0s3
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:ea:a1:08 brd ff:ff:ff:ff:ff:ff
    inet 192.168.0.50/24 brd 192.168.0.255 scope global enp0s3
       valid_lft forever preferred_lft forever
    inet6 fd00::a00:27ff:feea:a108/64 scope global dynamic mngtmpaddr 
       valid_lft 86305sec preferred_lft 14305sec
    inet6 fe80::a00:27ff:feea:a108/64 scope link 
       valid_lft forever preferred_lft forever

# ip route
default via 192.168.0.1 dev enp0s3 onlink 
192.168.0.0/24 dev enp0s3 proto kernel scope link src 192.168.0.50 

# cat /etc/network/interfaces
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

allow-hotplug enp0s3
iface enp0s3 inet static
address 192.168.0.50
netmask 255.255.255.0
gateway 192.168.0.1
dns-nameservers 8.8.8.8 1.1.1.1


## Prática checkpoint04 – DHCP e Configuração Manual (p. 287)
###Resumo da Prática
Configurei a interface enp0s3 para utilizar DHCP modificando o arquivo /etc/network/interfaces.
Após isso, configurei manualmente um endereço IP com o comando ip address add e o gateway com ip route add.

```bash
# Saída do comando 'ip address show enp0s3'
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:ea:a1:08 brd ff:ff:ff:ff:ff:ff
    inet 192.168.0.50/24 brd 192.168.0.255 scope global enp0s3
       valid_lft forever preferred_lft forever
    inet6 fd00::a00:27ff:feea:a108/64 scope global dynamic mngtmpaddr 
       valid_lft 85956sec preferred_lft 13956sec
    inet6 fe80::a00:27ff:feea:a108/64 scope link 
       valid_lft forever preferred_lft forever

# Saída do comando 'ip route'
default via 192.168.0.1 dev enp0s3 
192.168.0.0/24 dev enp0s3 proto kernel scope link src 192.168.0.50 

# Saída do comando 'cat /etc/network/interfaces'
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

allow-hotplug enp0s3
iface enp0s3 inet static
address 192.168.0.50
netmask 255.255.255.0
gateway 192.168.0.1
dns-nameservers 8.8.8.8 1.1.1.1

### Prática checkpoint05 – Teste com wget (p. 288)
#### Resumo da Prática
Não consegui realizar o download, pois dava esse erro : Resolving www.aied.com.br (www.aied.com.br)... failed: Temporary failure in name resolution.
Então eu acessei o site e criei um install.py e coloquei o que estava no site dentro.

* **Evidência de Validação:**
```bash
# Saída do comando 'cat /tmp/install.py'
#!/usr/bin/python3
import os;
import sys
import platform
machine2bits = {'AMD64': 64, 'x86_64': 64, 'i386': 32, 'x86': 32, 'i686' : 32}
os_version = machine2bits.get(platform.machine(), None)

os.system("apt update");
os.system("wget -O /tmp/libjsoncpp1_1.7.4-3_amd64.deb http://ftp.br.debian.org/debian/pool/main/libj/libjsoncpp/libjsoncpp1_1.7.4-3_amd64.deb");
os.system("dpkg -i /tmp/libjsoncpp1_1.7.4-3_amd64.deb");
#os.system("apt install libjsoncpp-dev -y");
os.system("apt install g++ -y");
os.system("apt install libcurl4-openssl-dev -y");
os.system("rm -r /etc/aied");
os.system("rm -r /etc/aied");
os.system("mkdir /etc/aied");
os.system("wget -O /tmp/aied.tar.gz http://www.aied.com.br/linux/download/aied_"+ str(os_version) +".tar.gz" );
os.system("tar -xzvf /tmp/aied.tar.gz -C /etc/aied/");
#os.system("rm /usr/sbin/aied");
#os.system("rm /usr/bin/aied.py");
os.system("ln -s /etc/aied/aied_"+ str(os_version) +" /usr/bin/aied");
os.system("chmod +x /etc/aied/aied_"+ str ( os_version ) + "   " );

#OK, serÃ¡ usado para isntalacao do aied.com.br

---
## ATIVIDADE 2: Mapa Mental (Conceitos Chave)
* **Instrução:** Esta atividade é **física** e **manual**.
* **Tarefa:** Crie um Mapa Mental em uma **única folha A4**, feito à mão, conectando os
conceitos centrais dos Capítulos 6, 7 e 9. Não pode ser feito a lápis.
* **Entrega:** Entregar a folha A4 fisicamente ao professor antes da prova, até o dia
**28/11/2025**.
---
