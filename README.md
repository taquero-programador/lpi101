# Linux Professionl

## Los sistemas operativos populares y la evolución de Linux
#### Introducción
Linux es uno de los sistemas operativos más populares; su desarrollo se inició en 1991
por Linus Torvalds. El sistema operativo se inspiró en Unix, otro sistema operativo
desarrollado en la década de 1970 por AT&T Laboratories. Unix estaba orientado a
pequeñas computadoras; en ese momento, las computadoras "pequeñas" se consideraban
máquinas que no necesitaban una sala completa con aire acondicionado y constaban menos
de un millón de dólares. Más tarde, fueron consideradas como máquinas que podían ser
transportadas por dos personas. En ese momento, Unix no estaba disponible en
computadoras pequeñas como las computadoras de oficina basadas en la plataforma x86. Por
lo tanto, Linux, que era un estudiante en ese momento, comenzó a implementar un sistema
operativo tipo Unix que se suponía que debía ejecutarse en esta plataforma.

Principalmente, Linux usa los mismos principios e ideas básicas de Unix, pero Linux es
sí no contiene código Unix, ya que es un proyecto independiente. Linux no está
respaldado por una compañia individual, sino por una comunidad internacional de
programadores. Como está disponible gratuitamente, puede ser utilizado por cualquier
persona sin restricciones.

#### Distribuciones
Una distribución de Linux es un paquete que consiste en un **kernel** de Linux y una
selección de aplicaciones mantenidas por una empresa o comunidad de usuarios. El
objetivo de una distribución es optimizar el núcleo y las aplicaciones que se ejecutan
en el sistema operativo para un determinado caso de uso grupo de usuarios. las
distribución a menudo incluyen herramientas específicas de distribución para la
instalación de software y la administración del sistema. Es por esto que algunas
distribuciones se usan princilamente para entornos de escritorio los cuales deben ser
fáciles de usar, mientras que otras se usan princilamente para ejecutarse en servidores
utilizando los recursos disponibles de la manera más eficiente posible.

Otra forma de clasificar las distribuciones es haciendo referencía a la distribución
familiar a la que pertenece. Las distribuciones de la familia Debian utilizan el gestor
de paquetes `dpkg` para gestionar el software que se ejecuta en el sistema operativo.
Los paquetes que pueden instalarse con su gestor son mantenidos por miembros voluntarios
de la comunidad. Los mantenedores utilizan el formato de paquete `deb` para especificar
cómo se instala el software en el sistema operativo y cómo está configurado de forma
predeterminada. Al igual que una distribución, un paquete es un conjunto de software
con su correspondiente configuración y documentación que facilita el proceso de
instalación, actualización y uso de software.

La distribución Debian GNU/Linux es la distribución más grande de la familia Debian.
El proyecto Debian GNU/Linux fue lanzado por Ian Murdock en 1993 y hoy día miles de
voluntarios están trabajando en el proyecto con el objetivo de proporcionar un sistema
operativo muy fiable y promover la visión de Richard Stallman de un sistema operativo
que respete las libertades del usuario para ejecutar, estudiar, distribuir y mejorar el
software. Esta es la razón por la cual no proporciona ningún software propietario por
defecto.

Ubuntu es otra distribución basada en Debian que vale la pena mensionar. Ubuntu fue
creado por Mark Shuttleworth y su equipo en 2004, con la misión de brindar un entorno
fácil de usar. La misión de Ubuntu es porporcionar software gratuito a todos en todo el
mundo, así como reducir el costo de los servicios profesionales. La distribución tiene
lanzamientos programados cada seis meses con un soporte a largo plazo cada 2 años.

Red Hat es una distribución de Linux desarrollada y mantenida por la compañia de
software con el mismo nombre, que fue adquiridad por IBM en 2019. La distribución
de Red Hat se inició en 1994 y se renombró en 2003 a Red Hat Enterprise Linux, a
menudo abreviado como RHEL. Se proporciono a las empresas como una solución empresarial
confiable que es compatible con Red Hat e incluye software que tiene como objetivo
facilitar el uso de Linux en entornos de servidores profesionales. Algunos de sus
componentes requieren suscripciones o licencias de pago. El proyecto CentOS utiliza el
código fuente disponible de Red Hat Enterprise Linux y lo compila con una distribución
que está disponible de forma totalmente gratuita, sin embargo, está distribución no
tiene soporte comercial.

