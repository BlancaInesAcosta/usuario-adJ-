## Paquetes y portes {#paquetes_y_portes}

Diversos programas han sido portados a OpenBSD; a las fuentes portadas se les llama portes y a los binarios de alguno de los portes compilados para alguna plataforma se les llama paquetes.

### Paquetes {#paquetes}

Cada paquete es un archivo con extensión .tgz que incluye información de dependencias y scripts para instalar y desinstalar. Para manejarlos se emplean los programas **pkg_add, pkg_delete**, **pkg_info y pkg_create**. El programa **pkg_add** instala un paquete y todos los que este requiera, usando para descargarlos lo(s) repositorio(s) especificada(s) en la variable de ambiente PKG_PATH, por esto en el archivo *~/.profile o en ~/.xsession* vale la pena agregar:

	```export PKG_PATH=ftp://ftp.pasosdeJesus.org/pub/OpenBSD/5.9/packages/amd64
```

o la vía del CD-ROM 1 o una vía de un espejo más rápido para su caso[^ paquetes_y_portes.1]. Una vez establecida esta variable, para agregar por ejemplo el paquete *vim-7.4.900-gtk2.tgz*:

	```pkg_add $PKG_PATH/vim-7.4.900-gtk2.tgz``` 
	
Puede ver la lista de paquetes instalados en su sistema con **pkg_info** o con **ls -l /var/db/pkg**, podrá desinstalar uno con **pkg_delete** seguido del nombre del paquete (con la opción *-f* dependencies se eliminarán también paquetes que dependan del que está quitando --que es útil si está actualizando un paquete).

