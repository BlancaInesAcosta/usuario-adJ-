#La distribución Aprendiendo de Jesús de OpenBSD como Sistema de Escritorio

Vladimir Támara Patiño (vtamara@pasosdeJesus.org)

Este documento, sus fuentes y actualizaciones se ceden al dominio público desde el 2002. Esta es la versión 5.9 de este documento, actualizada por última vez en 14/09/2016 para que corresponda a la distribución adJ de OpenBSD 5.9. No se ofrece garantía alguna, por el contrario se aprecia que se envíen correcciones y sugerencias al autor. Este escrito se dedica a Dios: 'Grandes y Maravillosas son Tus Obras'.

Expresamos agradecimiento especiales por las correcciones de Jaime Carvajal, Hernan Quishpe y los aportes de Christian Llanos (christian.sanchez@gmail.com) y Alvaron Pinzón.

La versión publicada más reciente está tanto [en línea](http://pasosdejesus.github.io/usuario_adJ/) como [en archivo comprimido]() y como [paquete para adJ/OpenBSD](). Sus fuentes en DocBook están disponibles en [https://github.com/pasosdeJesus/usuario_adJ/](https://github.com/pasosdeJesus/usuario_adJ/).

**Resumen**

Estas notas presentan algunas de las posibilidades de un computador con el sistema operativo OpenBSD como sistema de escritorio. Las hemos revisado para que concuerden con la versión 5.9 de OpenBSD.

**Tabla de contenidos**

1. Introducción
    
    1.1. Diferencias entre OpenBSD y Linux
2. Sobre la instalación
    
    
    2.1. Prerequisitos para la instalación
    
    2.2. Ayudas para la instalación
    
    2.3. Instalación y configuración del sistema básico
    
    2.4. Instalación del resto de adJ
    
    2.5. Inicio del sistema
    
    2.6. Configuración de Xorg

        2.6.1. Tipos de letra

        2.6.2. Lecturas recomendadas
3. Paquetes y portes

    3.1. Paquetes

        3.1.1. Actualización de paquetes

    3.2. Portes

        3.2.1. Busquedas de portes

        3.2.2. Compilación de portes

        3.2.3. Creación de un porte

    3.3. Emulación de binarios para Linux

4. Configuración y uso de programas y servicios básicos

    4.1. pdksh (ksh)

    4.2. ftp

    4.3. doas

        4.3.1. sudo

    4.4. cron: para programar tareas

    4.5. ld

    4.6. Kernel

        4.6.1. Compilación del kernel

    4.7. syslog

        4.7.1. Referencias y lecturas recomendadas

    4.8. Zona Horaria

5. Configuración y uso de algunas aplicaciones

    5.1. Escritorio y Archivos

        5.1.1. FluxBox

        5.1.2. xfe

    5.2. Espiritualidad

        5.2.1. xiphos

    5.3. Preparación de Documentos y Aplicaciones de Oficina

        5.3.1. Aplicaciones estilo MS-Office

        5.3.2. TeX y Ghostview

        5.3.3. DocBook

    5.4. Español

        5.4.1. ispell

    5.5. Multimedia

        5.5.1. Mplayer

        5.5.2. Edición de gráficos

        5.5.3. Edición de audio

        5.6. Operación en red

            5.6.1. mutt

            5.6.2. Chromium

        5.7. Ambiente de Desarrollo

        5.7.1. Ruby

        5.7.2. Java

6. Configuración y uso de algunos dispositivos y servicios

    6.1. Reconocimiento de hardware durante el arranque

        6.1.1. Modificación antes de arrancar

        6.1.2. Lecturas recomendadas

    6.2. Impresión

        6.2.1. Uso de una impresora ya configurada

        6.2.2. Configuración de una impresora local con lpd

        6.2.3. CUPS

        6.2.4. foomatic

    6.3. Discos duros

        6.3.1. Particiones

        6.3.2. Sistema de archivos ffs

        6.3.3. Zonas de intercambio (swap)

    6.4. Disquetes e imágenes de disquetes

        6.4.1. Programas del paquete mtools

        6.4.2. Montar disquettes en jerarquía de directorios

        6.4.3. Montar imagenes de disquettes

    6.5. Unidad de CD-ROM

        6.5.1. Montar un CD

        6.5.2. Montar imagen ISO 9660

        6.5.3. Referencias

    6.6. Quemadora de CD-R y CD-RW

        6.6.1. Crear una imagen ISO a partir de un CD existente

        6.6.2. Crear una imagen nueva ISO 9660

        6.6.3. Quemado de una imagen ISO 9660 en un CD-R o en un CD-RW

        6.6.4. Referencias

        6.7. Operaciones con DVD

        6.7.1. Referencias

    6.8. Memoria USB

    6.9. Imagen encriptada

        6.9.1. Creación de la imagen

        6.9.2. Montar imagen
        
        6.9.3. Montar en el arranque

        6.9.4. Referencias

    6.10. Teclado en español

        6.10.1. En las consolas tipo texto

        6.10.2. X-Window

7. Instalaciones duales

    7.1. Dual con Linux

        7.1.1. Arranque múltiple

        7.1.2. Montar directorios del otro sistema

    7.2. Dual con Windows XP o NT

        7.2.1. Arranque múltiple

        7.2.2. Montaje de particiones NTFS

A. Caracteres que pueden generarse

B. Herramientas

C. Novedades

Bibliografía

1. Introducción

OpenBSD es un sistema operativo tipo Unix de libre redistribución[1]. Es descendiente directo de NetBSD que a su vez desciende de los sistemas Unix desarrollados en la Universidad de Berkeley i.e. BSD[2]. Sus puntos más fuertes son estandarización (cumplir POSIX), seguridad y criptografía. Para lograr seguridad y descubrir fallas sus desarrolladores examinan detalladamente (auditan) y mejoran las fuentes de los componentes básicos del sistema operativo[3]. Este trabajo ha permitido liberar varias versiones de OpenBSD desde hace más de 10 años con tan sólo dos fallas de seguridad conocidas en los componentes básicos, en la instalación por defecto (apropiada para un servidor conectado a Internet).

1.1. Diferencias entre OpenBSD y Linux

Linux soporta más hardware y cuenta con muchas más aplicaciones, sin embargo la autodetección de hardware de OpenBSD es mejor y este sistema cuenta con capas de emulación que permiten ejecutar algunas aplicaciones para Linux, BSD/OS, SVR4, IBCS2 y FreeBSD (en i386).

OpenBSD no ha sido diseñado como sistema operativo de escritorio, sino para manejar un servidor conectado a Internet de forma extra-segura, sin embargo diversos desarrolladores han buscado mejorar su usabilidad en el escritorio, como se presenta en este escrito.

En particular la distribución Aprendiendo de Jesús busca personalizar OpenBSD como plataforma de comunicaciones y sistema de escritorio completo y usable.

Las fuentes de OpenBSD son en la humilde opinión del autor de este escrito más legibles y mejor documentadas, los mismos desarrolladores mantienen excelentes páginas man.

La licencia del kernel de OpenBSD y de la mayoría de componentes del sistema básico es BSD, aunque todo el sistema depende de una versión auditada de gcc.


[1] La mayoría de los componentes básicos de OpenBSD están cubiertos por licencias tipo BSD, que permiten copia, redistribución, modificación, inclusión en programas con licencia diferente y exigen únicamente crédito.

[2] BSD es sigla de Berkeley Software Distribution.

[3] Algunas técnicas que emplean para mejorar las fuentes de los portes se describen en http://www.openbsd.org/porting.html

# en archivos comprimidos no da una direccion pero si descarga los archivos no se como se anota
#en paquete para adJ/OpenBSD genera error 404 not found