Tanto RHEL como CentOS están optimizados para su uso en entornos de servidores. El
proyecto Fedora se fundó en 2003 y crea una distribución de Linux dirigida a
computadoras de escritorio. Red Hat inició y mantiene la distribución de Fedora desde
entonces. Fedora es muy progresistas y adopta nuevas tecnologías muy rápidamente y a
veces se considera un banco de pruebas para nuevas tecnologías que luego podrían
incluirse en  RHEL. Todas las distribuciones basdas en Red Hat usan el formato de
paquetes `rpm`.

La empresa SUSE fue fundada en 1992 en Alemania como un proveedore de servicios Unix.
La primera versión de SUSE Linux se lanzó en 1994. A lo largo de los años, SUSE Linux
se hizo conocido principalmente por su herramienta de configuración YaST. Esta
herramienta permite a los administradores instalar y configurar software y hardware, 
así como servicios y redes. Al igual que RHEL, SUSE lanza SUSE Enterprise Server, que
es una edición comercial. Esta distribución se publica con menos frecuencia y es
adecuada adecuada para implementaciones empresariales y de producción. Se distribuye
como un servidor, así como un entorno de escritorio con paquetes adecuados para el
propósito. En 2004, SUSE lanzó el proyecto openSUSE, que abrió oportunidades para que
los desarrolladores y usuarios probaran y desarrollaran aún más el sistema. La
distribución openSUSE está disponible gratuitamente para su descarga.

A lo largo de los años se han lanzado distribuciones independientes, algunas basadas
en Red Hat o Ubuntu, otras para mejorar una propiedad específica de un sistema o
hardware. Otras construidas con funcionalidades específicas como QuebeOS, un entorno
de escritorio muy seguro o Kali Linux, que proporciona un entorno para explotar las vulnerabiidades de software, utilizando principalmente por expertos en pruebas de
penetración, y otras superpequeñas distribuciones de Linux diseñadas para ejecutarse
de forma específica en contenedores Linux, como Docker. También hay distribuciones
creadas específicamente para componentes de sistemas embebidos e incluso dispositivos
inteligentes.

#### Sistemas embebidos
Los sistemas embebidos son combinación de hardware y software diseñados para tener una
función específica dentro de un gran sistema. Por lo general, forman parte de otros
dispositivos que ayudan a controlarlos. Los sistemas embebidos se encuentran en
aplicaciones de automoción, médicas e incluso militares. Debido a su gran variedad de
aplicaciones varios sistemas operativos basados en el kernel de Linux han sido
desarrollados para ser utilizados en sistemas embebidos. Una parte importante de los
dispositivos inteligentes tiene un sistema operativo basado en el kernel de Linux.

Por lo tanto, con los sistemas embebidos surge el software embebido. El propósito de
este software es acceder al hardware y hacerlos utilizable. Entre las principales
ventajas de Linux sobre cualquier software embebido propietario se encuentran, la
compatibilidad de plataformas entre vendedores, el desarrollo, el soporte y la ausencia
de cuotas por concepto de licencia. Dos de los proyectos de software embebido más
populares son Android, que se utiliza principalmente en télefonos móviles a través de
una variadad de proveedores, y Raspbian que se utiliza principalmente en Raspberry Pi.

#### Android
Android es un sistema operativo para dispositivos móviles desarrollado principalmente
por Google. Android Inc. fue fundado en 2003 en Palo Alto, California. La compañia
principalmente creó un sistema operativo destinado a funcionar en cámaras digitales.
en 2005, Google andquirió Android Inc. y lo desarrolló para convertise en uno de los
mayores sistemas operativos para dispositivos móviles.

La base de Android es una versión modificada del kernel de Linux con software adicional
de código abierto. El sistema operativo está desarrollado principalmente para
dispositivos con pantalla táctil, pero Google ha desarrollado versiones para
televisores y relojes de pulsera. Se han desarrollado diferentes versiones de Android
para consolas de juegos, cámaras digitales y PCs.

El código abierto de Android está disponible gratuitamente como Android Open Source
Project (AOSP). Google ofrece una serie de componentes propietarios además de núcle
de Android. Entre los componentes se incluyen aplicaciones como Google Calendar,
Google Maps, Google Mail, el navegador Chrome y Google Play Store, que facilitan la
instalación de aplicaciones. La mayoría de los usuarios considera estas herramientas
una parte integran de su experiencia con Android. Por lo tanto, casi todos los
dispositivos móviles enviados con Android a Europa y América incluyen el software
patentado de Google.

