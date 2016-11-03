## Configuración y uso de algunas aplicaciones 
{#configuracion_y_uso_de_algunas_aplicaciones}
### Escritorio y Archivos  {#escritorio_y_archivos}

#### FluxBox  {#fluxbox}

La operación básica de este liviano y estético administrador de ventanas se describe en [basico_adJ](http://pasosdejesus.github.io/usuario_adJ/bi01.html#basico_adJ), aquí describimos algunos detalles de configuración.

Para emplearlo como administrador de ventanas por defecto debe modificarse *~/.xsession*, así como en *~/.xinitrc* y agregar al final:

	if (test -x /usr/local/bin/startfluxbox) then {
		/usr/local/bin/startfluxbox
	} fi; 
Una vez en operación podrá realizar diversas configuraciones oprimiendo el botón derecho sobre el escritorio en el menú fluxbox menu. Por ejemplo podrá cambiar estilos en *System Styles* y esconder/mostrar la barra de herramientas con *Configure* -> *Toolbar* -> Visible

![FluxBox ejecutando xfig](fluxbox-xfig.png)
	      
Entre sus características:

- Es altamente configurable (recursos, menús, decoración de ventanas y estilos).

- Es muy liviano, requiere alrededor de 4MB en RAM.

- Con **Alt**+[Boton izquierdo] permite cambiar ubicación de la ventana sobre la que está el curso, y con **Alt**+[Botono derecho] el tamaño.

El menú que presenta se configura en un archivo texto con una sintaxis sencilla, puede cambiarse editando en *~/.fluxbox/menu*.

Los programas que se inician con el escritorio pueden configurarse en *~/.fluxbox/startup*. Por ejemplo en este archivo puede configurar el locale que usará agregando o cambiando la línea:

export LANG=es_CO.UTF-8
	
La apariencia en general puede configurarse en *~/.fluxbox/init*

Puede configurar teclas rápidas en el archivo *~/.fluxbox/keys*

##### Lecturas recomendadas  {#lecturas_recomendadas}

Sitio oficial [fluxbox.org](http://pasosdejesus.github.io/usuario_adJ/fluxbox.org).

Página sobre fluxbox de NetBSD [http://wiki.netbsd.org/fluxbox/](http://wiki.netbsd.org/fluxbox/)

Página sobre fluxbox de Gentoo [http://www.gentoo.org/doc/es/fluxbox-config.xml](https://wiki.gentoo.org/wiki/Fluxbox)

#### xfe  {#xfe}

La operación básica se describe en [basico_adJ](http://pasosdejesus.github.io/usuario_adJ/bi01.html#basico_adJ), aquí describimos algunos detalles de configuración.

El archivo de configuración se ubica en *~/.config/xfe/xferc*. La mayoría de posibilidades se configuran desde Editar->Preferencias, sin embargo para que opere bien Herramientas->Ventana superusuario nueva en adJ el archivo de configuración en la sección *OPTIONS* debe incluir:

```
uso_sudo=1
sudo_nopasswd=1
```

### Espiritualidad  {#espiritualidad}

#### xiphos  {#xiphos}

Como se describe en [xiphosmanual](http://pasosdejesus.github.io/usuario_adJ/bi01.html#xiphosmanual), es una herramienta gráfica de estudio e investigación bíblica que se basa en las librerías del proyecto Sword.

![Xiphos](xiphos.png)

Puede iniciarla desde el menu de fluxbox Espiritualidad->**Xiphos** o desde una terminal con xiphos. Deben instalarse módulos primero. Cada módulo puede ser una traducción, comentario, diccionario. Hay traducciones a muchos idiomas (incluyendo manuscritos griegos y hebreo). En español entre otras se encuentra parte de la traducción de dominio público de los evangelios que típicamente se ha incluido con adJ.

Una vez instale los módulos que desea usar, es posible ver varias traducciones en paralelo. Cada traducción puede contar con opciones como: Palabras de Cristo en Rojo, Números Strong, Etiquetas morfológicas, Notas al pie, encabezados.

### Preparación de Documentos y Aplicaciones de Oficina
{#preparacion_de_documentos_y_aplicaciones_de_oficina}

#### Aplicaciones estilo MS-Office 
{#aplicaciones_estilo_ms_office}

LibreOffice es un juego de aplicaciones de oficina que ofrece, prácticamente, la misma funcionalidad de MS-Office, un formato de documentos estándar (ISO) y facilidades para importar y exportar en formatos de MS-Office.

Lo puede iniciar desde el escritorio con botón derecho Oficina->LibreOffice o bien desde una terminal con

	soffice
	      
Después de instalarlo y ejecutarlo por primera vez arrancará en inglés, para configurarlo en español vaya al menú Tools->Options->Language y elija Español y Español/Colombia en las casillas de idioma que aparecen al lado derecho.

Incluye el procesador de texto *writer* (que puede iniciar desde una terminal con **swriter**), la hoja de cálculo *calc* (que inicia desde una terminal con **scalc**), el creador de presentaciones *impress* (que inicia con **simpress** desde un terminal) y el programa para diagrama *draw* (que puede iniciar con **sdraw**). Puede aprender más sobre este procesador en [libreoffice-basico](http://pasosdejesus.github.io/usuario_adJ/bi01.html#xiphosmanual)

Además el DVD de adJ incluye abiword-3.0.1p4 como procesador de texto capaz de abrir y escribir tanto en el formato de Microsoft Office como en OpenDocument (que es el formato de LibreOffice y OpenOffice).

Para operar con hojas de cálculo incluye gnumeric-1.12.26, que también puede abrir y guardar en OpenDocument y en formatos de Microsoft Office.

![Gnumeric](gnumeric.pg)

La funcionalidad de un procesador de palabra, así como la básica para hacer presentaciones también las ofrece LaTeX (ver [Sección 5.3.2.1, “LaTeX”](#latex)). Parte de la funcionalidad de una hoja de cálculo la tiene desde una terminal sc. También puede usar **magicpoint** para hacer presentaciones. Sin embargo para usar estas herramientas se requiere aprender formas diferentes de operar.

#### TeX y Ghostview  {{#tex_y_ghostview}

Para emplear TeX, LaTeX y asociados instale texlive y gv:

	doas pkg_add $PKG_PATH/texlive_base-2014p1.tgz 
	doas pkg_add $PKG_PATH/texlive_texmf-full-2014p0.tgz 
	doas pkg_add $PKG_PATH/gv-3.7.1p1.tgz 
Puede configurar tamaño del papel, separado en sílabas y otros detalles con **texconfig**.

A continuación se incluye un mini-tutorial de LaTeX adaptado de [AALinux], por otra parte puede consultar algo más sobre **gv** en la sección [Sección 6.2.1, “Uso de una impresora ya configurada”](#uso_de_una_impresora_ya_configurada).

#### LaTeX  {#latex}

LaTeX es una extensión a un sistema llamado TeX, desarrollado para escribir documentos de matemáticas. A continuación se presenta un ejemplo de un documento LaTeX y el resultado que se obtiene tras procesarlo.

```\documentclass{article}```

```\usepackage[T1]{fontenc}```

```\usepackage[spanish]{babel}```

```\begin{document}```

```\author{Rupertino Gonzales}```

```\title{Algunas posibilidades de LaTeX}```

```\maketitle```


```\section{Elementos}```

Puede estructurar el documento en capítulos, secciones, etc.
Este texto es el contenido de la primera sección de este ejemplo, 
puede escribir cada párrafo en líneas consecutivas.

```\subsection{Ayudas}```
Puede lograr efectos como *\emph*{Itálicas}, *\textbf*{negrillas} o 
cambios en el *\textsf*{tipo o {*\small* tamaño} de letra} (note 
como se anidaron ambientes en este ejemplo).

Puede crear listas:
```\begin{itemize}```

*\item* Primer elemento de lista.
*\item* Segundo elemento de lista.
\end{itemize}
o tablas
\\
\begin{tabular}{|l|r|} \hline
Título 1 & Título 2 \\\hline
elemento 1 & elemento 2 \\\hline
\end{tabular}
\subsection{Ecuaciones}
LaTeX es un experto en esta materia:
\[ \int_{x=-\infty}^{\infty}e^{-|x|} \]
\end{document}```
LaTeX ofrece plantillas para varios tipos de documentos: artículo, reporte, libro y ofrece el concepto de ambiente para indicar como presentar cierta información de acuerdo a la plantilla. En el ejemplo presentado, el tipo de documento es artículo (lo indica la línea *documentclass{article*}), y uno de los ambientes empleados es tabular, que genera una tabla.

Una vez edite un documento puede procesarlo con LaTeX para obtener un archivo DVI, por ejemplo para generar el archivo documento.dvi a partir de documento.tex:

	latex documento.tex
El archivo DVI es apropiado para imprimir, puede imprimirlo con un comando como **dvilj, dvidj** o un nombre análogo que corresponda a su impresora [^config_uso_aplicaciones.1]. Para visualizar un archivo DVI puede emplear el comando xdvi:

	xdvi documento.dvi
y para convertirlo a PostScript puede emplear **dvi2ps**:

	dvi2ps -c documento.ps documento.dvi
A continuación se presenta como se ve el ejemplo de esta sección con el programa **xdvi**.

![Visualización de DVI generado de 
		  fuente en LaTeX](ejlatex.png)
		  
Existen además otros programas para convertir de LaTeX a HTML como latex2html y HeVeA. Puede encontrar más información de latex2html en [http://ctan.tug.org/ctan/tex-archive/support/latex2html/](http://ctan.uniminuto.edu/ctan/tex-archive/support/latex2html/) y de HeVeA en [http://pauillac.inria.fr/hevea/](http://pauillac.inria.fr/hevea/).

#### DocBook  {#docbook}

Puede configurarse tanto DocBook SGML 4.4 y procesarse con las hojas de estilo DSSSL, o bien DocBook XML 4.4 y procesarse con **xsltproc**.

#### SGML 4.4 con DSSSL  {#sgml_4.4_con_dsssl}

Instale los paquetes openjade, docbook y docbook-dsssl:

	doas pkg_add $PKG_PATH/docbook-4.5p1.tgz
	doas pkg_add $PKG_PATH/docbook-dsssl-1.79.tgz
	doas pkg_add $PKG_PATH/openjade-1.3.3pre1p2.tgz 
Esto bastará para hacer conversiones de DocBook SGML a HTML por ejemplo si su hoja de estílo DSSL es "marcos.dsl" y va a convertir el documento DocBook marcos.xml:

	openjade  -t sgml -ihtml -d marcos.dsl#html marcos.xml 
Para convertir a PostScript además de los paquetes anteriores requiere el paquete texlive (ver [TeX y Ghostview](tex_y_ghostview)) y el paquete jadetex

##### XML 4.4 con XSL  {#xml_4.4_con xsl}

Cómo parte del paquete *docbook-4.5p1** se instalará el DTD de DocBook XML 4.4 en el directorio */usr/local/share/xml/docbook*. Es recomendable que cree el archivo */usr/local/share/xml/catalog* inicialmente con:

	CATALOG "docbook/catalog" 
	
y que agregue en este archivo la ruta de otros catálogos XML o de DTDs.

Para transformar documentos XML con hojas de estilo XSL puede emplear un procesador como *xsltproc*, que está incluido en el paquete *libxslt*. Esta herramienta recibe el nombre del archivo *XSL* y el nombre del archivo XML por transformar. También puede recibir la opción --*catalogs* que indica usar los catálogos de la variable *SGML_CATALOG_FILES* (se separan unos catálogos de otros con el carácter ':'), o la opción --*nonet* que indica no descargar DTDs de Internet (los catálogos deben resolver todos los identificadores públicos).

Las hojas de estilo XSL para transformar DocBook XML en HTML y en XML-FO (Formatting objects, apropiado para imprimir con un procesador FO) se instalan con el paquete *docbook-xsl-1.68.1p5* y quedan en */usr/local/share/xsl/docbook*.

Una vez instalados todas las partes puede procesar el archivo ejemplo.*xdbk*:

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE article PUBLIC "-//OASIS//DTD DocBook XML V4.1.2//EN"  "/usr/local/share/xml/docbook/4.4/docbookx.dtd" > 

<article lang="es">
	<title>articulo</title>
  <articleinfo>
    <authorgroup>
      <author>
	<firstname>Nombres</firstname>
	<surname>Apellidos</surname>
	<authorblurb>
          <para><address>
	      <email>micorreo@pasosdeJesus.org</email>
           </address></para>
	</authorblurb>
      </author> 
    </authorgroup>
    
    <abstract>
	    <para>Resumen</para>
    </abstract>

    <legalnotice>
	    <para>nota legal</para>
    </legalnotice>

  </articleinfo>

  <sect1 id="s1">
    <title>Primera sección</title>

    <para>Parrafo</para>
  </sect1>
</article>
con una hoja XSLT de DocBook con:

	export SGML_CATALOG_FILES="/usr/local/share/xml/catalog"
	xsltproc --catalogs --nonet 
	
	/usr/local/share/xsl/docbook/html/chunk.xsl ejemplo.xdbk
### Español  {#espanol}

#### ispell  {#ispell}

**ispell** permite revisar ortografía de textos planos o fuentes de documentos en diversos formatos (HTML, XML o TeX). Hay diccionarios disponibles para diversos idiomas. Al instalar el paquete *ispell* se instalarán dos diccionarios: inglés de EUA (*american*) e inglés de Inglaterra (*british*). Para emplear el diccionario en español (*spanish*) instale el paquete *ispell-spanish*, puede configurarlo como diccionario por defecto con **ispell-config**.

##### Corrección ortográfica  {#correccion_ortografica}

Puede corregir interactivamente la ortografía de un texto plano (digamos *carta.txt*) usando el diccionario por defecto con:

	ispell carta.txt
			
o si desea emplear el diccionario de un idioma particular:

	ispell -d spanish carta.txt
			
Al corregir interactivamente ispell mostrará palabras que no encuentre en el diccionario del idioma ni en su diccionario personal[config_uso_aplicaciones.2] del idioma (e.g. *~/.ispell_spanish*), así como posibles remplazos. Podrá elegir uno de los remplazos tecleando el número que antecede al remplazo.

### Multimedia  {#multimedia}

#### Mplayer  {#mplayer}

Es un reproductor de audio y video que soporta gran variedad de formatos. Por ejemplo para ver video.flv:

	mplayer video.flv
	      
Se distribuye con codecs que pueden distribuirse libremente. Para que soporte codecs de Windows es importante instalar también el paquete *win32-codecs*

También permite hacer conversiones y extraer partes. Por ejemplo para extraer pista de audio de un video descargado de youtube, y dejarla en formato WAV:

	mplayer -vo null -ao "pcm:file=pista.wav" -af resample=44100 "video.flv"
	
Para reproducir 10 segundos de un DVD comenzando en el segundo 240:

	mplayer -ss 240 -endpos 10 dvd://1
	
Se distribuye junto con **mencoder** que permite convertir de un formato a otro.

#### Edición de gráficos  {#edicion_de_graficos}

Para ver una gráfica (sin editarla) prácticamente en cualquier formato, puede usar display incluido en ImageMagick-6.9.4.10:

```display migrafica.png```
	      
Otra opción que facilita ver un directorio con imagenes es *xfi* incluido en el paquete *xfe-1.40.1p0*:

```xfi migrafica.png```
	      
Las operaciones típicas con gráficos son el retoque de fotos y la creación de diagramas. Para retocar fotografías se emplean editores gráficos a nivel de *pixels*, mientras que para creación de diagramas resultan más convenientes editores de gráficos vectoriales.

Como editor de gráficos a nivel de pixels, el CD de adJ incluye gimp-2.8.16

![Editor Gimp](gimp.png)

Como editor vectorial incluye inkscape-0.91p8

![](inkscape.png)

OpenOffice incluye otro editor vectorial llamado *draw*.

Para la generación de gráficos de barras y estadísticos resulta más apropiado el graficador de gnumeric-1.12.26 o de calc --la hoja de cálculo de OpenOffice--, ver [Sección 5.3.1, “Aplicaciones estilo MS-Office”](#aplicaciones_estilo_ms_office)

#### Edición de audio {#edicion_de_audio}

Para reproducir una pista de audio en diversos formatos (incluyendo el libre ogg, el común wav y el patentado mp3) puede usar *mplayer* (ver [Sección 5.5.1, “Mplayer”](mplayer)), o bien *audacious* (incluido en el paquete audacious-3.5.2p0):

![](audacious.png)

El programa *play* incluido en el paquete sox-14.4.2 también le permitirá escuchar diversos formatos.

Desde la línea de comandos podrá recortar, cambiar volumen y aplicar otros efectos empleando el programa *sox*.

Si prefiere un editor gráfico que le permite recortar y aplicar algunos efectos a pistas de audio en prácticamente cualquier formato de audio utilice el programa audacity-1.3.9p8:

![](audacity.png)



[^config_uso_aplicaciones.1]: Si usa **ksh** puede ver una lista de posibles programas que le permitan imprimir, tecleando dvi desde un intérprete de comandos y presionando **Tab** dos veces.

[^config_uso_aplicaciones.2]: Puede cambiar el diccionario personal que ispell debe emplear con la opción -p archivo

#  [fluxbox.org](http://pasosdejesus.github.io/usuario_adJ/fluxbox.org). dice file not found

# [http://ctan.tug.org/ctan/tex-archive/support/latex2html/](http://ctan.uniminuto.edu/ctan/tex-archive/support/latex2html/) dice not found

[https://rubymonk.com](https://rubymonk.com/) dice We're sorry, but something went wrong.

# [http://localhost:8808](http://localhost:8808/) dice No se puede acceder a este sitio web

# http://127.0.0.1:3000/ dice No se puede acceder a este sitio web

# http://127.0.0.1:3000/ dice No se puede acceder a este sitio web

# http://127.0.0.1:3000/departamentos/  dice No se puede acceder a este sitio web
 

