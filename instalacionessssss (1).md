## Sobre la instalación {#sobre_la_instalacion} 

### Prerequisitos para la instalación {#prerequisitos_para_la_instalacion}  

> 1. Un computador con procesador de 64 bits de la familia x86 (hay imagenes de versiones anteriores a la 4.9 para procesadores de 32 bits).

> 2.  Componentes básicos para la instalación en un medio que pueda acceder. Es decir en uno de los siguientes:

>>  - DVD: Puede descargarlo de 
[ftp://ftp.pasosdeJesus.org/pub/AprendiendoDeJesus por ejemplo con el programa **ftp** de OpenBSD](ftp://ftp.pasosdeJesus.org/pub/AprendiendoDeJesus por ejemplo con el programa **ftp** de OpenBSD):

```
	ftp -C ftp://ftp.pasosdeJesus.org/pub/AprendiendoDeJesus/AprendiendoDeJesus5.9-amd64.iso
```

o bien con el programa **wget**:

```
	wget -c ftp://ftp.pasosdeJesus.org/pub/AprendiendoDeJesus/AprendiendoDeJesus5.9-amd64.iso 
```                   

>> Usando estas formas podrá reanudar la transferencia en caso de que se interrumpa. Otra posibilidad que tiene que puede acelerar la descarga, pero sin posibilidad de reanudar en caso de que se interrumpa, es emplear el programa **rsync**:

```
	rsync -vz rsync://ftp.pasosdeJesus.org/AprendiendoDeJesus/AprendiendoDeJesus5.9-amd64.iso .  
```

>> Note que hay un punto al final, que indica que el destino de la copia es el directorio actual. Si desea ver que otros archivos hay en el mismo directorio de esa imagen ISO utilice:

```
	rsync -vz rsync://ftp.pasosdeJesus.org/AprendiendoDeJesus/
```

>> - O bien partición ext2 (de Linux), ffs (de OpenBSD) o FAT (de DOS y Windows) con juegos de instalación.

>> - O bien conexión a Internet o una Intranet con una tarjeta de red (el disco de instalación NO soporta PPP ni SLIP) y un espejo del servidor FTP o HTTP que se pueda acceder rapidamente desde su computador.

> Aunque es posible realizar la descarga de imagenes ISO, recomendamos comprar[^instalacion.4] los CDs oficiales de instalación para apoyar los proyectos OpenBSD y adJ. La estructura del CD oficial de OpenBSD tiene derechos de reproducción restrictivos ---sólo la estructura, las fuentes son de libre redistribución en su mayoría cubiertas por la licencia BSD. Por esto en caso de comprar CDs oficiales comprelos sólo a los desarrolladores o a redistribuidores autorizados. En el caso de OpenBSD ver [http://www.openbsd.org/orders.html](http://www.openbsd.org/orders.html) y en el caso de adJ ver [https://aprendiendo.pasosdeJesus.org](https://aprendiendo.pasosdejesus.org/).

3. Contar con hardware soportado. La mayoría de componentes típicos son soportados, aunque hay excepciones por lo que antes de comprar se recomienda consultar la lista completa de los dispositivos para procesadores amd64 que son soportados en: [http://www.openbsd.org/amd64.html](http://www.openbsd.org/amd64.html).

4. Una partición primaria de al menos 13GB en un disco duro, que comience en un cilindro arbitrario, pero en la cabeza 1 ---en lugar de la cabeza 0. Si planea comprar un computador con otro sistema operativo puede solicitarle al vendedor que le deje disponible una partición con estas características. Durante la instalación podrá asignar a esta partición el tipo OpenBSD (A6), podrá dividirla en etiquetas que son como subparticiones lógicas sólo visibles en OpenBSD (e.g para */, /home y /var*) sobre cada una de las cuales podrá emplear el sistema de archivos de OpenBSD (Fast File System o *ufs* en terminología Linux). También podrá montar particiones de otros sistemas operativos, por ejemplo ext2 está bien soportado, o tener un sistema dual (ver [Sección 7, “Instalaciones duales](#duales”)).

> Si en su computador no tiene una partición disponible, puede intentar cambiar el tamaño de una existente para liberar espacio y crear una nueva (sacando copia de respaldo antes de estas operaciones). Si una de las particiones tiene sistema FAT o FAT32 (e.g con Windows 9x o XP) puede usar *fips*. Si la partición que desea redimensionar tiene formato ext2fs (Linux) puede usar parted o resize2fs. En el caso de particiones NTFS (Windows Vista o 7) puede usar ntfsresize desde un sistema Linux o arrancando con un disquette como [PAUD](http://paud.sourceforge.net/), o bien si prefiere utilidades gráficas para reparticionar puede arrancar desde un pequeño CD de rescate como RIP o por ejemplo con un CD instalador de Ubuntu.

 5. Leer la fe de erratas que incluye solucionas a eventuales problemas que tendrá durante la instalación o cuando concluya junto con soluciones: [http://aprendiendo.pasosdejesus.org/?id=AdJ+5.9+-+Aprendiendo+de+Jesus+5.9](http://aprendiendo.pasosdejesus.org/?id=AdJ+5.9+-+Aprendiendo+de+Jesus+5.9)

### Ayudas para la instalación  {#ayudas_para_la_instalacion}  

Es muy recomendable consultar guía de instalación de OpenBSD.

Si tiene el DVD oficial, configure su BIOS para que arranque por este y reinicie[^instalacion.2]. Este DVD contiene un sistema OpenBSD mínimo que detectará automáticamente el hardware y lo guiará en el proceso de instalación. Si había creado con anterioridad la partición primaria para OpenBSD seguramente no tendrá inconveniente en esta instalación, basta que tenga en cuenta algunas diferencias entre Linux y OpenBSD:

Nombres y manejo de dispositivos
**Tabla 1. Nombres de dispositivos**

**Dispositivo** 	**Linux**       	**OpenBSD**
Disco IDE 1 maestro	/dev/hda	/dev/wd0c o en modo crudo /dev/rwd0c
Disco SCSI	/dev/sda	/dev/sd0c o en modo crudo /dev/rsd0c
Mouse	/dev/psaux o /dev/ttyS0, /dev/ttyS1 ...	/dev/wsmouse si es PS/2. Si es serial uno como /dev/tty00 o /dev/tty01.
Teclado	/dev/kbd	/dev/wskbd
Primera unidad de disquete	/dev/fd0	/dev/fd0c[a]
Primera unidad de DVD	Si es IDE algo como /dev/hdb, si es SCSI algo como /dev/sda	/dev/cd0c
[a] En discos, DVDs y disquettes las particiones se indican con a, b, d y letras que se usan como postfijo. El postfijo c representa el disco/DVD/disquette completo. Por ejemplo en el caso del primer disco IDE las particiones pueden ser: /dev/wd0a, /dev/wd0b, /dev/wd0d y así sucesivamente, mientras que /dev/wd0c representa el disco completo. Pueden verse las particiones precisas que se usan del primer disco IDE con **disklabel /dev/wd0c**

> En Linux los controladores están en módulos algunos de los cuales deben configurarse con herramientas externas al kernel o manualmente. En OpenBSD los controladores están integrados en el kernel y normalmente detectan los dispositivos y se configuran automáticamente. Si durante el arranque del disquete o del sistema algún dispositivo soportado no logra ser detectado o configurado puede emplear **boot -c** en el prompt de arranque para entrar a un editor de las variables de configuración del kernel y ajustar manualmente los recursos para el dispositivo.

Sistema básico y portes
> Notará que la instalación es muy corta porque sólo se instala un sistema básico, que consta del kernel, comandos básicos (de */bin y /sbin y /usr/lib*), archivos de configuración (de /etc) y eventualmente, si los escoge al instalar, compilador, documentación y el servidor X-Window. Estos componentes conforman OpenBSD y han sido ampliamente auditados.

> Después de instalar el sistema básico podrá instalar programas que han sido portados pero que no han pasado por un proceso de revisión tan intenso como el de los componentes básicos (ver [Paquetes y portes](#paquetes_y_portes)).

Particiones
> OpenBSD puede dividir la partición que haya destinado para este sistema en "subparticiones". Tenga en cuenta no transpasar los límites de la partición que reservó para OpenBSD al definir esta subparticiones con el programa **disklabel** (el mismo programa le ayudará a evitarlo). Los componentes básicos del sistema estarán especialmente en /usr, mientras que los paquetes emplearán */usr/local*

> Para un sistema de escritorio puede bastar dividir el espacio de su partición para OpenBSD en: */* (al menos con 3G o si desea instalar los paquetes incluidos en el DVD y para compilar fuentes al menos con 50G), */home* (con tanto espacio como desee para los usuarios) y	*/var* (al menos con 1.2G si planea sacar respaldos de imagenes encriptadas en CD o de 8G si planea sacarlas en DVD).

Interprete de comandos
> Por defecto emplea **ksh** que es muy parecido a **bash**. Hay también un paquete de **bash** que podría instalar después de tener en operación el sistema básico.

Herramientas UNIX
> Notará también que otros programas (como **sed**, **awk**, **tar** y **make**) tienden a conformar el estándar POSIX pero no algunas extensiones comunes en sistemas Linux.

[^instalacion.1] Los CDs de OpenBSD ordenados por la página web de OpenBSD si llegan a Colombia.

[^instalacion.2] El método de instalación por red PXE requiere que configure otro servidor con los servicios DHCP para asignarle dirección durante la instalación y TFTP para que pueda servirle el archivo de arranque boot y un kernel como bsd


# En ftp://ftp.pasosdejesus.org/pub/AprendiendoDeJesus dice que no se puede acceder a este sitio web

# RIP http://www.tux.org/pub/people/kent-robotti/looplinux/rip/ no funciona

# error en listas