Android en dispositivos integrados tiene muchas ventajas. El sistem operativo es
intuitivo y fácil de usar con una interfaz gráfica de usuario, tiene una comunidad
de desarrolladores muy amplia, así que es fácil encontrar ayuda para el desarrollo.
También es compatible con la mayoría de de los proveedores de hardware con un
controlador Android, por lo tanto, es fácil y rentable crear un prototipo para un
sistema entero.

#### Raspbian y Raspbian Pi
Raspberry Pi es una computadora de bajo costo del tamaño de una tarjeta de crédito que
puede funcionar como un ordenador de sobremesa con todas sus funciones, pero que a su
vez puede utilizarse dentro de un sistema Linux integrado. Este ordenador fue
desarrollado por la Fundación Raspberry Pi, que es una organización benéfica educativa
con sede en el Reino Unido. Su principal porpósito es enseñar a los jovenes a aprender
a programar y comprender la funcionalidad de las computadoras. El Raspberry Pi se puede
diseñar y programar para realizar tareas u operaciones que forman parte de un sistema
mucho más complejo.

Las especialidades de Raspberry Pi incluyen un conjunto de pines de Entrada/Salida de
Propósito General (GPIO) que pueden ser utilizado para acoplar dispositivos
electrónicos y placas de extensión, lo que permite utilizar Raspbian Pi como
plataforma de hardware, ya que a pesar de que estaba pensado para fines educativos se
utiliza hoy en día en varios proyecto de DIY (hágalo usted mismo), así como para la
creación de prototipos insutriales en el desarrollo de sistemas embebidos.

Raspberry Pi utiliza procesadores ARM. Varios sistemas operativos, incluido Linux,
corren sobre Raspberry Pi. Como Raspberry Pi no contiene un disco duro, el sistema
operativo se inicia desde una tarjeta de memoria SD. Una de las distribuciones de Linux
más destacadas para Raspberry Pi es Raspbian. Como su nombre lo indica, pertenece a
la familia de distribución de Debian. Está personalizado para instalarse en el hardware
de Raspberry Pi y proporciona más de 35000 paquetes optimizados para este entorno.
Además de Raspbian, existen muchas otras distribuciones de Linux para Raspberry Pi,
como, por ejemplo, Kodi, que convierte a Raspberry Pi en un centro de medios.

#### Linux y el Cloud Computing
El término cloud computing se refiere a una forma estandarizada de consumir recursos
informáticos, ya sea comprándolos a un proveedor público de cloud computing o ejecutando
una nube privada. Según informes del 2017, Linux ejecuta el 90% de la carga de trabajo
en la nube pública. Todos los proveedores de cloud computing, desde Amazon Web Services
(AWS) hasta Google Cloud Plataform (GCP) ofrece diferentes formas de trabajar con Linux.
Incluso Microsoft, una empresa cuyo antiguo CEO comparó Linux como un cáncer, ofrece
hoy en día máquinas virtuales basadas en Linux en su nube Azure.

Linux generalmente es ofrecido como parte de Infrastucture as a Service (IaaS). Las
instancias IaaS son máquinas virtuales que se aprovisionan en cuestión de minutos en la
nube. Cuando se inicia una instancia IaaS, se elige una imagen que contiene los datos
que se desplegarán en la nueva instancia. Los proveedores de nube ofrecen varias
imágenes que contienen instalaciones listas para ejecutar tanto de las distribuciones
populares, así como sus propias versiones de Linux. El usuario podrá elegir una imagen
que contiene sus distribución preferida y acceder a una instancia en la nube que ejecute
esta distribución poco después de haberse creado. La mayoría de los proveedores agregan
herramientas a sus imágenes para ajustar la instalación a una instancia especifica en la
nube. Estas herramientas pueden, por ejemplo, extender los sistemas de archivos de la
imagen para que se ajusten al disco duro real de la máquina virtual.

#### Ejercicios
1. ¿En qué se diferencia Debian GNU/Linux de Ubuntu?