Podrá ver los paquetes disponibles para instalar en [http://www.openbsd.org/5.9_packages/amd64.html](http://www.openbsd.org/5.9_packages/amd64.html). A continuación destacamos algunos que permiten tener un ambiente para producir contenidos, programar, escuchar música y ver vídeos (se incluyen en la distribución [adJ](http://aprendiendo.pasosdejesus.org/)):

**Tabla 2. Paquetes Característicos de adJ**

**Paquete**	**Descripción**
evangelios_dp	Evangelios: traducción de dominio público.
sword	Permite consultar Biblia, incluye módulo KJV
xiphos	Herramienta para estudiar Biblia, usa módulos de sword.
Mt77	Buscar rápido y preciso
basico_adJ	Manual de uso básico de OpenBSD
servidor_adJ	Documentación de OpenBSD como servidor
usuario_adJ	Documentación para usuarios de OpenBSD
asigna	Cuadra horarios en un colegio
markup	Librería para reconocimiento de XML en Ocaml
repasa	Crear y repasar contenidos (incluye currículo)
sigue	Maneja registros valorativos de un colegio.
AnimalesI	Hojas de lectoescritura letra script
AprestamientoI	Hojas de lectoescritura letra script
NombresCursiva	Hojas de lectoescritura letra cursiva
PlantasCursiva	Hojas de lectoescritura letra cursiva
TiposLectoEscritura	Tipos de letra para aprender a leer y escribir
ganglia	Sistema de monitoreo distribuido y escalable
ocaml-labltk	Interfaz en Ocaml para el entorno gráfico Tk
realboy	Emulador de Game Boy Classic/Color

**Tabla 3. Escritorio gráfico**:

**Paquete**	    **Descripción**
audacious	Reproductor multimedia
audacity	Editor de audio
abiword	Procesador de texto que puede importar MS-Word
chromium	Navegador web Chrome
dia	Herramienta para dibujar diagramas técnicos
fbdesk	Permite poner iconos en escritorio fluxbox
fluxbox	Escritorio extra liviano
fluxter	Pagina espacios de trabajo en fluxbox
filezilla	Transmite archivos (soporta protocolo ssh)
fontforge	Editor/conversor de tipos de letra vectoriales
gimp	Editor gráfico por pixels
gnumeric	Hoja de cálculo
gv	Visualizador de PostScript y PDF
inkscape	Editor gráfico vectorial
midori	Navegador web liviano
mplayer	Escuchar y ver diversidad de audio y vídeo
libreoffice	Aplicaciones de oficina
libreoffice-i18n-es	Localización en Español para LibreOffice
pidgin	Mensajería (soporta protocolo SILC)
qemu	Emulador de varios sistemas
qgis	Sistema de información geográfica de escritorio
scribus	Publicación de escritorio
vlc	Reproductor Multimedia
xcdplayer	Reproductor de CDs
xarchiver	Manejador de archivos comprimidos
xfe	Administrador de archivos
xsane	Opera escaner con sane
xpdf	Visualizador de archivos PDF

**Tabla 4. Escritorio consola:**

**Paquete** 	**Descripción**
antiword	Convierte a texto plano archivos de MS-Word
rtorrent	Sistema de distribución de archivos cooperativo
bzip2	Descompresor de formato BZ2
cdparanoia	Utilidad para leer CDDA
cdrtools	Quema CDs y sistemas de archivo ISO 9660
colorls	Comando ls con color
cups	Sistema de impresión
curl	Descarga de servidores FTP, Gopher, HTTP y HTPPS
dfc	Muestra uso de disc
ffmpeg	Conversor y emisor de video con bktr(4)
hexedit	Ver y editar archivos en hexadecimal o ASCII
ImageMagick	Conversión de formatos gráficos
ispell	Diccionario
ispell-spanish	Diccionario en español para ispell
rsync	Para sincronizar copias remotas
scrot	Captura imágenes de pantallas
silc-client	Cliente para conferencia segura
sox	Conversión, grabación, reproducción de sonido
texlive_base	Binarios para distribución TeXLive
texlive_texmf-buildset	Pequeño texmf de texlive para crear portes
texlive_texmf-minimal	texmf con funcionalidad básica
texlive_texmf-full	texmf con macros extras
unzip	Descompresión formato ZIP
vorbis-tools	Reproduce, codifica y maneja OGG
wget	Descarga sitios web
zip	Comprime en formato ZIP

**Tabla 5. Servidor / Plataforma de comunicaciones:**

**Paquete** 	**Descripción**
dovecot	Servidor compacto de IMAP/POP3
ldapvi	Actualiza entradas LDAP con un editor de texto
letsencrypt	Gestiona certificados SSL gratuitos
mariadb-client	Cliente de base de datos SQL de multiples hilos
mariadb-server	Servidor de base de datos SQL de multiples hilos
nginx	Servidor web
openpoppassd	Serivicio para cambio de claves
owncloud	Acceso universal a archivos personales compartidos
p5-Mail-SpamAssassin	Filtro de correo para identificar y marcar spam
php	Lenguaje script empotrado en HTML que opera en servidor
postgis	Soporte para objetos geográficos en PostgreSQL
postgresql-client	Cliente para motor de bases de datos PostgreSQL
postgresql-server	Servidor de bases de datos PostgreSQL
sqlite	Implementación embebida de SQL (versión 2).

**Tabla 6. Seguridad:**

**Paquete** 	**Descripción**
apg	Genera claves aleatorias seguras
john	Auditoría de claves
sleuthkit	Herramienta para análisis forense

**Tabla 7. Desarrollo de programas, documentación y páginas web**

**Paquete** 	**Descripción**
cppcheck	Verificador estático de C/C++
cppunit	Infraestructura para probar programas en C++
gcc	Versión más reciente de gcc
git	Almacenar historia de árbol de directorios
llvm	Compilador modular y rápido de C/C++/ObjC
lua	Lenguaje de programación potente y liviano
ocaml	Lenguaje de programación funcional
python	Lenguaje de programación
ruby	Lenguaje de programación
tcl	Lenguaje de programación
tk	Complemento gráfico para TCL
w3m	Navegador modo texto que soporta UTF-8 y descargas

#### Actualización de paquetes  {#actualizacion_de_paquetes}

Podrá actualizar un paquete con **pkg_add -r -D update paquete.tgz**

Si configura PKG_PATH podrá buscar dependencias que debe actualizar para actualizar un paquete con **pkg_add -u paquete**.

Cada paquete instalado crea un directorio con el nombre del paquete en */var/db/pkg* por lo que además de **pkg_info -L** puede usar

	```ls /var/db/pkg```
        
Y para actualizar todos los paquetes puede utilizar

	```doas pkg_add -u``` 
        
### Portes  {#portes}

Aunque NetBSD y FreeBSD tienen más programas portados que OpenBSD, la colección de portes de OpenBSD ya cuenta con casi 5000 programas clasificados en las siguientes categorías empleadas por el sistema de portes: archivers, astro, audio, benchmarks, biology, books, cad, chinese, comms, converters, databases, devel, distfiles, editors, education, emulators, games, graphics, infrastructure, japanese, java, korean, lang, mail, math, mbone, misc, multimedia, net, news, palm, plan9, print, productivity, russian, security, shells, sysutils, telephony textproc, www, x11.

Puede ver el desarrollo de los portes en: [http://openports.se](http://openports.se/) y más información sobre portes y paquetes en: [http://www.openbsd.org/ports.html](http://www.openbsd.org/ports.html)

Si prefiere compilar los paquetes que va a instalar o desea ayudar a portar aplicaciones a OpenBSD debe tener las fuentes de los portes (parches, makefiles y scripts), que normalmente se ubican en */usr/ports*. Puede bien descomprimirlas de un CD oficial, o de un servidor FTP y después actualizarlas de un repositorio CVS o bien descargarlas de un repositorio CVS como se presenta a continuación:

	```cd /usr
	export CVSROOT=anoncvs@anoncvs.de.openbsd.org:/cvs
	cvs -z3 -q get -rOPENBSD_5_9 -P ports```
	
La vía para CVSROOT adaptela a su servidor más cercano, escogiendo entre los disponibles en: [http://www.openbsd.org/anoncvs.html](http://www.openbsd.org/anoncvs.html).

#### Busquedas de portes  {#busquedas_de_portes}

Para buscar un programa en la colección de portes puede emplear:

	```cd /usr/ports
	make search key=cadena```
	  
Que presentará los portes que en su descripción tengan la cadena dada. Esta busqueda se realiza sobre el archivo */usr/ports/*INDEX, el cual si lo desea también puede explorar con un editor.

#### Compilación de portes  {#compilacion_de_portes}

Para compilar e instalar un porte (tras la compilación normalmente se crea un paquete), basta pasar al directorio del porte y como usuario root ejecutar:

	```make install```
	
El respectivo Makefile obtendrá las fuentes originales de la red, aplicará los cambios necesarios, compilará, creará un paquete y lo instalará (posteriormente podrá eliminar el paquete con **pkg_delete** ver [Paquetes](#paquetes)). Si el porte tiene otros portes como prerequisitos los compilará e instalará antes.

#### Creación de un porte  {#creacion_de_un_porte}

Elija una categoría para su porte (i.e un directorio en */usr/ports*), puede copiar el directorio de un porte similar al que desea hacer y modificarlo. Emplee como nombre de directorio el nombre del paquete (sin versión). Debe tener al menos estos archivos:

Makefile

- El cual incluye descripción corta, nombre del paquete, versión, URL del cual descargar fuentes, el correo de quien mantiene el porte, URL de parches si los hay, información sobre posibilidad de redistribución, foma de configurar, compilar e instalar. Este archivo normalmente es muy corto pues se basa en la infraestructura para portes de */usr/ports/infrastructure/mk/bsd.ports.mk*.

distinfo
- 
- Este archivo incluye diversas "firmas"[^ paquetes_y_portes.2] de cada fuente que debe descargarse para asegurar su integridad. Se genera automáticamente con **make makesum** y se verifica con **make checksum**

pkg/DESCR

- Este archivo incluye una descripción un poco más larga del paquete (la que presenta **pkg_info** cuando se corre sin opciones).

pkg/PLIST

- Indica los archivos que se instalan después de instalar el paquete. Se puede crear un candidato inicial de este archivo después de compilar y hacer una instalación de prueba (make fake). Puede actualizarse de acuerdo a lo instalado con make plist

patches/

- En este directorio se mantienen parches que deben aplicarse a las fuentes descargadas. Puede generarlos automáticamente (con make patches), si en el directorio donde se descomprimen las fuentes, saca una copia de cada archivo que vaya a modificar y le agrega el sufijo *.orig*

Crear un porte es un proceso iterativo, en cada iteración debe refinarse el porte para asegurar que cumple con todos los lineamientos que espera el equipo de portes de OpenBSD. Puede consultar más en [porting-checklist] [porting].

### Emulación de binarios para Linux  {#emulacion_de_binarios_para_linux}

Algunas aplicaciones cuyas fuentes no han sido portadas a OpenBSD pueden ejecutarse con las capas de emulación de OpenBSD. En particular para i386 hay capas de emulación para binarios de Linux, FreeBSD, BSDI y SCO Unix (binarios iBCS2). Aquí se describe brevemente la emulación de binarios para Linux (ver *man compat_linux*).

Para comenzar rapidamente puede utilizar el paquete *redhat_base* que instalará librerías compartidas y archivos básicos de una distribución Redhat con *libc*. Cree un enlace para que la emulación logre ubicar información donde la espera:

	```mkdir -p emul
	ln -s /usr/local/emul/redhat /emul/linux```
      
Active la emulación de Linux en el kernel usando **sysctl -w kern.emul.linux=1** y haga que el cambio se repita cada vez que reinicie, moficando */etc/sysctl.conf*[^ paquetes_y_portes.3] para que incluya la línea:

	```kern.emul.linux=1```
      
Después de hacer esto o de copiar nuevas librerías que algún programa requiera, debe ejecutar ```/emul/linux/sbin/ldconfig.``` Para automatizar esta tarea durante cada arranque puede agregar a su archivo */etc/rc.local*:

	```if (test -x /emul/linux/sbin/ldconfig) then {
	      /emul/linux/sbin/ldconfig
	} fi```;``` 
Con el enlace que hizo, en el directorio /emul/linux quedará una jerarquía de directorios como la raíz de un sistema Linux (excepto por que no tiene *kernel*).

Al ejecutar un binario para Linux, OpenBSD transformará los nombres que el programa requiera suponiendo que */emul/linux* es la raíz. Si un archivo que se debe leer no es encontrado allí, OpenBSD intenta como segunda opción en la raíz real del sistema. Por ejemplo si después de instalar *redhat_base* y crear el enlace intenta:

	```/emul/linux/bin/cp /bin/bash /```
	
es decir ejecuta **cp** de Linux, el archivo */emul/linux/bin/bash* (que es dejado allí por redhat_base) será copiado a */emul/linux*. Pero si ejecuta

	```/emul/linux/bin/cp /bin/ksh /``` 
como el archivo */emul/linux/bin/ksh* no existe (*redhat_base* no lo incluye), se copiará el archivo */bin/ksh* (**ksh** de OpenBSD) en la raíz de Linux (i.e */emul/linux*).

Si desea ver su sistema como si la raíz fuera */emul/linux* (digamos para ejecutar varios programas Linux) puede emplear:

	```/emul/linux/bin/bash``` 
que iniciará un interprete de comandos compilado para Linux (así que las vías de ese interprete de comandos son las que ven todos los binarios de Linux).

Aunque la emulación no es perfecta funcionan varios programas (como el compilador **gcc**), de hecho algunos paquetes OpenBSD emplean esta capa (e.g **jdk-1.3-linux**).

Para emplear una distribución de Linux diferente basta asegurar que pueda accederse desde */emul/linux*. Por ejemplo si en la partición */dev/wd0i* tiene un sistema Debian, puede agregar la siguiente línea *a /etc/fstab* para montarla automáticamente durante el arranque:

	```/dev/wd0i /mnt/debian ext2fs rw 1 1``` 
	
Cree un enlace de */emul/linux a /mnt/debian* (creando antes el directorio */mnt/debian*) y recuerde ejecutar **ldconfig** para que las librerías compartidas pueda ser ubicadas.


[^ paquetes_y_portes.1] Para determinarlo puede ver la velocidad de cada espejo con algo como **ping ftp.pasosdeJesus.org** o emplear el **script pos.sh** que hará la prueba para los espejos oficiales de OpenBSD. Está en el directorio herram de las fuentes de este escrito.

[^ paquetes_y_portes.2] Resultados de pasar archivos por funciones de hashing.

[^ paquetes_y_portes3] El kernel genérico permite hacer el cambio de esta manera porque fue compilado con la opción de configuración COMPAT_LINUX

# faltan tablas
# El enlace http://www.openbsd.org/5.9_packages/amd64.html dice not found
# El enlace http://www.openbsd.org/ports.html dice not found