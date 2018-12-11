# UF1. Exercici 2.1. Crear màquines virtuals

## Introducció

Les màquines virtuals simularan un maquinari que ens permetrà instal.lar-hi sistemes operatius convidats (guest).

## Continguts

Farem ús del virt-manager per tal de crear i gestionar maquinari de màquines virtuals amb l'hipervisor de codi obert KVM/qemu:

Requisits:
- CentOs/Fedora: dnf install @Virtualization -y
- Debian/Ubuntu: apt install qemu-kvm libvirt-clients qemu-utils libvirt-daemon-system virt-viewer virt-manager -y

Comproveu que el servei libvirtd està funcionant: systemctl status libvirtd i si no l'engegueu.

## Entrega

1. **Crea una màquina amb el virt-manager que ens permeti instal.lar-hi el Fedora 27 a partir del servidor de xarxa PXE. Inícia-la i instala-hi el sistema operatiu Fedora 27. Fes servir un disc dur virtual qcow2 de 10GB. Què has seleccionat a cada apartat?**
- 1/4 Com volem instal.lar el sistema operatiu?

			Mitjançant una iso

- 2/4 Quin emmagatzematge has fet servir? Detalla'n el procés de creació.

			Un disc dur virtual. A la línea de comandes posem aquesta comanda: qemu-img create -f qcow2 debian9.qcow2 11G

- 2/4 Quin tipus de sistema operatiu i versió has seleccionat?

			Un debian 9.5 (Linux)

- 3/4 Quanta memòria i fils de processador has seleccionat? Indica també el total físic que té la teva màquina

		1048MiB de memòria RAM i 2 fil de processador. La màquina en total té 3831MiB i 2 Fils. 

- 4/4 Quin és el nom que li has posat a la màquina?

		debian

2. **Una vegada instal.lat ves al maquinari de la màquina virtual dins el virt-manager i indica:**
- Quina interfície de disc dur fa servir?

		IDE Disk 1

- Quin model de tarja de xarxa fa servir?

		rtl8139

- Quina tarja de vídeo té?

		QXL

- Quina pantalla (display) té?

		Spice Server

- Quin processador s'ha seleccionat?

		Nehalem-IBRS

3. **Sense aturar la màquina virtual afegiu-hi una segona tarja (model virtio) de xarxa des del maquinari. Feu que aquesta xarxa sigui la d'amfitrió.**
- Comproveu dins la màquina virtual Fedora si ha aparegut (sense reiniciar-la) la tarja virtual. Podeu fer servir la comanda **ip address show**.

		Ho fa

- Indiqueu quines xarxes teniu, la seva adreça ip i identifiqueu cadascuna segons sigui NAT o d'amfitrió.

		Adreça 1: 52:54:00:47:ed:9e Adreça 2: 52:54:00:ec:88:0e Adreça 2 es l'amfitrió i la 1 la NAT

4. **Proveu ara (sense aturar la màquina virtual) de canviar la memòria RAM. Mireu dins la màquina (top/htop) si ha canviat. Quines conclusions en treieu?**

		No, no ha canviat. Que per poder canviar el tamany de les memòries RAM es necessari fer un reinici del sistema. 

5. **Mireu al maquinari de la màquina virtual quines opcions d'arrencada disposa i indiqueu-los aquí.**

		IDE DISK i IDE CDROM 

6. **Amb la màquina arrencada feu ús de la línia d'ordres i amb l'ordre virsh indiqueu les ordres per tal de:**
- Suspendre la màquina virtual (pausar): virsh suspend

- Resumir una màquina virtual pausada (continuar executant): virsh resume

- Enviar senyal d'apagada a la màquina virtual: virsh shutdown

- Forçar l'aturada de la màquina virtual: virsh destroy

- Llistar totes les màquines virtuals que tenim definides: virsh list all

- Editar la configuració XML de la màquina virtual: virsh edit

- Iniciar la màquina virtual: virsh start

- Veure la URI de conexió al visor de la màquina virtual: virsh domdisplay

- Desar la configuració XML de maquinari de la màquina virtual en un fitxer anomentat Fedora.xml: virsh dumpxml Debian9

- Eliminar la màquina virtual: virsh undefine

- Crear de nou la màquina virtual a partir del fitxer de maquinari Fedora.xml virsh define

7. **Amb la utilitat virt-install podem crear màquines virtuals a partir de repositoris que hi ha a Internet o també a partir de ISO d'instal.lació de sistemes operatius. L'haureu d'instal.lar i crear un disc primer on instal.lar el sistema operatiu que farem servir (Ubuntu 18):
- Creem un disc amb qemu-img: qemu-img create -f qcow2 ./ubuntu18.qcow2 8G
- Descarreguem la imatge ISO d'instal.lació de Ubuntu 18.
- Creem la màquina amb l'ordre: virt-install --name Ubuntu18 --ram 1024 --dis
k path=./ubuntu18.qcow2,size=8 --vcpus 1 --os-type generic --os-variant generic --network bridge=virbr0 --graphics vnc,port=5999 --console pty,target_type=serial --cdrom ./ubuntu-18.04.1-live-server-amd64.iso 



8. **Descarregueu del web http://www.qemu-advent-calendar.org/2016/ el dia 6 on hi ha el MenuetOs (també el teniu en aquest directori). Mirant el fitxer run.sh realitzeu:**
- On es troba el fitxer de disc de la màquina MenuetOS i quin nom té? Indiqueu com el podem obtenir.

		Es troba a la casella day 06 i quan es descarrega el nom es day06

- Quina extensió té el fitxer d'imatge de disc? 

		run.sh

- Quin format té aquesta imatge de disc? 

		.img
		
- De quin tipus de dispositiu d'emmagatzematge es tracta?

		floppy

- Quin tamany té el fitxer de disc d'emmagatzematge on hi ha instal.lat el sistema operatiu MenuetOS?

        1.5MB

- Quin maquinari es farà servir per arrencar aquesta màquina virtual? Indiqueu emmagatzematge, memòria i tarja de so.
        
        Un HDD de 1.5MB, una tarja de so ac97 i una memòria de 512MB 

- Quina és la comanda qemu-system-x86_64 complerta que haurem d'executar per tal que arrenqui?

       `QEMU -M accel=kvm:tcg -soundhw ac97 -m 512 -drive file=M32-086.IMG format=raw,if=floppy`

9. **Creeu ara aquesta mateixa màquina virtual MenuetOS mitjançant l'entorn gràfic del virt-manager i arrenqueu-la. Indiqueu com l'heu configurat:** 
- 1/4 Com volem instal.lar el sistema operatiu?

        

- 2/4 Quin emmagatzematge has fet servir? Detalla'n el procés de creació.



- 2/4 Quin tipus de sistema operatiu i versió has seleccionat?



- 3/4 Quanta memòria i fils de processador has seleccionat? Indica també el total físic que té la teva màquina



- 4/4 Quin és el nom que li has posat a la màquina?



- Heu hagut de realitzar algun canvi en el maquinari per defecte per tal que pogués arrencar?



10. **Creeu ara una màquina virtual Windows que faci servir per a la interfície de xarxa i de disc dur els controladors virtio. Indiqueu el procés seguits (passos)**