Ubuntu está basado en un snapshot de Debian, es por eso que existen muchas
características similares entre ellos. Sin embargo, existen otras diferencias
significativas entre las dos distribuciones. La primera sería la aplicabilidad para los
principiantes. Ubuntu se recomienda para principiantes por su facilidad de uso. Por otro
lado, Debian se recomienda para usuarios más avanzados. La mayor diferencia está en la
complejidad del proceso de instalación el cual Ubuntu hace más simple al usuario.

Otra diferencia pudiera estar relacionada con la estabilidad de cada distribución.
Debian se considera más estable comparada con Ubuntu. Debian recibe pocas
actualizaciones que son probadas minuciosamente por lo que el sistema, de forma general,
es más estable. Por otro lado, Ubuntu permite al usuario usar las últimas versiones
de software y nuevas tecnologías.

2. ¿Cuáles son los entornos/plataformas más comunes para los que se utiliza Linux?

Algunos de los entornos/plataformas comunes serían teléfonos inteligentes, computadoras
de escritorio y servidores. En teléfonos inteligentes, puede ser utilizado por
distribuciones como Android. En escritorio y servidores, puede utilizarse cualquier
distribución, que sea adecuada a la funcionalidad de ese equipo, desde Debian, Ubuntu
a CentOS y Red Hat Enterprise Linux.

3. Se planea instalar una distribución de Linux en un nuevo entorno. Nombre cuatro
aspectos que deban ser considerados al elegir una distribución.

El costo, el rendimiento, la escalabilidad, estabilidad y la demanda de hardware del
sistema.

4. Nombre tres dispositivos en lo que se pueda ejecutar el sistema operativo Android,
que no sean teléfonos inteligentes.

Televisores inteligentes, tablets, Android Auto y relojes inteligentes.

5. Explique tres ventajas importanes de la computación en la nube.

La flexibilidad, la fácil recuperación y el bajo costo de uso. Los servicios basados en
la nube son fáciles de implementar y escalar, dependiendo de los requisitos del negocio.
Tiene una gran ventaja en las soluciones de respaldo y recuperación, ya que permite a
las empresas recuperarse de los indicidentes más rápido y con menos repercusiones.
Además, reduce los costos de operación, ya que permite pagar solo por los recursos que
utiliza una empresa en un modelo basado en suscripción.

6. Teniendo en cuenta el costo y el rendimiento, ¿Qué distribuciones son más adecuadas
para una empresa que tiene como objetivo reducir los costos de licencias, manteniendo
el rendimiento al máximo?

Una de las distribuciones más adecuadas para ser utilizadas por empresas es CentOS. Esto
se debe a que incorpora todos los productos de Red Hat que utiliza en su sistema
operativo comercial, a la vez que es de uso gratuito. Del mismo modo, las versiones
de Ubuntu LTS garantizan la compatibilidad durante un periodo de tiempo más largo.
Las versiones estables de Debian GNU/Linux también se usan a menudo en entornos
empresariales.

7. ¿Cuáles son las principales ventajas de Raspberry Pi y qué funciones pueden tener en
los negocios?

A pesar de que el Raspberry Pi es muy pequeño, puede utilizarse como una computadora
normal. Además, es de bajo costo y puede manejar el tráfico web y muchas otras
funcionalidades. Se puede usar como un servidor, cortafuegos y se puede usar como placa
principal para robots y muchos otros dispositivos pequeños.

8. ¿Qué gama de distribuciones ofrecen Amazon Web Services y Google Cloud?

Las distribuciones comunes son Ubuntu, CentOS y Red Hat Enterprise Linux. Amazon tiene
Amazon Linux y Kali Linux, mientras que Google ofrece servidores FreeBSD y Windows.

## Principales aplicaciones de código abierto
#### Introducción
Una aplicación es un programa informático que no está directamente relacionado con el
funcionamiento de la computadora, sino con las tareas a realiza el usuario. Las
distribuciones de Linux ofrecen muchas opciones de aplicaciones para realizar una
variedad de tareas, tales como aplicaciones de oficina, navegadores web, reproductores y
editores multimedia, etc. A menudo hay más de una aplicación o herramienta para realizar
un trabajo en particular, y es el usuario quien debe elegir la aplicación que mejor se
adapte a sus necesidades.

