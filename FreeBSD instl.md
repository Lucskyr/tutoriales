Notas para Instalación y configuración de FreeBSD

Instalación normal de FreeBsd con ISO en VM
agregar user al group wheel

Iniciar como Root, instalar pkg actualizar con update y upgrade
Instalar sudo y paquetes( nano ) 
Agregar al usuario nuevo a la lista de sudoers con:
	ee /usr/local/etc/sudoers
	“user ALL(ALL=ALL) ALL” 

Interfaz grafica y Escritorio
Para la interfaz de usuario grafica y escritorio se instala:
	xorg con: pkg install -y xorg
	xfce con: pkg install -y xfce
	
		pkg install -y slim slim-themes

		habilitar dbus para que los procesos en el escritorio se puedan comunicar, con: 
			sysrc dbus_enable=YES
		
		habilitar halo para mantener las db de los dispositivos conectados con el SO en Tiempo Real, con: 
			sysrc hald_enable=YES
		
		habilitar slim para tener un tema de escritorio con:  
			sysrc slim_enable=YES
		
		habilitar tarjeta de sonido con:  
			sysrc sound_load=YES
		
		habilitar controladores de sonido con:  
			sysrc snd_hda_load
		
		crear y editar el archivo: .xinitrc en la ruta /home/“username”/.xinitrc, con: 
			nano /home/“username”/.xinitrc
			agregamos al archivo:  exec startxfce4
			guardamos y salimos

	Instalar paqueteria de aplicaciones básicas para el usuario

Reiniciar el sistema con reboot 
Listo tenemos un retorno de escritorio funcional!


Configuración de NIC 
ee /etc/rc.conf

	ifconfig_em0=“DHCP”		o static:
							recordar que para ip estática la configuracion de red en la VM tiene que estar en modo puente
	ifconfig_em0=ïnet 192.168.1.xxx netmask 255.255.255.0”
	defaultrouter=“192.168.1.255”

ifconfig
netstat -r
service netif restart && service routing restart


Instalación y Configuración de DNS (Nombres de dominio)

Busca e Instala bind9 con:
	pkg search bind9
	pkg install bind9xx
		inicia configurando clave de bind con: rndc-confgen -a
		agrega a /etc/rc.conf con: ee  /etc/rc.conf 
									named_enable=“YES”
		inicia el servicio named con:
			/usr/local/etc/rc.d/named start

		comprueba el funcionamiento de named con:
			/usr/local/etc/rc.d/named status
		
		cambiar y remplazar en el resolv.conf el nombre de dominio con:
			ee /etc/resolv.conf 
				search “DomNamServ”.com	o	.org	  .net 
				nameserver “ 192.168.1.xxx ”	o 	“ IP ” de ifconfig	
					
					Probar la resolución de una dirección con: dig www. xxx .com
					No! responde 
				
				Modifica el listen-on del archivo named.conf en el directorio /usr/local/etc/namedb con:
					ee /usr/local/etc/namedb name.conf
								listen-on    {127.0.0.1; agregando el IP; }; que abilita la interfaz
							Eje:	listen-on    {127.0.0.1; 192.168.1.xxx; };	
			
			Se reinicia el servicio con:
				service named restart

			Probar nuevamente la resolución de una dirección con: dig www. xxx .com 
			Resuelve!!!  ;)


	Para configurar Zona de dominio

		GitHub
User: lucskyr@gmail.com
Pasw: darqad-havbik-0rizWa
