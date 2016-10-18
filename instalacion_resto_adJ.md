### Instalación del resto de adJ    {#instalacion_del_resto_de adJ} 

Tras instalar el sistema base y reiniciar su computador, puede ingresar a la cuenta del usuario administrador que creó y puede completar la instalación ejecutando desde una terminal:

	```/inst-adJ.sh
	```
Este procedimiento permite instalar y actualizar adJ, así que puede ejecutarlo cuantas veces lo requiera para completar la instalación o una actualización. El archivo de comandos /inst-adJ.sh lo guiará en la instalación del resto del sistema con preguntas típicamente de si o no, como se presenta en las siguientes capturas de pantalla de ejemplo:

![InsadJ.1](img/insadJ.1)

![InsadJ.2](img/insadJ.2)

![InsadJ.3](img/insadJ.3)

adJ puede configurar por defecto 2 imagenes cifradas, una para almacenar bases de datos de PostgreSQL (directorio */var/postgresql* y otra para almacenar copias de respaldo de la base de datos y otros datos que usted requiera (directorio */var/www/resbase*). Cada una de estas imagenes tienen asociadas claves para cifrar y descifrar, estas claves debe suministrarlas típicamente durante el arranque o posteriormente ejecutando:

	```/etc/init.d/montaencpos```
	```/etc/init.d/montaencres```

![InsadJ.4](img/insadJ.4)

![InsadJ.5](img/insadJ.5)

El servidor web Apache será configurado con SSL por lo que debe dar detalles para el certificado como se presenta en la siguiente captura de pantalla.

![InsadJ.6](img/insadJ.6)

![InsadJ.7](img/insadJ.7)

Después es recomendable que consulte man afterboot que incluye una lista de chequeo de cosas por hacer después de la instalación.

### Inicio del sistema  {#inicio_del_sistema} 

El arranque se realiza de acuerdo al archivo */etc/rc* (que no debe modificarse), este archivo en particular lee variables definidas por el usuario en */etc/rc.conf.local* donde puede especificar que servicios desea iniciar durante el arranque. Un servicio es un programa que se mantiene en operación mientras el computador está encendido, esperando conexiones o requerimientos de programas o de otros servicios para atenderlos, por ejemplo un servidor web es un servicio (que responde a requerimientos de páginas web en cualquier momento que un usuario las realice). */etc/rc* también iniciará los servicios típicos del sistema y algunos proveidos por paquetes y después ejecutará */etc/rc.local.*

En el archivo */etc/rc.conf.local* pueden definirse variables asociadas a servicios (puede verse el listado de variables posibles para el sistema base en */etc/rc.conf*), si el valor de una variable es NO el servicio relacionado no se activa, mientras que otro valor activa el servicio y es pasado como parámetro (si no se desean parámetros adicionales puede simplemente asignarse ""). Por ejemplo para iniciar XDM en cada arranque:

	```xdm_flags=""```
	
Los servicios proveidos por paquetes que son iniciados se especifican en la variable *pkg_scripts*, que se define en el archivo */etc/rc.conf.local*. Los servicios tanto del sistema base como de paquetes típicamente tiene un archivo en el directorio */etc/rc.d.* Estos archivos de comandos pueden ejecutarse con una de las siguientes opciones:

*start*

> Para iniciar el servicio.

*stop*

> Para detener el servicio.

*check*

> Para examinar el estado del servicio. Si retorna 0 significa que el servicio está operando, 1 de lo contrario.

*restart*

> Detiene y vuelve a iniciar el servicio.

*reload*

> Si el servicio está operando lo vuelve a iniciar.

En adJ el archivo de comandos */etc/rc.local* además de poder contener acciones por realizar en el arranque, permite reiniciar servicios que se hayan detenido. Por esto tras detener un servicio (bien intencionalmente o por error) puede ejecutar este archivo de comandos para reiniciarlo con:

	```doas sh /etc/rc.local```
	      
### Configuración de Xorg  {#configuracion_de_xorg} 

Si durante la instalación eligió usar X-Window y xdm, e instaló los juegos de instalación de X-Window (*i.e xbase59, xfont59, xserv59, xshare59*), y si su tarjeta fue correctamente configurada, al arrancar iniciará en el ambiente gráfico.

Xorg puede también leer la configuración de su tarjeta de video, monitor, teclado y ratón del archivo */etc/X11/xorg.conf*, por lo que este es el archivo que debe generar y configurar en caso de que la autodetección de Xorg no opere con su hardware.

Para generar un archivo de configuración inicial ejecute:

    ```su -
    Xorg -configure```
	
no siempre funciona, pero si en varios casos dejará un archivo de configuración en */root/xorg.conf.new* que puede probar con:

    ```su - 
    Xorg -config xorg.conf.new```
	
Si no logra ingresar al modo gráfico revise la bitácora de errores del servidor X-Window con:

  	```doas less /var/log/Xorg.0.log```
y edite el archivo de configuración, siguiendo las instrucciones de

    ```man xorg.conf```	
hasta lograr que opere, puede editar por ejemplo con:

    ```doas mg /root/xorg.conf.new```
Una vez le funciona (entra a modo gráfico del cual puede salir con Ctrl-Alt-BackSpace) copielo al sitio donde debe quedar :

    ```doas cp /root/xorg.conf.new /etc/X11/xorg.conf```
	
y termine ejecutando

    ```doas xdm```
	
Si persisten sus dificultades, puede buscar en Internet una configuración correcta o solicitar el archivo de configuración a alguien que le funcione o enviar la bitácora */var/log/Xorg.0.log* a un grupo de usuarios. Unos cambios que suelen funcionar son:

> 1. Si en la bitácora */var/log/Xorg.0.log* se recomienda, agregar

    machdep.allowaperture=2
			
> o bien

    machdep.allowaperture=1
	
en */etc/sysctl.conf*, según lo recomendado en la bitácora.

> Elegir un monitor y editar el archivo para quitar el comentario que deja en la frecuencia horizontal --es decir el símbolo # al comienzo de la línea.

> 2. Escoger vesa como controlador y tarjeta de video.

En casos extremos puede comenzar con un archivo de configuración típico como el que se presenta a continuación (para teclado en español, tarjeta de video que soporta VESA, y un monitor que no requiere especificar rangos):

Section "ServerLayout"

>	Identifier     "X.org Configured"

>	Screen      0  "Screen0" 0 0

>	InputDevice    "Mouse0" "CorePointer"

>	InputDevice    "Keyboard0" "CoreKeyboard"

EndSection

Section "Files"

> *#*	RgbPath      "/usr/X11R6/share/X11/rgb"

> *#*	ModulePath   "/usr/X11R6/lib/modules"

> 	FontPath     "/usr/X11R6/lib/X11/fonts/misc/"

> 	FontPath     "/usr/X11R6/lib/X11/fonts/TTF/"

> 	FontPath     "/usr/X11R6/lib/X11/fonts/Type1/"

> 	FontPath     "/usr/X11R6/lib/X11/fonts/CID/"

> 	FontPath     "/usr/X11R6/lib/X11/fonts/75dpi/"

> 	FontPath     "/usr/X11R6/lib/X11/fonts/100dpi/"

> 	FontPath     "/usr/local/lib/X11/fonts/local/"

> 	FontPath     "/usr/local/lib/X11/fonts/Speedo/"

> 	FontPath     "/usr/local/lib/X11/fonts/TrueType/"

> 	FontPath     "/usr/local/lib/X11/fonts/freefont/"

EndSection

Section "Module"

> 	Load  "dbe"

> 	Load  "freetype"
EndSection

Section "InputDevice"

> 	Identifier  "Keyboard0"

> 	Driver      "kbd"

> 	Option      "XkbLayout" "es"

> 	# Si el teclado fuera latinoamericano en vez de "es" usar "latam"
	
>     Option      "XkbModel" "pc105"
EndSection

Section "InputDevice"

> 	Identifier  "Mouse0"

> 	Driver      "mouse"

> 	Option      "Protocol" "wsmouse"

> 	Option      "Device" "/dev/wsmouse"

> 	# Si es serial usar protocolo "Microsoft" y dispositivo "/dev/tty00" 

> 	Option      "ZAxisMapping" "4 5"
EndSection

Section "Monitor"

> *#*       HorizSync    30.0 - 70.0

> *#*       VertRefresh  50.0 - 90.0

> *#* Puede funcionar quitar el comentario en HorizSync (vea /var/log/Xorg.0.log)

> 	Identifier   "Monitor0"

> 	Option      "DPMS"

EndSection

Section "Device"

> 	Identifier  "Card0"

> 	Driver      "vesa

> *#*	BusID       "PCI:0:1:0"

EndSection

Section "Screen"     

> 	Identifier "Screen0"

> 	Device     "Card0"

> 	Monitor    "Monitor0"

> 	DefaultDepth     16

> 	SubSection "Display"

>> 		Viewport   0 0

>> 		Depth     1

> 	EndSubSection

> 	SubSection "Display"

>>  		Viewport   0 0

>>  		Depth     4

> 	EndSubSection

> 	SubSection "Display"

>>  		Viewport   0 0

>>		Depth     8

> 	EndSubSection

> 	SubSection "Display"

>>  		Viewport   0 0

>>  		Depth     15

> 	EndSubSection

> 	SubSection "Display"

>>  		Viewport   0 0

>>  		Depth     16

>>  		Modes    "1024x768"

> 	EndSubSection

> 	SubSection "Display"
>>  		Viewport   0 0

>>  		Depth     24

> 	EndSubSection
EndSection
	
En este ejemplo mire que en la sección del monitor, la línea HorizSync está comentada (el símbolo # al comienzo de la línea) por lo que no se usa ese parámetro de configuración. De funcionar en su caso este archivo iniciara en 1024 x 768 con colores de 16 bits, porque la profundidad de color por defecto especificada es esa (*DefaultDepth* 16) y el primer modo indica esa resolución (*Modes* "1024x768").

Algunas tarjetas antiguas no son soportadas por Xorg pero si por el servidor XFree86 3.3.6, que podrá encontrar en versiones de OpenBSD anteriores a 3.7. Este último emplea normalmente el archivo de configuración */etc/XF86Config* que puede generarse con ayuda de **XF86Setup** o bien en modo texto con **xf86config3**.

Para la configuración del ratón tenga en cuenta que los ratones PS/2 y USB se usan con el protocolo wsmouse, dispositivo */dev/wsmouse*, mientas que diversos ratones seriales emplean el protocolo *Microsoft* y uno de los dispositivos seriales */dev/tty00 o /dev/tty01*.

Una vez logre configurar Xorg puede activar el administrador de vistas XDM permanentemente agregando la siguiente línea al archivo */etc/rc.conf.local* (creélo si no existe):

    ```xdm_flags=""``` 
    
#### Tipos de letra  {#tipos_de_letra}  

Los tipos de letra para X-Window se mantienen en /usr/X11R6/lib/X11/fonts en diversos formatos (por ejemplo PCF, SNF, BDF, TTF). Podrá editar diversos formatos y crear nuevos tipos de letra con el excelente editor *fontforge*.

Una secuencia típica para copiar una fuente PCF (tomada de la documentación de Bochs 2.0.2) es:

 >   cp font/vga.pcf /usr/X11R6/lib/X11/fonts/misc
    compress /usr/X11R6/lib/X11/fonts/misc/vga.pcf
    mkfontdir /usr/X11R6/lib/X11/fonts/misc
    xset fp rehash
	  
Para emplear un nuevo tipo de letra TrueType deber registrarla tanto con el servidor X como con **fontconfig** así:

> 1. Asegurar que la ruta de la fuente está en un FontPath de la sección Files de /etc/X11/xorg.conf. Por ejemplo si sus fuentes TTF están en /usr/local/lib/X11/fonts/MisTTF/:

>     FontPath     "/usr/local/lib/X11/fonts/MisTTF/"
	
> 2. Genere archivos fonts.dir y scale en el directorio donde está la fuente:

>     cd /usr/local/lib/X11/fonts/MisTTF
    /usr/X11R6/bin/mkfontscale
    /usr/X11R6/bin/mkfontdir
				  
> 3. Aplicar cambios a sesión actual

>     xset fp rehash
				  
> 4. Editar /etc/fonts/local.conf para que sea de la forma:

>  <?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "/etc/fonts/fonts.dtd">
<fontconfig>
  <dir>/usr/local/lib/X11/fonts/MisTTF</dir>
</fontconfig> 
> 5. Regenerar cache de tipos de letra, ejecutando como usuario root:

> 	cd /usr/local/lib/X11/fonts/MisTTF
	/usr/X11R6/bin/fc-cache -v
	/usr/X11R6/bin/fc-cache -v
				  
Si eventualmente el nuevo tipo no queda en los archivos generados (fonts.dir, fonts.scale) edítelos y agréguelo (el primer número en estos archivos es la cantidad de tipos por lo que debe incrementarlo).

#### Lecturas recomendadas {#lecturas_recomendadas} 

- Página man de **xorg.conf**

- Hay detalles sobre la configuración de Xorg en OpenBSD en el archivo */usr/X11R6/README*

- El servidor de X-Window incluido en OpenBSD 5.9 soporta fuentes anti-aliasing y True Type. Puede consultar el procedimiento para instalar fuentes True Type en [http://www.openbsd.org/faq/truetype.html](http://www.openbsd.org/faq/truetype.html).
ste enlace http://www.openbsd.org/faq/truetype.html dice 404 ot found
# Este enlace genera error 404 ot found http://www.openbsd.org/faq/truetype.html
# hay una lista que o  me queda bien