#### Paquetes de software
Casi todas las distribuciones de Linux ofrecen aplicaciones preinstaladas. Además una distribución tiene un repositorio de paquetes con una vasta colección de aplicaciones
listas para instalar, junto a un gestor de paquetes *package manager*. Por ejemplo,
Debian, Ubuntu y Linux Mint utilizan las herramientas `dpkg`, `apt-get` y `apt` para
instalar paquetes de software, generalmente denominados paquetes *DEB*. Distribuciones
como Red Hat, Fedora y CentOS usan los comandos `rmp`, `yum` y `dnf` que a su vez
instalan paquetes *RPM*. El gestor de paquetes de la distribución elegirá los paquetes
adecuados, las dependencias necesarias y las actualizaciones futuras.

#### Ejercicios
1. Para cada uno de los siguientes comandos, indentifique si está asociado con el
sistema de empaquetado Debian o Red Hat:
Package Manager | Distribución
-- | --
`dpkg` | Debian sistema de empaquetado
`rmp` | Red Hat sistema de empaquetado
`apt-get` | Debian sistema de empaquetado
`yum` | Red Hat sistema de empaquetado
`dnf` | Red Hat sistema de empaquetado

2. ¿Qué comando se puede usar para instalar Blender en Ubuntu?

    sudo apt install blender

3. ¿Qué aplicación del paquete LibreOffice se puede utilizar para trabajar con hojas
de cálculo?

Calc

4. ¿Qué navegador web de código abierto se utiliza como base para el desarrollo de
Google Chrome?

Chromium

5. SVG es un estándar abierto para gráficos vectoriales. ¿Cuál es la aplicación más
popular para editar archivos SVG en sistemas Linux?

Inkscape

6. Para cada uno de los siguientes formatos de archivo, escriba el nombre de una
aplicación capaz de abrir y editar el archivo:
Formato | Aplicación
-- | --
`png` | Gimp
`doc` | LibreOffice Write
`xls` | LibreOffice Calc
`ppt` | LibreOffice Impress
`wav` | Audacity

7. ¿Qué paquete de software permite compartir archivos entre máquinas Linux y Windows
a través de la misma red local?

Samba

8. ¿Cómo puede eliminar automáticamente el paquete llamado `cups` y sus archivos de
configuración de un sistema basado en DEB?

    sudo apt --purge autoremove cups

9. ¿Qué paquete puede usar para convertir una imagen TIFF a JPG desde la línea de
comandos?

ImageMagick

10. ¿Con qué software puede abrir documentos de Microsoft Word enviados por un usuario
de Windows?

LibreOffice - OpenOffice

## Software de Código Abierto y licencias
#### Introducción
Aunque los términos *software libre* y *software de código abierto* ("Open Source") se
utilizan ampliamente, todavía existen algunos conceptos erróneos acerca de su
significado. En particular el concepto "libre" requiere una conceptualización mucho más
detallada.

## Destrezas TIC Y el trabajo con Linux
#### Llegando a la línea de comandos
Para nosotros, una de las aplicaciones más importantes es el emulador de termianal
gráfico. Son llamados emuladores de terminal porque realmente emulan en un entorno
gráfico. Los terminales seriales de estilo antiguo (a menudo maquinas de teletipo) en
realidad eran clientes que solían estar conectados a una máquina remota donde realmente
ocurría la informática. Esas máquinas eran computadoras realmente simples sin gráficos
que se utilizaron en la primeras versiones de Unix.

En Gnome, tal aplicación se llama *Gnome Termial*, mientras que en KDE, se conoce como
Konsole. Existen varias opciones disponibles, como *XTerm*. Estas aplicaciones permiten
que tengamos acceso a un entorno de línea de comandos para poder interactuar con la shell.

Otra forma de entrar en la terminal es utilizar la TTY virtual. Usted puede entrar
pulsando `Ctrl + Alt + F#`. Comprender que `F#` es una de las teclas de función del
1 al 7. `F7` vuelve al entorno gráfico. 

#### Usos industriales de Linux
Esta gran adopción se da no solo por la naturaleza libre de Linux, sino también por su
estabilidad, flexibilidad y rendimiento. Estas características permiten a los
proveedores ofrecer sus servicios a un coste menor mayor escalabilidad. Una parte
importante de los sistemas Linux se ejecutan hoy en día en la nube, ya sea en un modelo
IaaS (Infraestructura como servicio), PaaS (Plataforma como servicio) o SaaS (Software
como servicio).

