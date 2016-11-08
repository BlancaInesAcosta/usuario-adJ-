
 # Configuración y uso de algunos dispositivos y servicios
   {#configuracion_y_uso_de_algunos_dispositivos_y_servicios}

### Reconocimiento de hardware durante el arranque
  {#reconocimiento_de_hardware_durante_el_arranque}
    
Durante el arranque el kernel detecta hardware usando diversos controladores en cierto orden. El orden y los controladores que se usen dependen de la configuración que se haya dado al kernel al compilarlo. El orden típico configurado en el kernel genérico es:

- Procesador, Memoria, BIOS

- Discos duros, CDROM y unidades de disquete

- Buses, Teclado, Dispositivos ISA

- Puertos paralelos y seriales

- Dispositivos ISA plug and play

La configuración de la mayoría de estos dispositivos puede ser determinada por el controlador, pero con algunos dispositivos (e.g ISA, ciertos teclados) el kernel incluye valores predeterminados que en algunos casos pueden no corresponder al hardware o que por la forma de detectar congelan el sistema completo. En tal caso, determine los recursos que el dispositivo tiene configurados con jumpers o con software (e.g IRQ, dirección de entrada salida base, DMA) y modifique los valores que emplea el kernel de una de estas formas (en un caso extremo puede tener que deshabilitar el dispositivo):

- Arranque con **boot -c** y cambie parámetros usados por los controladores antes de continuar con la detección automática de hardware. El cambio sólo durará mientras no reinicie el sistema, puede hacer el cambio durable por ejemplo con el comando: **config -u**.

- Después de que su sistema haya iniciado modifique los recursos asignados por defecto del kernel con **config -e -o /bsd.new /bsd**.

- Recompile el kernel con una configuración apropiada.

#### Modificación antes de arrancar {#modificacion_antes_de_arrancar}

Al arrancar, cuando esté en el prompt boot>, inicie con:

	    boot> boot -c
	  
Así entrará a un entorno interactivo, que le permite cambiar la asignación de recursos predeterminada para algunos dispositivos. Por ejemplo si tiene una tarjeta de red compatible NE2000 con dirección base 0x300 e IRQ 3, el controlador ne la asignará la interfaz ne1 que por defecto usa la dirección base 0x300 pero la IRQ 10. Desde el prompt que **boot -c** produce ingrese:

	    UKC> change ne1
	  
puede ver otros comandos disponibles con el comando **help**. Una vez complete la configuración salga del entorno interactivo con **quit**, tras esto el kernel continuara la detección pero usando los cambios que haya hecho.

Una vez haya configurado los recursos que un controlador emplea para que correspondan a los de un dispositivo y que su sistema esté operando, puede hacer el cambio permanente usando:

	doas config -e -o /bsd.ne1 -u /bsd
	  
que le permitirá al mismo entorno interactivo, que procurará detectar los cambios que usted haya podido hacer durante el arranque, le permitirá completarlos y cuando salga (con **quit**) escribirá un nuevo *kernel /bsd.ne1* con la misma configuración de /bsd pero con sus cambios aplicados.

Si no logra configurar algún dispositivo y esto hace que el sistema completo se congele durante el arranque, puede requerir deshabilitar el dispositivo (situación que es muy inusual).

#### Lecturas recomendadas {#lecturas_recomendadas}  

- Página del manual unix de **autoconf**(8). autoconf determina el orden y los controladores que se usan para la detección automática de hardware.

- Página del manual unix de boot_config() y de 8() donde se describen los comandos aceptados por el entorno interactivo de configuración de dispositivos.

- Puede encontrar más sobre la secuencia de arranque de OpenBSD por ejemplo en http://dhobsd.pasosdeJesus.org/pres21abr2005/

### Impresión {#impresion}

Para atender trabajos de impresión hay disponibles para Unix varios sistemas: lpd, LPRng, CUPS, QPD. El más popular y el incluido en la distribución básica de OpenBSD es **lpd** que además de poder llevar cuentas de cada usuario, permite a varios computadores en red compartir una misma impresora.

#### Uso de una impresora ya configurada 
{#uso_de_una_impresora_ya_configurada}  

Aun cuando algunos programas ofrecen menús con una opción para imprimir, prácticamente todos usan en últimas el programa **lpr** (o **/usr/local/bin/lpr** si está usando CUPS) que se encarga de poner la información que debe enviarse a la impresora en una cola de trabajos pendientes [^conf_uso_dispositivos.1], que es atendida automáticamente (i.e de ella se envían trabajos pendientes cuando la impresora está disponible) por un programa.

Cada impresora configurada tiene un nombre, la impresora por defecto se llama **lp** [^conf_uso_dispositivos.2].

Posiblemente su sistema esté configurado para permitir a **lpr** la impresión de textos planos y documentos PostScript o PDF. Para programar la impresión de un texto plano con nombre *tarea.txt* en la impresora por defecto, puede emplear:

      lpr tarea.txt
    
o eventualmente para especificar una impresora diferente a **lp**, digamos **imp2** use la opción -P:

      lpr -Pimp2 tarea.txt
    
Puede examinar la cola de sus impresiones pendientes con lpq que junto con los nombres de las impresiones pendientes presentará un número que la identifica. Tal número le permitirá cancelar una de sus tareas de impresión pendiente, usándolo como parámetro de **lprm**.

Para imprimir gráficas y documentos con diversos tipos de letra o colores, primero debe convertirse la información a secuencias de control particulares de su impresora. Cómo de una impresora a otra varían los secuencias de control, en sistemas tipo Unix con LPD suele emplearse PostScript (que es un lenguaje apropiado para documentos por imprimir) como formato común y se usan los filtros del programa Ghostscript para traducir de PostScript al formato particular de su impresora [^conf_uso_dispositivos.3]. Si se usa CUPS es posible emplear controladores particulares para su impresora y no depender de Ghostscript.

Ghostscript es un programa que puede leer documentos PostScript y PDF [^conf_uso_dispositivos.4] y presentarlos en una ventana de X-Window o transformarlos al lenguaje particular de algunas impresoras. Por ejemplo para generar a partir de un documento PostScript *s2.ps* la secuencia de caracteres apropiada para una impresora LaserJet en el archivo *s2.lj* se usaría:

	gs -sDEVICE=laserjet -sOutputFile=s2.lj s2.ps -c quit       
      
O para imprimir directamente usando **lpr**, en lugar de *s2.lj* puede usar **-sOutputFile=\|lpr**. Puede experimentar con este intérprete tecleando **gs** y por ejemplo ingresando la siguiente secuencia de instrucciones en lenguaje PostScript:

	100 100
	moveto
	200 200
	lineto
	stroke
      
Para visualizar un documento PostScript o PDF puede emplear el programa **gv** ---el cual se apoya en Ghostscript--- por ejemplo:

	gv micarta.ps
    
mostrará el documento PostScript micarta.ps en una ventana de X-Window y con menús le permitirá consultarlo e imprimirlo.

Para imprimir y hacer transformaciones a un PostScript (por ejemplo 2 páginas en una sola), o para convertir de otros formatos a PostScript puede emplear el programa **a2ps**:

	a2ps --columns=2 -o micarta-2.ps micarta.ps
      
o **mpage**:

	mpage -2 micarta.ps > micarta-2.ps
      
Además de los programas *a2ps* y del paquete psutils es posible modificar directamente un archivo PostScript. Por ejemplo siguiendo las indicaciones de http://www.ghostscript.com/pipermail/bug-gs/2001-August/000641.html, resulta posible rotar una página, editando el archivo y en la sección %%%BeginPageSetup agregando después de establecer tamaño de página, *e.g. 595 842 /a4 setpagesize*:

	currentpagedevice /PageSize get aload pop translate
	180 rotate
    
Tanto PostScript como PDF requieren bastante espacio para describir un documento, usualmente los documentos PDF requieren menos porque mantienen la información comprimida. Para convertir entre PostScript y PDF se emplean **ps2pdf** y **pdf2ps** [^conf_uso_dispositivos.5]. Para visualizar e imprimir un PDF, además de **gv**, puede emplear **xpdf** o el programa Acrobat Reader (**acroread**).

#### Configuración de una impresora local con lpd    
{#configuracion_de_una_impresora_local_con_lpd}

lpd (también llamado Berkeley line printer spooling system) es un sistema que maneja una cola de impresión por cada impresora configurada, filtros para convertir información de diversos tipos (e.g PostScript y PDF) al lenguaje particular de cada impresora así como filtros de cuentas que permiten monitorear y controlar el uso de cada impresora por parte de cada usuario. LPD recibe y procesa solicitudes de impresión realizadas por programas cliente e imprime la información de las colas en los dispositivo configurados para cada cola (*e.g /dev/lpt0* o enviando por red a una impresora remota).

En OpenBSD lpd debe activarse durante el arranque, agregando en */etc/rc.conf.local* la línea:

	lpd_flags="" 
	
su archivo de configuración principal es /etc/printcap el cual es leído cada vez que lpd inicia y	donde se configura cada cola de impresión, el dispositivo asociado a cada una y sus parámetros, los filtros para los diversos tipos de información que puede imprimir y filtros de cuentas.

lpd es un servidor TCP/IP que imprime trabajos pendientes en las colas de impresoras locales (sólo modificables con programas clientes) y que atiende conexiones de clientes que emplean el protocolo LPD en el puerto TCP 515. La conexión normalmente la realiza un programa cliente como *lpr, lpq, lprm o lpc* que pueden solicitar una de las siguientes operaciones:

- Revisar trabajos en cola de una impresora.

- Recibir y poner en cola de una impresora un trabajo de otra máquina.

- Listado corto del estado de los trabajos de un usuario en la cola de una impresora.

- Listado largo del estado de los trabajos de un usuario en la cola de una impresora.

- Eliminar trabajos de un usuario de la cola de una impresora

**lpd** envía errores a la bitácora */var/log/lpd-errs*.

Además de esta documentación puede consultar más sobre la configuración de **lpd** en [FreeBSDHandBook] y [OpenBSDlpd]

La mayoría de impresoras se conectan al puerto paralelo (por ejemplo utilizables con dispositivos */dev/lpt0 o /dev/lpt1*). Puede probar que su impresora funciona enviando una cadena sencilla al puerto apropiado por ejemplo *echo "Hola" >/dev/lpt0 o lptest > /dev/lpt0*.

El siguiente es un ejemplo del archivo */etc/printcap*:

	  lp|local line printer:\
	  :lp=/dev/lpt0:sd=/var/spool/output:lf=/var/log/lpd-errs:
	
que configura una impresora local para textos llamada *lp o local line printer*. Está conectada a */dev/lpt0*, la cola de impresión la mantiene en */var/spool/output* y envia errores a */var/log/lpd-errs*. Para configurar impresoras que puedan imprimir gráficas (así como PostScript con diversos tipos de letra) debe emplear un filtro del programa Ghostscript.

#### CUPS {#cups} 

##### Instalación {#instalacion} 

Hay un paquete de CUPS entre los portes oficiales de OpenBSD 5.9. Una vez instalado puede ejecutarlo con

	doas cupsd
	
Para que en cada arranque se ejecute automáticamente, se recomienda agregar en *rc.local*:

	if (test -x /usr/local/sbin/cupsd) then {
		echo -n ' cupsd';
		/usr/local/sbin/cupsd;
	} fi;
	
La distribución estándar de cups incluye unos pocos archivos ppd, así que posiblemente tendrá que generar o conseguir los apropiados para su impresora con **foomatic** (ver Sección 6.2.4, “foomatic”).

##### Configuración {#configuracion} 

La configuración de CUPS (por ejemplo para añadir una impresora)	puede hacerse con facilidad una vez esté ejecutándose el servidor, usando un navegador y abriendo en la máquina donde corre CUPS el puerto 631, por ejemplo **http://127.0.0.1:631**. Para hacer labores administrativas ingrese con la identificación y la clave de un usuario que esté en el grupo *sys* o por la cuenta *root*.

También es recomendable que cree un directorio:

	doas mkdir /var/spool/cups/tmp
	doas chown _cups:_cups /var/spool/cups/tmp/
	
Si tiene problemas al instalar/usar una impresora puede consultar el archivo de errores de **cups** en: */usr/local/var/log/cups/error_log*.

##### Utilización de CUPS {#utilizacion_de_cups} 

El paquete de **cups** cuenta con programas análogos a los de LPD, pero ubicados en */usr/local/bin*. Diversos programas emplearan el comandos **lpr** para hacer impresiones, así que tiene dos opciones:

Ejecutar **cups-enable** que remplazará los ejecutables relacionados con impresión de */usr/* con los de CUPS disponibles en */usr/local/*.

Configurar cada programa para que en lugar de emplear */usr/bin/lpr* utilice */usr/local/bin/lpr*. Por ejemplo así debe configurar xpdf.

#### Foomatic {#foomatic} 

**foomatic** ofrece gran cantidad de controladores para una variada gama de impresoras, estos controladores e instrucciones para impresoras particulares están disponibles en http://www.openprinting.org Es posible emplearlos bien con lpr o bien con cups.

##### Foomatic con LPR  {#foomatic_con_lpr} 

Si la impresora está impresora está conectada a */dev/lpt0* basta que agregue a */etc/printcap* una entrada como:

    lp|local line printer:\
    	:lp=/dev/lpt0:\
            :af=/etc/foomatic/laserjet6p.ppd:\
            :if=/usr/local/bin/foomatic-rip:\
            :sd=/var/spool/lpd:\
            :lf=/var/log/lpd-errs:\
            :mx#0:sh:
            
##### Foomatic con CUPS {#foomatic_con_cups 

Cada archivo ppd que descargue debe ubicarlo en: */usr/local/share/cups/model/*. Después de agregar archivos en ese directorio reinicie CUPS para que lea nuevamente sus archivos de configuración con:

```# pkill -HUP cupsd```
			  
### Discos duros {#discos_duros} 

#### Particiones {#particiones} 

Hay dos niveles de particiones: (1) del BIOS y (2) particulares de OpenBSD. Las primeras se configuran con **fdisk** y las segundas se crean dentro de las primeras con **disklabel**.

#### Sistema de archivos ffs {#sistema_de_archivos_ffs} 

Un sistema de archivos ffs sólo puede crearse en una partición identificada con **disklabel**. Como se explica en [FFS], consta de:

- Un superbloque con los parámetros básicos del sistema de archivos (e.g tamaño de cada bloque, lista de grupos de cilindros) y con los parámetros del hardware que afectan el desempeño[^conf_uso_dispositivos.6] (tipo de transferencia de disco a memoria, tiempo esperado por transferencia, bloques por pista, velocidad de rotación).

- Copias de seguridad del superbloque en diversos bloques del disco.

- Archivos, algunos de los cuales pueden ser directorios. Todo archivo tiene asociado un nodo-i, que mantiene información del archivo (dueño, tiempo de última modificación y de creación, algunos de los bloques que emplea el archivo y referencia a otros nodos-i con el resto de bloques).

Los bloques pueden fragmentarse para aprovechar espacio cuando se mantienen archivos pequeños. Cada grupo de cilindros tiene una tabla de fragmentos libres en los bloques del grupo e información sobre bloques libres de acuerdo a las diversas posiciones rotacionales (para optimizar ubicación de información de acuerdo al hardware disponible).

Algunos bloques se reservan para que sólo puedan ser usados por el administrador (reserva de espacio libre). En OpenBSD por defecto es 5% del espacio total.

Entre las características soportadas por FFS están:

- Enlaces duros dentro de una misma partición o enlaces simbólicos. Candados, nombres largos para archivos, validaciones y cuotas (para limitar espacio utilizable por los usuarios).

Para revisar un sistema de archivos *ffs* (digamos que esté en la partición */dev/wd1j*) ejecute:

	doas fsck_ffs /dev/rwd1j
      
note la *r* antes de *wd1j* que indica que la partición debe tratarse en modo puro (del inglés *raw*). Puede agregar la opción -y antes del dispositivo para responder si por defecto a toda pregunta (intentado que solucione automáticamente todos los problemas). En algunos casos cuando un disco está bastante inconsistente en su estructura, puede perderse información que *fsck_ff*s intentará recuperar y dejar en archivos cuyos nombres son números en el directorio *lost+found*. En los casos que se dañe el superbloque de la partición que desea revisar *fsck_ffs* se negará a realizar el chequeo, en tales casos puede intentar con un superbloque de respaldo --típicamente hay uno en el bloque 32:

	doas fsck_ffs -b 32 /dev/rwd1j
      
Para crear un sistema de archivos *ffs* en un disco ya particionado con **disklabel** puede emplearse **newfs**, por ejemplo:

	doas newfs -t ffs /dev/wd1j
	
![Aviso](img/warning.png)	**Aviso**

Al crear un nuevo sistema de archivos se borra la información que pudiera haber existido.

Para detectar particiones *ffs* en un disco, puede emplearse **scan_ffs**.

Para examinar la estructura de una partición con *ffs*, se emplea **dumpfs**. Puede afinarse un sistema ffs con **tunefs**

#### Zonas de intercambio (swap) {#zonas_de_intercambio_(swap)} 

Son porciones de un disco duro que pueden emplearse como si fuera memoria RAM (aunque es mucho más lenta). Si por ejemplo desea agregar como dispositivo de intercambio el disco */dev/wd1l* debe:

1. Asegurarse de poner tipo swap a la partición. Puede emplear **disklabel**. Por ejemplo puede emplear el modo interactivo de este programa:

	sudo disklabel -E /dev/wd1c 
	
en este modo puede examinar las particiones y divisiones del disco con **p**, puede ver una ayuda abreviada con **h**. Con **m** le será posible cambiar el tipo de una partición (por ejemplo puede ser *4.2BSD* si se trata de un sistema *ffs* o *swap* si se trata de un dispositivo para intercambio), y la ubicación.

![Atención](img/caution.png)	**Antes de cambiar la ubicación de una partición**

El sitio donde reubique una partición NO debe estar traslapado sobre una partición ya existente. Si traslapa una partición sobre otra, la información que hubiera en la partición sobre la que traslapa puede perderse de manera irreversible.

2. Agregue una línea a su archivo */etc/fstab* con el dispositivo de intercambio, punto de montaje none, tipo swap y opción *sw*:

	/dev/wd1l none  swap sw 0 0 
Con este cambio, el dispositivo será montado como zona de intercambio cada vez que el sistema inicie (está en */etc/rc*).

3. Intente agregar el dispositivo como zona de intercambio sin reiniciar con:

	sudo swapon -a 
o con:

	sudo swapctl -A -t blk 
	
Ambos comandos intentarán montar como zonas de intercambio todos dispositivos por bloques de */etc/fstab* que tengan la opción *sw*. Puede verificar la adición listando todas las zonas de intercambio con:

	sudo swapctl -l 
	
	
[^conf_uso_dispositivos.1] En el caso de LPD el directorio con la cola de trabajos de una impresora se configura en /etc/printcap, en el caso de lp el directorio por defecto es /var/spool/outputlpd/lp.

[^conf_uso_dispositivos.2] Normalmente puede configurar otro nombre para la impresora por defecto en la variable de ambiente PRINTER.

[^conf_uso_dispositivos.3] Hay algunas impresoras que pueden imprimir PostScript directamente, pero en general para hacer la traducción de PostScript al lenguaje de una impresora se requiere un filtro que el administrador del sistema debe configurar.

[^conf_uso_dispositivos.4] PDF (Portable Document Format es otro lenguaje para impresión, de documentos con gráficas y diversos tipos de letras, basado en PostScript (de la misma compañía ---Adobe).

[^conf_uso_dispositivos.5] De acuerdo a Printig-HOWTO estas herramientas ofrecen la funcionalidad de las herramientas "distiller" de Adobe.

[^conf_uso_dispositivos.6] Los parámetros del hardware mantenidos ayudan a localizar bloques libres de forma óptima.