IaaS es una forma de compartir los recursos de un gran servidor ofreciéndoles acceso
a máquinas virtuales, de hecho, son múltiples sistemas operativos que se ejecutan como
invitados en una máquina anfitriona a través de un software importante que se llama
*hipervisor*. El hipervisor es responsable de hacer posible que estos SO invitados se
ejecuten separados y adminstrando los recursos disponibles en la maquina anfitriona
para esos invitados. Esto es lo que llamamos virtualización. En el modelo IaaS, paga
solo por la fracción de recursos que usa su tuctura.

Linux tiene tres conocidos hipervisores de código abierto: *Xen*, *KVM* y *VirtualBox*.
Xen es probablemente el más antiguo de ellos, KVM ha dejado de lado a Xen como el más
prominente de los hipervisores de Linux, tiene su desarrollo patrocinado por RedHat y
es utilizado por ellos y por otros usuarios, tanto en servicios de nuble públicas como
en configuraciones de nube privadas. VirtualBox pertenece a Oracle desde su
adquisición de Sun Microsystems y suele ser utilizado por los usuarios finales debido
a su facilidad de uso y administración.

Por otro lado, PaaS y SaaS se basan en el modelo SaaS, tanto desde el punto de vista
técnico como conceptual. En PaaS, en lugar de una máquina virtual, los usuarios tiene
acceso a una plataforma en la que es posible desplegar y ejecutar una aplicación.
El objetivo de aliviar la carga de las tareas de administración del sistema y las
actualizaciones de los sistemas operativos. Heroku es un ejemplo común de PaaS en el
que el código de programa puede ejecutarse sin tener que ocuparse de los contenedores
subyacentes y las máquinas virtuales.

Por último, el SaaS es el modelo en el que normalmente se paga una suscripción para
utilizar software sin preocuparse de nada más. *Dropbox* y *Salesforce* son dos buenos
ejemplos de Saas. La mayoria de los servicios se acceden a través de un navegador web.

Un proyecto como *OpenStack* es una colección de software de código abierto que puede
hacer uso de diferentes hipervisores y otras herramientas con el fin de ofrecer un
entorno completo de IaaS en la nube aprovechando la potencia del clúster informático
en su propio centro de datos. Sin embargo, la configuración de dicha infraestructura no
es trivial.

## Aspectos básicos de la línea de comandos
Cuando se utiliza un shell interactivo, el usuario ingresa comandos en un indicador
llamado prompt. En cada distribución Linux, el indicador predeterminado podría verse
un poco diferente, pero generalmente sigue esta estructura:

    username@hostname current_directory shell_type

En Ubuntu o Debian GNU/Linux, la solicitud para un usuario normal probablemente se verá
así:

    carol@mycomputer:~$

El indicador de superusuario se verá así:

    root@mycomputer:~#

En CentOS o Red Hat Linux, el mensaje para un usuario normal se vea así:

    [dave@mycomputer ~]$

Y el indicador de superusuario sería así:

    [root@mycomputer ~]#

**username**  
Nombre del usuario que ejecuta el shell

**hostname**  
Nombre del host donde se ejecuta el shell. También hay un comando `hostname`, con el
cual puede monstrar o configurar el nombrel del host en el sistema.

**current_directory**  
Es el directorio donde se encuentra actualmente el shell. El carácter `~` significa
que la shell está en el cidrectorio princila (home) del usuario.

**shell_type**  
`$` indica que el shell lo ejecuta un usuario normal.  
`#` indica que el shell es ejecutado por el superusuario `root`.

#### Variables
Las variables son elementos de almacenamiento de datos, como texto o números. Una vez
establecida, se puede acceder al valor de una variable en un momento posterior. Las
variables tienen un nombre que permite acceder a un dato específico, incluso cuando el
contenido de la variable cambia.

#### Variables locales
Estas variables están disponibles solo para el proceso de shell actual. Si crea una
variable local y luego inicia otro programa desde la misma shell, la variable ya no estará disponible para ese programa, debido a que no son heredadas por subprocesos.

#### Variables de entorno (Environment) o globales
Estas variables están disponibles tanto en una sesión shell específica como en 
subprocesos generados a partir de esa misma sesión. Estas variables se pueden usar
para pasar datos de configuración a comandos cuando se ejecutan. Debido a que estos
porgramas pueden acceder a estas variables, se denominan variables de entorno. La
mayoría de estas variables están en mayúsculas (por ejemplo, `PATH`, `DATE`, `USER`).
Algunas variables de entorno proporcionan información como el directorio de inicio
del usuario o el tipo de terminal. En ocasiones, el conjunto completo de todas las
variables de entorno se conocen como *environmenr*.

Usar `env` o `printenv` para mostrar todas las variables de entorno.

#### La variable `PATH`
La variable `PATH` es una de las variables de entorno más importantes en un sistema
Linux. Almacena una lista de directorios separados por dos puntos que contienen
programas ejecutables elegible como comandos de la shell de Linux.
```bash
echo $PATH
# resultado
/home/user/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/ga
mes
```

Para agregar un nuevo directorio a la variable deberá usar el signo de dos puntos (:).
```bash
echo $PATH
# /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

PATH=$PATH:/home/user/bin
echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/home/user/bin
```

## Crear, mover y borrar archivos
#### Globbin
Lo que comúnmente se conoce como globbin hace referencia a un lenguaje simple de
coincidencia de patrones.

**(*)**  
Coincide con cualquier número de caracteres, incluyendo los no caracteres.

**?**  
Coincide con cualquier carácter.

**[]**  
Corresponde a una clase de caracteres.

```bash
ls star1*
ls star*
ls star2*
# lista todos los start1{más cualquier carácter}

ls star2*2
#lista cualquier archivo con star2{lo que sea}2
```

```bash
ls file2?
# por cada n cantidad de caracteres se debe colocar un ?
```

```bash
ls file[1-3]
# file1 file2 file3

# se puede especificar diferentes rangos
ls file[1-25-7]

ls file[1a5]
# file1 filea file 5

ls file[^4]
# coincide con todos menos con el 4
```

Para que coincida con una clase de caracteres, use `[:classname:]`. Por ejemplo, para
usar la clase dígitos que coincidan con los números:
```bash
ls file[[:digit:]]
# file del 1 al 9

ls file[[:digit:]a]
# conincide con file{1 al 9} o filea

ls file[[:digit:]]a
# solo coincide con file{cualquier dígito}a

# ejemplo extendido
ls log_[a-z]_201?_*_01.txt
```

**[:alnum:]**: letras y números.

**[:alpha:]**: letras mayúsculas o minúsculas.

**[:blank:]**: espacios y tabulaciones.

**[:cntrl:]**: caracteres de control, por ejemplo, backspaces, bell, NAK, escape.

**[:digit:]**: números (0123456789).

**[:graph:]**: caracteres gráficos (todos los caracteres excepto `ctrl` y el carácter de espacio).

**[:lower:]**: letras minúsculas.

**[:print:]**: caracteres imprimibles (alnum, punct y el carácter de espacio).

**[:punct:]**: caracteres de puntuación, es decir !, &, ".

**[:space:]**: caracteres de espacio en blanco, por ejemplo, tabulaciones, espacios, líneas nuevas.

**[:upper:]**: letras mayúsculas (A-Z)

**[:xdigit:]**: números hexadecimales (normalmente 0123456789abcdefABCDEF).

## Crear un script a partir de una serie de comandos
`$#` devuelve el números de argumentos, si no se pasa ningún argumento el valore es `0`.

```bash
#!/bin/bash

# se puede listar valores de una variable al estar separados por espacio

FILES="/usr/sbin/accept /usr/sbin/pwck/ usr/sbin/chroot"

for i in $FILES; do
    echo $i
done
```

```bash
#!/bin/bash
# recorrer los argumentos

if [[ $# -eq 0 ]]; then
    echo "ingresa algo"
    exit 1 # termina bien, pero queremos un código de salida
elif [[ $# -eq 1 ]]; then
    echo "$@"
    echo "$#"
else
    echo "demasiadas cosas"
    for i in $@; do
        echo $i
    done
fi
```

- Usando `echo -n` suprimirá la nueva línea después de imprimir, lo que significa que todos los ecos se imprimirán en la misma línea.
- El comando `shift` eliminará el primer elemento de un array.

#### Uso de expresiones regulares para realizar la comprobación de erroes

    echo Animal | grep "^[A-Za-z]*$"

El `^` y `$` indican el inicio y el final, el `[A-Za-z]` indica un rango de letras en
mayúsculas o minúsculas, el `*` es un cuantificador, y lo modificara de manera que haga coincidir el cero con muchas letras. Solo aceptara letras, de lo contrario fallara.

# pg 254
