# Linux 60
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
populares son Android, que se utiliza principalmente en teléfonos móviles a través de
una variadad de proveedores, y Raspbian que se utiliza principalmente en Raspberry Pi.

#### Android
Android es un sistema operativo para dispositivos móviles desarrollado principalmente
por Google. Android Inc. fue fundado en 2003 en Palo Alto, California. La compañia
principalmente creó un sistema operativo destinado a funcionar en cámaras digitales.
en 2005, Google adquirió Android Inc. y lo desarrolló para convertise en uno de los
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
herramientas a sus imágenes para ajustar la instalación a una instancia específica en la
nube. Estas herramientas pueden, por ejemplo, extender los sistemas de archivos de la
imagen para que se ajusten al disco duro real de la máquina virtual.

#### Ejercicios
1. ¿En qué se diferencia Debian GNU/Linux de Ubuntu?

    Ubuntu está basado en un snapshot de Debian, es por eso que existen muchas
    características similares entre ellos. Sin embargo, existen otras diferencias
    significativas entre las dos distribuciones. La primera sería la aplicabilidad
    para los principiantes. Ubuntu se recomienda para principiantes por su facilidad
    de uso. Por otro lado, Debian se recomienda para usuarios más avanzados. La mayor
    diferencia está en la complejidad del proceso de instalación el cual Ubuntu hace
    más simple al usuario.

    Otra diferencia pudiera estar relacionada con la estabilidad de cada distribución.
    Debian se considera más estable comparada con Ubuntu. Debian recibe pocas
    actualizaciones que son probadas minuciosamente por lo que el sistema, de forma
    general, es más estable. Por otro lado, Ubuntu permite al usuario usar las   
    últimas versiones de software y nuevas tecnologías.

2. ¿Cuáles son los entornos/plataformas más comunes para los que se utiliza Linux?

    Algunos de los entornos/plataformas comunes serían teléfonos inteligentes,
    computadoras de escritorio y servidores. En teléfonos inteligentes, puede ser
    utilizado por distribuciones como Android. En escritorio y servidores, puede
    utilizarse cualquier distribución, que sea adecuada a la funcionalidad de ese 
    equipo, desde Debian, Ubuntu a CentOS y Red Hat Enterprise Linux.

3. Se planea instalar una distribución de Linux en un nuevo entorno. Nombre cuatro
aspectos que deban ser considerados al elegir una distribución.

    El costo, el rendimiento, la escalabilidad, estabilidad y la demanda de hardware del sistema.

4. Nombre tres dispositivos en lo que se pueda ejecutar el sistema operativo Android,
que no sean teléfonos inteligentes.

    Televisores inteligentes, tablets, Android Auto y relojes inteligentes.

5. Explique tres ventajas importanes de la computación en la nube.

    La flexibilidad, la fácil recuperación y el bajo costo de uso. Los servicios 
    basados en la nube son fáciles de implementar y escalar, dependiendo de los
    requisitos del negocio. Tiene una gran ventaja en las soluciones de respaldo y
    recuperación, ya que permite a las empresas recuperarse de los indicidentes más
    rápido y con menos repercusiones. Además, reduce los costos de operación, ya que
    permite pagar solo por los recursos que utiliza una empresa en un modelo basado
    en suscripción.

6. Teniendo en cuenta el costo y el rendimiento, ¿Qué distribuciones son más adecuadas
para una empresa que tiene como objetivo reducir los costos de licencias, manteniendo
el rendimiento al máximo?

    Una de las distribuciones más adecuadas para ser utilizadas por empresas es
    CentOS. Esto se debe a que incorpora todos los productos de Red Hat que utiliza
    en su sistema operativo comercial, a la vez que es de uso gratuito. Del mismo
    modo, las versiones de Ubuntu LTS garantizan la compatibilidad durante un periodo 
    de tiempo más largo. Las versiones estables de Debian GNU/Linux también se usan a 
    menudo en entornos empresariales.

7. ¿Cuáles son las principales ventajas de Raspberry Pi y qué funciones pueden tener en
los negocios?

    A pesar de que el Raspberry Pi es muy pequeño, puede utilizarse como una
    computadora normal. Además, es de bajo costo y puede manejar el tráfico web y
    muchas otras funcionalidades. Se puede usar como un servidor, cortafuegos y se
    puede usar como placa principal para robots y muchos otros dispositivos pequeños.

8. ¿Qué gama de distribuciones ofrecen Amazon Web Services y Google Cloud?

    Las distribuciones comunes son Ubuntu, CentOS y Red Hat Enterprise Linux. Amazon
    tiene Amazon Linux y Kali Linux, mientras que Google ofrece servidores FreeBSD y
    Windows.

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
ejemplos de Saas. La mayoría de los servicios se acceden a través de un navegador web.

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

```bash
#!/bin/bash

if [[ $# -eq 0 ]]; then
    echo "Please enter at least one user to greet."
    exit 1
else
    echo -n "Hello $1"
    shift
    for username in $@; do
        echo -n ", and $username"
    done
    echo "!"
    exit 0
fi
```

- Usando `echo -n` suprimirá la nueva línea después de imprimir, lo que significa que todos los ecos se imprimirán en la misma línea.
- El comando `shift` eliminará el primer elemento de un array.

#### Uso de expresiones regulares para realizar la comprobación de erroes

    echo Animal | grep "^[A-Za-z]*$"

El `^` y `$` indican el inicio y el final, el `[A-Za-z]` indica un rango de letras en
mayúsculas o minúsculas, el `*` es un cuantificador, y lo modificara de manera que
haga coincidir el cero con muchas letras. Solo aceptara letras, de lo contrario
fallara.

```bash
#!/bin/bash

if [[ $# -eq 0 ]]; then
    echo "Please enter at least one user to greet."
    exit 1
else
    for username in $@; do
        echo $username | grep "^[A-Za-z]*$" > /dev/null
        if [[ $? -eq 1 ]]; then
            echo "ERROR: Names must only contains letters."
            exit 2
        else
            echo "Hello $username!"
        fi
        done
        exit 0
fi
```

Redirigir la salida con `/dev/null` no produce ningún resultado.

#### Ejercicios
```bash
#!/bin/bash

if [ $# -lt 1 ]; then
    echo "This script requires at least 1 argument."
    exit 1
fi

echo $1 | grep "^[A-Z]*$" > /dev/null
if [ $? -ne 0 ]; then
    echo "no cake for you!"
    exit 2
fi

echo "here's your cake!"
exit 0
```

```bash
#!/bin/bash
# $1 es un argumento que tomara como directorio
# hará un respaldo de todo lo que termine en .txt y será file.txt.bak

for filename in $1/*.txt; do
    cp $filename $filename.bak
done
```

```bash
#!/bin/bash
# toma cualquier número de argumentos y comprueba si es mayor a 10
# imprime todos sobre la misma línea con -n
# el código se puede reducir, solo es explicativo

for i in $@; do
    echo $i | grep "^[0-9]*$" > /dev/null
    if [ $? -eq 0 ]; then
        if [ $i -gt 10 ]; then
            echo -n "$i "
        fi
    fi
done
echo "" # evita que imprima un carácter o solo echo
```

## El sistema operativo Linux
#### El kernel y las distribuciones Linux
Cuando se habla de las distribuciones, el sistema operativo es Linux. Linux es el
*kernel* y el núcleo de cada distribución. El software del kernel es mantenido por un
grupo de individuos liderados por Linus Torvalds. Torvalds es empleado de un consorcio
de la industria llamado **The Linux Fundation** donde trabaja en el kernel de linux.

**Linux Kernel**: todas las distribuciones de Linux ejecutan el mismo sistema operativo, Linux.

Para saber la versión del núcleo `uname -r`.

#### Tipos de distribuciones Linux
Puede parecer una opción obvia ejecutar siempre la última versión del kernel de Linux,
pero no es tan simple como parecer. Podemos categorizar vagamente las distribuciones
Linux en tres conjunto:

- Enterprise Grade Linux Distributions
    - Red Hat Enterprise Linux
    - CentOS
    - SUSE Linux Enterprise Server
    - Debian GNU/Linux
    - Ubuntu LTS
- Consumer Grade Linux Distributions
    - Fedora
    - Ubuntu non-LTS
    - openSUSE
- Experimental and Hacker Distributions
    - Arch
    - Gentoo

#### Enterprise Grade Linux
Las distribuciones como CentOS (Community Enterprise OS) están diseñadas para
implementarse dentro de grandes organizaciones que utilizan hardware empresarial. Las
necesidades de las empresas son muy diferentes de las necesidades de las pequeñas
empresas, aficionados o usuarios domésticos. Para garantizar la diponibilidad de sus
servicios, los usuarios empresariales tienen requisitos más altos con respecto a la
estabilidad de su hardware y software. Por lo tanto, las distribuciones Linux
empresariales tienden a incluir versiones anteriores del kernel y otro software que
funcionan de manera confiable. A menudo, las distribuciones tranfieren actualizaciones
importantes, como correcciones de seguridad a estas versiones estables. En la mayoría
de los casos, las distribuciones empresariales pueden carecer de soporte para el
hardware már reciente y porporcionan versiones anteriores de paquete de software.
Sin embargo, al igual que las distribuciones Linux para consumidores(consumer), las
empresas también tienden a elegir componentes de hardware maduros y a coonstruir sus
servicios en versiones de software estable.

#### Consumer Grade Linux
Las distribuciones como Ubuntu están más dirigidas a pequeñas empresas o usuarios
domésticos y aficionados. Como tal, también es probable que estén utilizando el
hardware már reciente que se encuentra en los sistemas de consumer. Estos sistemas
necesitarán los controladores más recientes para aprovechar al máximo el nuevo
hardware, pero es poco probable que la madurez tanto de hardware como de los
controladores satisfagan las necesidades de las empresas más grandes. Sin embargo, para
el mercado de consumo, el último kernel es exactamente lo que se necesita con los
controladores más modernos, incluso si están poco probados. Los nuevos núcleos de
Linux tendrán los controladores más recientes para admitir el hardware más reciente
que probablemente esté en uso. Especialmente con el desarrollo que vemos con Linux en
el mercado de juegos, es muy importante que los últimos controladores estén
disponibles para estos usuarios.

#### Experimental and Hacker Linux Distributions
Distribuciones como Arch Linux o Gentoo Linux viven a la vanguardia de la tecnología.
Contienen las versiones más recientes de software, incluso si tiene errores y
características no probadas. A cambio, estas distribuciones tienden a usar un modelo
de lanzamiento continuo que les permite entregar actualizaciones en cualquier momento.
Estas distribuciones son utilizadas por usuarios avanzados que desean recibir siempre
el software más reciente y son conscientes de que la funcionalidad puede romperse en
cualquier momento y luego necesitan reparar sus sistemas.

#### Unix
Antes de tener Linux como sistema operativo, existía Unix. Unix solía venderse junto
con el hardware y todavía hoy en día hay varios Unix comerciales como AIX y HP-UX
disponibles en el mercado. Si bien Linux se inspiró mucho en Unix (y la falta de
disponiblidad para cierto hardware), además, la familia de sistemas operativos BSD se
basa directamente en Unix. Hoy, FreeBSD, NetBSD y OpenBSD junto con otros sistemas BSD
relacionados, están disponibles como software libre.

Unix se utilizó mucho en las empresas, pero hemos visto una disminución en la suerte
de Unix con el crecimiento de Linux. A medida que Linux ha crecido y las ofertas de
soporte también han crecido, hemos visto que Unix comienza a desaparecer lentamente.
Solaris, originalmente de Sun antes de mudarse a Oracle, ha desaparecido recientemente.
Este fue unos de los sistemas operativos Unix más grandes utilizado por las
compañias de telecomunicaciones, conocido como *Telco Grade Unix*.

Los sistemas operativos Unix incluyen:
- AIX
- FreeBSD, NetBSD y OpenBSD
- HP-UX
- Irix
- Solaris

## Conocer el hardware del ordenador
#### Tipos de procesadores

**i386**: hace referencia al conjunto de instrucciones de 32 bits asociado con el Intel
80386.

**x86**: por lo general, hace referencia a los conjuntos de instrucciones de 32 bits
asociados con los sucesores 80386, como 80486, 80586 y Pentium.

**x64/x84-64**: procesadores de referencias que admiten las instrucciones de 32 bits y
64 bits de la familia x86.

**AMD**: una referencia al soporte x86 de los procesadores AMD.

**AMD64**: una referencia al soporte x64 de los procasadores AMD.

**AMR**: hace referencia a un CPU de *Reduced Instruction Set Computer* (RISC) que no
se basa en el conjunto de instrucciones x86. Comúnmente utilizado por dispositivos
embebidos, móviles, tabletas y dispositivos con batería. Raspberry Pi utiliza una
versión de Linux para ARM.

El archivo `/proc/cpuinfo` contiene información detallada sobre el procesador de un
sistema. Lamentablemente, los detalles no son amigables para los usuarios en general.
Se puede obtener un resultado más general con el comando `lscpu`. Ejemplo:
```bash
Arquitectura:                        i686
modo(s) de operación de las CPUs:    32-bit, 64-bit
Orden de los bytes:                  Little Endian
Tamaños de las direcciones:          32 bits physical, 48 bits virtual
CPU(s):                              2
Lista de la(s) CPU(s) en línea:      0,1
Hilo(s) de procesamiento por núcleo: 2
Núcleo(s) por «socket»:              1
«Socket(s)»                          1
ID de fabricante:                    GenuineIntel
Familia de CPU:                      6
Modelo:                              28
Nombre del modelo:                   Intel(R) Atom(TM) CPU N470   @ 1.83GHz
Revisión:                            10
CPU MHz:                             1822.009
CPU MHz máx.:                        1834.0000
CPU MHz mín.:                        1000.0000
BogoMIPS:                            3657.86
Caché L1d:                           24 KiB
Caché L1i:                           32 KiB
Caché L2:                            512 KiB
Vulnerability Itlb multihit:         Not affected
Vulnerability L1tf:                  Not affected
Vulnerability Mds:                   Not affected
Vulnerability Meltdown:              Not affected
Vulnerability Mmio stale data:       Not affected
Vulnerability Retbleed:              Not affected
Vulnerability Spec store bypass:     Not affected
Vulnerability Spectre v1:            Not affected
Vulnerability Spectre v2:            Not affected
Vulnerability Srbds:                 Not affected
Vulnerability Tsx async abort:       Not affected
Indicadores:                         fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe nx lm constant_tsc arch_perfmon pebs bts cpuid aperfmperf pni dtes64 monitor ds_cpl est tm2 ssse3 cx16 xtpr pdcm movbe lahf_lm dtherm
```

**Bite size**: para la CPU, este número se relaciona tanto con el tamaño nativo de
datos que manipula, así como la cantidad de memoria a la que puede acceder. La mayoría
de los sistemas modernos son de 32 bits o 64 bits. Si un aplicación necesita acceso a
más de 4 gigabytes de memoria, debe ejecutarse en un sistema de 64 bits, ya que 4
gigabytes es la dirección máxima que se puede representar con 32 bits. Y, aunque las
aplicaciones de 32 bits generalmente se pueden ejecutar en sistemas de 64 bits, las
aplicaciones de 64 bits no se pueden ejecutar en sistemas de 32 bits.

**Clock speed**: a menudo se expresa en megahercios (MHz) o gigahercios (GHz). Esto se
relaciona con la rapidez con que un procesador ejecuta las instrucciones. Pero la
velocidad del procesador es uno de los factores que afectan los factores de respuesta,
los de espera y el rendimiento del sistema. Incluso un usuario "multi-taskin" rara
vez mantiene activa una CPU de una PC de escritorio por más del 2 o 3 porciento del
tiempo. En cualquier caso, si utiliza con frecuencia aplicaciones intensivas que
involucran actividades como el cifrado o la representación de video, la velocidad de
la CPU puede tener un impacto significativo en el rendimiento y el tiempo de espera.

**Cache**: las CPU requieren un flujo constante de instrucciones y datos para
funcionar. El costo y el consumo de energía de una memoria de sistema de varios
gigabytes a la que se puede acceder a velocidades de reloj podría ser porhibitiva.
La memoria caché del CPU está integrado en su chip para proporcionar un búfer de alta
velocidad entre las CPU y la memoria del sistema. La memoria caché se separa en varias
capas, comúnmente denominadas L1, L2, L3 e incluso L4. En el caso de la memoria caché,
entre mayor sea suele ser mejor.

**Cores**: Core o núcle se refiere a una CPU individual, además el núcleo representa
una CPU física. *Hyper-Threading Tecnology* (HTT) permite que una sola CPU física
procese simultáneamente múltiples instrucciones actuando virtualmente como múltiples
CPU físicas. Por lo general, los núcleos físicos múltiples se empaquetan como un solo
chip de procesador físico. En teoría, tener más núcleos para procesar tareas siempre
parece producir un mejor rendimiento del sistema. Desafortunadamente, las aplicaciones
de escritorio a menudo solo mantienen ocupadas las CPU 2 o 3 por ciento del tiempo,
por lo que agregar más CPU inactivas probablemente resulte en una mejora mínima del
rendimiento. Más núcleos son los más adecuados para ejecutar aplicaciones que están
escritas para tener múltiples subprocesos independientes de operación, como la
representación de cuadros de video, la representación de páginas web o entornos de
máquinas virtuales multiusuario.

## Donde los datos se almacenan
#### Dónde se almacena los archivos binarios
Al igual que cualquier otro archivo, los archivos ejecutables se encuentran en
directorios que pertenecen en última instancia de `/`. Más específicamente, los
programas se distribuyen en una estructura de tres niveles: el primer nivel (`/`)
incluye programas que pueden ser necesarios en modo de usuario único, el segundo nivel
(`/usr`) contiene la mayoría de los programas multiusuario y el tercer nivel
(`/usr/local`) se utiliza para almacenar software que no es proporcionado por la
distribución y que se ha compilado localmente.

Las ubicaciones típicas para los programas incluyen:

**`/sbin`**: contiene los archivos binarios esenciales para la administración del
sistema, como `parted` o `ip`.

**`/bin`**: contiene archivos binarios esenciales para todos los usuarios, como `ls`,
`mv` o `mkdir`.

**`/usr/sbin`**: almacena binarios para la administración del sistema, como `deluser`
o `groupadd`.

**`/usr/bin`**: incluye la mayoría de los archivos ejecutables, como `free`, `pstree`,
`sudo` o `man` que pueden ser utilizados por todos los usuarios.

**`/usr/local/sbin`**: se utiliza para almacenar programas instalados localmente para
la administración del sistema que no son gestionados por el administrador de paquetes.

**`/usr/local/bin`**: sirve para el mismo propósito que `/usr/local/sbin` pero para
programas de usuario regulares.

#### El directorio `/etc`
En los primerios días de Linux había una carpeta para cada tipo de datos, como `/bin`
para binarios y `/boot` para los núcleos. Sin embargo, `/etc` (etcétera) se creó como
un directorio general para almacenar cualquier archivo que no perteneciera a las otras
categorías. La mayoría de estos archivos eran archivos de configuración. Con el paso
del tiempo, se agregaron más y már archivos de configuración, por lo que `/etc` se
convirtió en la carpeta principal para los archivos de configuración de los programas.

En `/etc` podemos encontrar diferentes patrones para los nombres de los archivos de
configuración:

- Archivos con una extensión *ad hoc* o sin extensión, por ejemplo
    - **`group`**: base de datos de los grupos del sistema
    - **`hostname`**: nombre del equipo
    - **`hosts`**: lista de direcciones IP y sus traducciones de nombre de host.
    - **`passwd`**: base de datos del sistema: compuesta por siete campos separados por dos puntos que proporcionan información sober el usuario.
    - **`profile`**: archivo de configuración de todo el sistema para Bash.
    - **shadow**: archivo encriptado para contraseñas de usuario.
- Archivos de inicialización que terminan con `rc`:
    - **`bash.bashrc`**: archivo `.bashrc` en todo el sistema para shells interactivos.
    - **`nanorc`**: ejemplo de archivo de inicialización para GNU nano.
- Archivos que terminan en `conf`:
    - **`resolv.conf`**: Archivo de configuración para el resolvedor que proporciona acceso al sistema de nombres de dominio de Internet (DNS).
    - **`sysctl.conf`**: archivo de configuración para establecer variables del sistema para el núcleo.
- Directorios con el sufijo `.d`:

    Algunos programas con archivos de configuración único (`*.conf` o de otro modo)
    han evolucionado para tener un directorio dedicado `*.d` que ayuda a construir
    configuraciones modulares y más robustas. Por ejemplo, para configurar logrotate,
    encontrará `logrotate.conf`, pero también los directorios `logrotate.d`.

    Este enfoque es útil en aquellos casos en que diferentes aplicaciones necesitan
    configuraciones para el mismo servicio específico. Si, por ejemplo, un paquete de
    servidor web contiene una configuración de logrotate, esta configuración ahora se
    puede colocar en un archivo dedicado en el directorio `logrotate.d`. Este archivo
    puede ser actualizado por el paquete del servidor web sin interferir con la
    configuración de logrotate restante. Del mismo modo, los paquetes pueden agregar
    tareas específicas colocando archivos en el directorio `/etc/cron.d` en lugar de
    modicar `/etc/crotab`.

    En Debian y derivados de Debian, este enfoque se ha aplicado a la lista de fuentes
    confiables leídas por la herramienta de administración de paquetes `apt`: aparte
    del clásico `/etc/apt/sources.list`, ahora encontramos el directorio
    `/etc/aptsources.list.d`.

#### Archivos de configuración en `HOME` (Dotfiles)
A nievel de usuario, los programas almacenan sus configuraciones en archivos ocultos
del directorio de inicio de cada usuario (también representado `~`). Recuerde, los
archivos ocultos comienzan con un punto (`.`), de ahí su nombre: *dotfiles*.

Algunos de estos archivos de puntos son scripts de Bash que personalizan la sesión de
shell del usuario y se obtienen tan pronto como el usuario inica sesión en el sistema:

**`.bash_history`**: almacena el historial de la línea de comandos.

**`.bash_logout`**: incluye comandos para ejecutar al salir de la línea de comandos.

**`.bashrc`**: script de inicialización de Bash para shells sin inicio de sesión.

**`.profile`**: script de inicialización de Bash para shells de inicio de sesión.

Los archivos de configuración de otros programas específicos del usuario se obtienen
cuando se inician sus respectivos programas: `.gitconfi`, `.emacs.d`, `.ssh`, etc.

#### El Kernel de Linux
Antes de que se pueda ejecutar cualquier proceso, el kernel debe cargarse en una área
protegida de la memoria. Después de eso, el prceso con PID `1` pone en marcha la cadena
de procesos, es decir, un proceso inicia otro(s) y así sucesivamente. Una vez que los
procesos están activos, el kernel de Linux se encarga de asignarle recursos (teclado,
mouse, discos, memoria, interfaces de red, etc).

#### Dönde se almacenan los núcleos: `/boot`
El kernel reside en `/boot` junto con otros archivos relacinados con el arranque. La
mayoría de estos archivos incluyen los componentes del número de versión del núcleo en
sus nombres (versión de núcleo, revisión mayor, revisión menor y número de parche).

El directorio `/boot` incluye los siguientes tipos de archivos con nombres
correspondientes a la versión el kernel respectiva:

**`config-4.9.0-9-amd64`**: los ajustes de configuración para el núcleo como las
opciones y los módulos que se compilaron junto con el núcleo.

**`initrd.img-4.9.0-9-amd64`**: imagen del disco RAM inicial que ayuda al proceso de
inicio al cargar un sistemas de archivos raíz temporal en la memoria.

**`System-map-4.9.0-9-amd64`**: el archivo `System-map` contiene las direcciones de
las memoria de nombres de los símbolos del kernel. Cada vez que se reconstruya un
kernel, el contenido del archivo cambiará, ya que las ubicaciones de memoria podrían
ser diferentes. El kernel utiliza este archivo para buscar las ubicaciones de la
memoria para un símbolo de kernel en particular, o viceversa.

**`vmlinuz-4.9.0.9-amd64`**: El kernel propiamente dicho en un formato comprimido
autoextraible que ahora espacio.

**`grub`**: directorio de configuración para el gestor de arranque `grub2`.

#### El directorio `/proc`
El directorio `/proc` es uno de los llamados sistemas de archivos virtuales o psuedo,
ya que su contenido no se escribe en el disco, sino que se carga en la memoria. Se
llena dinámicamente cada vez que que la computadora se inicia y refleja constantemente
el estado actual del sistema. `/proc` incluye información sobre:

- Procesos ejecutables
- Configuración del kernel
- Hardware del sistema

Este directorio también almacena archivos con información sobre el hardware del
sistema y los ajustes de configuración del kernel. Algunos de estos archivos incluyen:

**`/proc/cpuinfo`**: almacena información sobre el CPU del sistema.

**`/proc/cmdline`**: almacena las cadenas pasadas al núcleo en el arranque.

**`/proc/modules`**: muestra la lista de módulos cargados en el kernel.

**El directorio `/proc/sys`**: este directorio incluye ajustes de la configuración
del kernel por medio de archivos y clasificados en categorías por subdirectorio. La mayoría de estos archivos actúan como un interruptor, por eso solo contienen uno de
los dos valores posibles: `0` o `1` ("on" u "off"). Por ejemplo:
```bash
cat /proc/sys/net/ipv4/ip_forward
1
```
Sin embargo, hay unas excepciones:
```bash
cat /proc/sys/kernel/pid_max
32768
```

#### Dispositivos de hardware
Recuerde, en Linux "todo es un archivo". Esto implica que la información del
dispositivo de hardware, así como los ajustes de configuración propios del núcleo se
almacenan en archivos especiales que residen en directorios virtuales.

#### El directorio `/dev`
El directorio `/dev` contiene archivos de dispositivo o nodos para todos los
dispositivos de hardware conectados. Estos archivos se utilizan como una interfaz
entre los dispositivos y los procesos que los utilizan. Cada archivo de dispositivo
se divide en una de dos categorías:

- Dispositivos de bloque (Block devices): son aquellos en los que los datos se leen y escriben en bloques que puedan abordarse individualmente. Los ejemplos incluyen discos duros (y sus paritciones, como `/dev/sda1`), unidades flash USB, CD, DVD, etc.
- Dispositivos de cáracter (Character devices): son aquellos en los que los datos se leen y escriben secuencialmente un cáracter a la vez. Los ejemplos incluyen teclados, la consola de texto (`/dev/console`), puertos seriales (como `/dev/ttySO`, etc.).

Además, `/dev` incluye algunos archivos especiales que son bastante útiles para
diferentes porpósito de programación:

**`/dev/zero`**: proporciona tantos caracteres nulos como se solicite.

**`/dev/null`**: aka *bit bucket*. Descarta toda la información que se le envía.

**`/dev/urandom`**: genera números pseudialeatorios.

#### El directorio `/sys`
El sistema de archivos *sys* está montado en `/sys`. Se introdujo con la llegada del
kernel 2.6 y significó una gran mejora en `/proc/sys`.

Los procesos deben interactuar con los dispositivos en `/dev` y por lo tanto, el
núcleo necesita un directorio que contenga información sobre estos dispositivos de
hardware. Este directorios es `/sys` y sus datos están ordenados en categorías. Por
ejemplo, para verificar la dirección MAC de su tarjeta de red (`enp0s3`) puede usar
el comando `cat` en el siguiente archivo:

    cat /sys/class/net/wlp1s0/address

#### Memoria y tipos de memoria
Básicamente, para que un programa comience a ejecutarse debe cargarse en la memoria.
En general, cuando hablamos de memeoria nos referimos a *Random Access Memory* (RAM)
y en comparación con los discos duros mecánicos tiene la ventaja de ser mucho más
rápido. En el lado negativo, es volátil (una vez que la computadora se apaga, los
datos desaparecen).

Cuando se trara de memoria podemos diferenciar dos tipos principales en un sistema
Linux:

**Memoria física**: también conocido como *RAM*, son en forma de chips formados por
circuitos integrados que contienen millones de transistores y condensadores. Estos a
su vez, forman celdas de memoria. Cada una de estas celdas tiene un código
hexadecimal asociado, una dirección de memeoria, para que pueda ser referenciada
cuando sea necesario.

**Swap**: también conocido como *swap space*, es la porción de memoria virtual que se
encuentra en el disco duro y se usa cuando no hay más RAM disponible.

Por otro lado, existe el concepto de memoria virtual, que es una abstracción de la
cantidad total de memoria utilizable (RAM y espacio en disco).

`free` analiza `/proc/meminfo` y muestra la cantidad de memoria libre y usada en el
sistema de una manera muy clara:
```bash
free
               total        used        free      shared  buff/cache   available
Mem:         2050448      423744      116888       36900     1509816     1374176
Swap:         999420        3360      996060
```

Expliquemos las diferentes columnas:

**`total`**: cantidad de memoria física y de intercambio instalada.

**`used`**: cantidad de memoria física e intercambio actualmente en uso.

**`free`**: cantidad de memoria física e intercambio que actualmente no está en uso.

**`shared`**: cantidad de memoria física utilizada principalmente por `tmpfs`.

**`buff/cache`**: cantidad de memoria física actualmente en uso por las memorias
intermedias del núcleo y la caché.

**`available`**: estimación de cuánta memoria física está disponible para nuevos proceso.

Por defecto, `free` muestra valores en kibibytes, pero permite una variedad de
interruptores muestres sus resultados en diferentes unidades de medida. Algunas de
estas opciones incluyen:

**`-b`**: Bytes.

**`-m`**: Mebibytes.

**`-g`**: Gibibytes.

**`-h`**: formato legible para humanos.

#### Procesos
Cada vez que un usuario emite un comando, se ejecuta un programa y se generan uno o
más procesos.

Los procesos existen en una jerarquía. Después de que el kernel se carga en la memoria
durante el arranque, se inicia el primer proceso que a su vez, inicia otros procesos
también. Cada proceso tiene un identificador único (`PID`) y un identificador de
proceso padre (`PPID`). Estos son números enteros positivos que se asignan en orden secuencial.

**`top`**: obtiene una lista de los procesos en ejecución:
- **`M`**: ordena por el uso de memoria
- **`N`**: ordena por el número de identificación de proceso
- **`T`**: ordena por tiempo de ejecución
- **`P`**: ordena por porcentaje de uso de CPU

**Snapshot de los procesos `ps`**: mientras que `top` proporciona información
dinámica, el comando `ps` es estática:
- **`-f`**: muestra la lista en formato completo
- **`-uf`**: muestra la relación entre padre los procesos padre e hijo
- **`-v`**: muestra el porcentaje de memoria utilizado

#### Información del proceso en el directorio `proc`
Ya hemos visto el sistema de archivos `/proc`. Este incluye un subdirectorio numerado
para cada proceso en ejecución en el sistema (el número es el PID del proceso):
```bash
ls /proc
# or
ls /proc/1/
```

#### Carga del sistema
Cada proceso puede potencialmente consumir recursos del sistema. La llamada carga del
sistema intenta agregar la carga general del sistema en un solo indicador númerico.
Puede ver la carga actual con el comando `uptime`:
```bash
uptime
 18:47:45 up  7:55,  1 user,  load average: 0.53, 0.62, 0.53
```

#### Registro del sistema y mensajería del sistema
Tan pronto como el núcleo y los procesos comienzan a ejecutarse y comunicarse entre
sí, se produce una gran cantidad de información. La mayor parte se envía a archivos:
los llamados archivos de registro o simplemente *logs*.

Sin iniciar sesión, la busqueda de un evento que sucedión en un servidor daría mucho
dolor de cabeza a los administradores de sistemas, de ahí la importancia de tener una
forma de estandarizada y centralizada de realizar un seguimiento de los eventos del
sistema. Además, los registros son determiantes y reveladores cuando se trata de
resolución de problemas y seguridad, así como fuente de datos confiables para
comprender las estadísticas del sistema y hacer predicciones de tendencias.

#### Iniciar sesión con el demonio syslog
Tradicionalmente, los mensajes del sistema han sido gestionados por la instalación
del registro estándar -syslog-o cualquiera de sus derivados -syslog-ng o rsyslog-.
El demonio de registro recopila mensajes de otros servicios o programas y los
almacena en archivos de registro, generalmente en `/var/log`. Sin embargo, algunos
servicios se encargan de sus propios registros (por ejemplo, el servidor web Apache
HTTPD). Del mismo modo, el kernel de Linux utiliza el "ring buffer" en memoria para
almacenar sus mensajes de registro.

#### Archivos de registros `/var/logd`
Debido a que los registros son datos que varían con el tiempo, normalmente se
encuentran en `/var/log`.

Si explora `/var/log` se dará cuenta de que los nombres de los registros son, hasta
cierto punto, bastante explicativos. Algunos ejemplos incluyen:
- **`/var/log/auth.log`**: almacena información sobre la autenticación
- **`/var/log/kern.log`**: almacena información del kernel
- **`/var/log/syslog`**: almacena información del sistema
- **`/var/log/messages`**: almacena datos del sistema y de algunas aplicaciones

#### Accediendo a los archivos de registro

    sudo tail -f /var/log/messages

Encontrara la salida en el siguiente formato:
- Timestamp
- Nombre del host del que proviene el mensaje
- Nombre del programa/servicio que generó el mensaje
- El PID del programa que generó el mensaje
- Descripción de la acción

La mayoría de los archivos de registro están escritos en texto plano; sin embargo,
algunos pueden contener datos binarios, como es el caso de `/var/log/wtmp` que
almacena datos relevantes para inicios de sesión exitosos.

#### Rotación de registro
Los archivos de registro pueden crecer mucho en unas pocas semanas o meses y ocupar
todo el espacio libre en disco. Para abordar esto, se utiliza la utilidad `logrotate`.
Este realiza la rotación de registros o ciclos que implicac acciones tales como mover
archivos de registro a un nuevo nombre, archivarlos y/o comprimirlos, a veces
enviándolos por correo electrónico al administrador del sistema y eventualmente
eliminándolos a medida que avanza. Las convenciones utilizadas para nombrar estos
archivos de registro son diversas (por ejemplo, agregar un sufijo con la fecha); sin
embargo, es común observar un sufijo con un número entero:
```bash
sudo ls /var/log/apache2
access.log
access.log.1
error.log
error.log.1
other_vhosts_access.log
```

#### Kernel Ring Buffer
El kernel ring buffer es una estructura de datos de tamaño fijo que registra los
mensajes de arranque del núcleo, así como cualquier mensaje en vivo del kernel. La
función de este buffer, uno muy importante, es el registrar todos los mensajes del
kernel producidos en el arranque, cuando el `syslog` no está todavía disponible.
El comando `dmesg` imprime el ring buffer del kernel (que solía estar también
almacenado en `/var/log/dmesg`). Debido a la extensión del ring buffer, este comando
se usa normalmente en combinación con la utilidad de filtrado de texto `grep` o un
paginador como `less`. Por ejemplo, para buscar mensajes de arranque:

    sudo dmesg

#### El diario del sistema: systemd-journald
A partir de 2015, systemd remplazó a SysV Init como adminstrador de servicios y
sistema de facto en la mayoría de las principales distribuciones de Linux. Como
consecuencia, el daemon journal-journald se ha convertido en los componentes de
registro estándar reemplazando s syslog en la mayoría de los aspectos. Los datos ya
no se almacenan en texto plano, sino en forma binaria. Por lo tanto, la utilidad
`journalctl` es necesaria para leer los registros. Además de eso, journald es
compatible con syslog y puede integrarse con este.

`journalctl` es la utilidad que se utiliza para leer y consultar la base de datos
(diario) de Systemd. Si es invocada sin opciones, imprime el journal completo.

    journalctl

Otras opciones incluyen:

- **`-k` o `dmesg`**: sera equivalente a usar el comando `dmesg`
- **`-b` o `--boot`**: muestra información del arranque
- **`-u`**: muestra mensajes sobre una unidad específica. Aproximadamente una unidad se puede definir como cualquier recurso manejado por systemd. Por ejemplo, `journalctl -u apache2.service` se usa para leer mensajes sobre el servicio web Apache2.
- **`-f`**: muestras los mensajes de diario más recientes y sigue imprimiendo nuevas entradas a medida que se agregan al diario, de forma muy similar a `tail -f`.

## Tu ordenador en la red
#### Redes de capa de enlace
El trabajo de cualquier paquetes es llevar información desde la fuente a su destino
a través de un enlace que conecta ambos dispositivos. Estos dispositivos necesitan una
forma de identificarse entre sí. Este es el propósito de una *dirección capa de
enlace*. En una red ethernet, se usa *Media Access Control Address* (MAC) para
identificar dispositivos individuales. Una dirección MAC consta de 48 bits. No son
necesariamente únicos a nivel mundial y no pueden utilizarse para dirigirse a pares
fuera del enlace actual. Por lo tanto, estas direcciones no se pueden usar para
enrutar paquetes a otros enlaces. El destinatario de un paquete verifica si la
dirección de destino coincide con su porpia capa de enlace y, si lo hace, procesa el
paquete. De lo contrario, el paquete se descarta.

El comando `ip link show` muestra una lista de las interfaces de red disponibles y
sus direcciones de capa de enlace, así como alguna otra información sobre el tamaño
maximo de paquetes:
```bash
ip link show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT
group default qlen 1000
link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode
DEFAULT group default qlen 1000
link/ether 00:0c:29:33:3b:25 brd ff:ff:ff:ff:ff:ff
```
El resultado anterior muestra que el dispositivo tiene dos interfaces, `lo` y `ens33`.
`lo` es el *loopback device* y tiene la dirección MAC `00:00:00:00:00:00` mientras
que `ens33` es una intefaz de Ethernet y tiene la dirección MAC `00:0c:29:33:3b:25`.

#### Direcciones IPv4
Agregar manualmente una interfaz usando el comando `ip addr add`:

    sudo ip addr add 192.168.0.100/255.255.255.0 dev ens33

El comando `ip soute show` enumera la tabla de enrutamiento de IPv4 actual:

    ip route show

Para agregar una ruta predeterminada, todo lo que se ncesesita es la dirección
interna del enrutador que será la puerta de enlace predeterminada. Si por ejemplo,
el enrutador tiene la dirección `192.168.0.1`, entonces el siguiente comando lo
configura como una ruta predeterminada:

    sudo ip route add default via 192.168.0.1

#### Direcciones IPv6
Método para agregar manualmente una dirección IPv6 a una interfaz:

    sudo ip addr add 2001:db8:0:abcd:0:0:0:7334/64 dev ens33

#### DNS
Las direcciones IP son dificiles de recordar y no tiene exactamente un alto factor
de frialdad si está tratando de comercializar un servicio o producto. Aquí es donde
entra en juego el *Domain Name System*. Es una forma más simple, DNS es una guía
telefónica distribuida que asigna nombres de dominio fáciles de recordar como
`example.com` a direccion IP. Cuando, por ejemplo, un usuario navega a un sitio web,
ingresa el nombre de host DNS como parte de la URL. El navegador web luego envía el
nombre DNS a resolver DNS que se haya configurado. Este resolver a su vez encontrará
la dirección que se correlaciona con el dominio. Luego, el resolver responde con esa
dirección y el navegador web intenta llegar al servidor web con esa dirección IP.

Los resolvers que Linux usa para buscar datos DNS se configuran en el archivo de
configuración `/etc/resolv.confi`:
```bash
cat /etc/resolv.conf
# Generated by NetworkManager
nameserver 1.1.1.1
nameserver 1.0.0.1
```
Cuando el resolver realiza una búsqueda de nombre, primero verifica el archivo
`/etc/hosts` para ver si contiene una dirección para el nombre solicitado. Si lo hace,
devuelve esa dirección y no se pone en contacto con el DNS. Esto permite a los
administradores de red proporcionar resolución de nombres sin tener que pasar por el
esfuerzo de configurar un servidor DNS completo. Cada línea en ese archivo contiene
una dirección IP seguida de uno o más nombres:
```bash
cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	debian
127.0.0.1	localapi.local
127.0.0.1	virtual.local


# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

Para realizar una busqueda en el DNS use el comando `host`:

    host debian.org

Se puede recuperar información más detallada utilizando el comando `dig`:

    dig debian.org

#### Sockets
Un *socket* es un punto final de comunicación para dos programas que se comunican
entre sí. Si el socket está conectado a una red, los programas pueden ejecutarse en
diferentes dispositivos, como un navegador web que se ejecuta en la computadora
portátil de un usuario y un servidor web que se ejecuta en el centro de datos de una
empresa.

Hay tres tipos principales de sockets:
- **Unix Sockets**: son procesos de conexión que se ejecutan en el mismo dipositivo.
- **UDP (User Datagram Protocol) Sockets**: se conectan aplicaciones usando un protocolo que es rápido pero no es resistente.
- **TPC (Transmission Control Protocol) Sockets**: son más confiables que los sockets UDP y confirman la recepción de datos.

Los sockets Unix solo pueden conectar aplicaciones que se ejcutan en el mismo
dispositivo. Sin embargo, los sockets TCP y UDP pueden conectarse a través de una red.
TCP permite una secuencia de datos que siempre llega en el orden exacto en que se
envió. UDP es más fugaz; el paquete se envía, pero no se garantiza su entrega en el
otro extremo. Sin embargo, UDP carece de la sobrecarga de TCP, por lo que es perfecto
para aplicaciones de baja latencia, como los videojuegos en línea.

TPC y UDP usan puertos para abordar múltiples sockets en la misma dirección IP. Si
bien el puerto de origen para une conexión suele ser aleatorio, los puertos de
destino suelen estar estandarizados para un servicio específico. Por ejemplo HTTP
generalmente está alojado en el puerto 80, HTTPS se ejecutan en el puerto 443. SSH
un protocolo para iniciar sesión de forma segura en un sistema Linux remoto, escucha
en el puerto 22.

El comando `ss` le permite a un administrador investigar todos los sockets en una
computadora Linux. Muestra todo, desde la dirección de origen, la dirección de
destino y el tipo. Una de sus mejores características es el uso de filtros para que un
usuario pueda monitorear los sockets en cualquier estado de conexión que desee. El
comando `ss` puede ejecutarse con un conjunto de opciones, así como una expresión
de filtro para limitar la información que se muestra.

Cuando se ejecuta sin ninguna opción, el comando muestra una lista de todos los
sockets establecidos. El uso de la opción `-p` incluye información sobre el proceso
de cada socket. La opción `-s` muestra un resumen de los sockets. Hay más opciones
disponibles para esta herramienta, pero el último conjunto de las principales son
`-4` y `-6` para reducir el protocolo IP a UPv4 o IPv6 respectivamente, `-t` y `-u`
permiten al administrador seleccionar sockets TCP o UDP y `-l` para mostrar solo los
sockets que escuchan nuevas conexiones.

Enumerar todos los sockets TCP actualmente en uso:

    ss -t

## Seguridad y permisos de usuario
#### Identificadores (UIDs/GIDs)
Los *User* y *Group Identifiers* (UIDs/GIDs) son las referencias básicas y enumeradas
a las cuentas. Las primeras implementaciones eran enteros limitados a 16 bits (valores
de 0 a 65535), pero los sistemas del siglo XIX admiten UIDs y GIDs de 64 bits. Los
usuarios y grupos se enumeran de forma independiente, por lo que el mismo ID puede
representar tanto a usuario como a un grupo.

Cada usuario no tiene solo un UID, sino también un GID primario. EL GID primario para
un usuario puede ser exclusivo de solo ese usuario y puede terminar sin ser utilizado
por ningún otro. Sin embargo, este grupo también podría ser un grupo compartido por
numerosos usuarios. Además de estos grupos principales, cada usuario tambien puede ser
miembro de otros grupos.

Por defecto en los sistemas Linux, cada usuario está asignado a un grupo con el mismo
nombre de usuario y el mismo GID que su UID. Por ejemplo, si crear un nuevo usuario
`newuser`, por defecto, su grupo predeterminado también será `newuser`.

#### La cuenta de superusuario
En Linux, la cuenta de superusuario tiene el GID `0` y también se denomina `root`. El
directorio de inicio para el superusuario es un directorio dedicado de nivel superior,
`/root`, al que solo puede acceder el usuario `root`.

#### Cuentas de usuario estándar (User Accounts)
Todas las cuentas que no sean `root` son cuentas de usuario técnicamente regulares,
pero en un sistema Linux el término colequial *user accounts* a menudo significa una
cuenta de usuario "regular" (sin privilegios). Por lo general, tiene las siguientes
propiedades, con algunas excepciones.

- EL UID comienza con 1000 (4 dígitos), aunque en algunos sistemas heredados pueden comenzar en 500.
- Posee un definido directorio de inicio, generalmente es un subdirectorio de `/home`, dependiendo de la configuración local del sitio.
- Posee un shell definido para el inicio de sesión. En Linux, el shell predeterminado sueles ser *Bourne Again Shell* (`/bin/sh`), aunque puede haber otros disponibles.

Si una cuenta de usuario no tiene un shell válido en sus atributos, el usuario no podrá abrir un shell interactivo. Usualmente `/sbin/nologin` se usa para un shell
inválido. Esto pueden tener un propósito, solo si el usuario autenticará para otros
servicios que no sea la consola o el acceso SSH, por ejemplo, solo para el acceso
Secure FTP (sftp).

#### Cuentas del sistema (System Accounts)
Las cuentas del sistema (System Access) normalmente se crean en el momento de la
instalación del sistema. Estos son para instalaciones, programas y servicios que no
se ejecutarán como superusuario. En un mundo ideal, todas estas serían instalaciones
del sistema operativo.

Las cuentas del sistema varían, pero sus atributos incluyen:
- Los UID suelen ser inferiores a 100 (2 dígitos) o 500-1000 (3 dígitos).
- Ya sea o no que posean un directorio de inicio dedicado o un directorio que generalmente noes un subdirectorio de `/home`.
- Sin shell de inicio de sesión válido (normalmente `/sbin/nologin`), con raras excepciones.

En Linux la mayoría de las cuentas del sistema iniciarán sesión y tampoco necesitan
un shell en sus atributos. Muchos procesos de propiedad y ejecutados por las cuentas
del sistema se bifurcan en su propio entorno por la administración del sistema,
ejecutándose con la cuenta del sistema específica. Estas cuentas generalmente tienen
privilegios limitados o no tienen privilegios (la mayoría de las veces).

En general, las cuentas del sistema no deben tener un válido shell de inicio de
sesión. Lo contrario sería un problema de seguridad en la mayoría de los casos.

#### Cuentas de servicio (Service Accounts)
Las cuentas de servicio generalmente se crean cuando los servicios se instalan y
configuran. Al igual que las cuentas del sistema, son para instalaciones, programas y
servicios que no se ejecutarán como superusuario.

Las cuentas de sistema y servicio son similares y se intercambian a menudo. Esto
incluye la ubicación de los directorios de inicio que normalmente están fuera de
`/home`, si se define en todas (las cuentas de servicio a menudo tienen más
probabilidad de tener una ubicación, en comparación las cuentas del sistema), además
no hay un shell válido de inicio de sesión. Aunque no existe una definición estricta,
la diferencia principal entre las cuentas de sistema y servicio se desglosa en
UID/GID.

**Cuentas del sistema**: UID/GID <100 (2- dígitos) o <500-1000 (3- dígitos).

**Cuentas de servicio**: UID/GID >1000 (4+ dígitos), pero no una cuenta de usuario
"estándar" o "regular", una cuenta para servicios con un UID/GID > 1000 (4+ dígitos).

Algunas distribuciones de Linux todavía tienen cuentas de servicio previamente
reservadas con un UID <100, y también podrían considerarse una cuenta del sistema,
aunque no se creen en la instalación del sistema operativo. Por ejemplo, en las
distribuciones Linux basadas en Fedora (incluido Red Hat), el usuario Apache del
servidor web tiene un UID (y GID) 48. Claramente es una cuenta del sistema, a pesar
de tener un directorio de inicio de sesión (generalmente en `/usr/share/httpd` o
`/var/www/html/`)>.

>Las cuentas del sistema son UID <1000, y las cuentas de usuario normales son UID (>)1000. Como las cuentas de usuario normales son >1000, estos UID también pueden incluir cuentas de servicio.

#### Shells de inicio de sesión y directorios de inicio
Algunas cuentas tienen un shell de inicio de sesión, mientras que otras no, ya que no
requieren acceso interactivo y por temas de seguridad. En lamayoría de las
distribuciones de Linux, el shell predeterminado de inicio de sesión es *Bourne Again
Shell* o `bash`, pero puede haber otros shells disponibles, como C Shell (`csh`),
Korn shell (`ksh`) o Z shell (`zsh`), entre otros.

Un usuario pueden cambiar su shell de inicio de sesión utilizando el comando `chsh`.
Por defecto, el comando se ejecuta en modo interactivo y muestra un mensaje
preguntado qué shell debe usar. La respuesta debería ser la ruta completa del binario
de shell:
```bash
chsh
Changing the login shell for emma
Enter the new value, or press ENTER for the default
Login Shell [/bin/bash]: /usr/bin/zsh
```

También puede ejecutar el comando en modo no interactivo, con el parámetro `-s`
seguida de la ruta del binario:

    chsh -s /usr/bin/zsh

La mayoría de las cuentas tienen definido un directorio de inicio. En Linux, este
suele ser la única ubicación donde esa cuenta de usuario tiene acceso garantizado de
escritura, con algunas excepciones (por ejemplo, áreas del sistema de archivos
temporales). Sin embargo, por razones de seguridad, algunas cuentas se configuran a
propósito para que no tengan acceso de escritura ni siquiera a su propio directorio
de inicio.

#### Obtenga información sobre sus usuarios
Enumerar la información básica de los usuarios es una práctica muy común y cotidiana
en un sistema Linux. En algunos casos, se deberá cambiar de usuario y aumentar los
privilegios para completar algunas tareas.

Incluso los usuarios tienen la capacidad de enumerar atributos y acceder desde la
línea de comandos.

Listar la información actual de un usuario en la línea de comandos es tan simple con
`id`. La salida variará según su ID de inicio de sesión:
```bash
id
uid=1024(emma) gid=1024(emma)
1024(emma),20(games),groups=10240(netusers),20480(netadmin)
context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
```

En la lista anterior, el usuario (`emma`) tiene identificadores que se desglosan de la
siguiente manera:
- `1024` = ID de usuario (UID), seguido del nombre de usuario
- `1024` = ID de grupo primario (GID), seguido del nombre de grupo
- Una lista de GID adicionales (nombres de grupo) a los que también pertenece el usuario.

Para enumerar la última vez que los usuarios han iniciado sesión es `last`:
```bash
last
emma pts/3 ::1
Fri Jun 14 04:28
still logged in
reboot system boot 5.0.17-300.fc30. Fri Jun 14 04:03 reboot system boot 5.0.17-300.fc30. Wed Jun 5 14:32 - 15:19 (00:46)
reboot system boot 5.0.17-300.fc30. Sat May 25 18:27 - 19:11 (00:43)
reboot system boot 5.0.16-100.fc28. Sat May 25 16:44 - 17:06 (00:21)
reboot system boot 5.0.9-100.fc28.x Sun May 12 14:32 - 14:46 (00:14)
root tty2
```

La información que figura en las columnas puede variar, pero algunas entradas notables
en el listado anterior son:
- Un usuario `emma` inicio sesión a través de la red (pseudo TTY `pts/3`) y todavía está conectado.
- Se enumera la hora del arranque actual, junto con el kernel. En el ejemplo anterior, unos 25 minutos antes de que el usuario inicie sesión.
- El superusuario `root` inicio sesión a través de una consola virtual (TTY `tty2`), a mediados de mayo.

Una variante de `last` es el comando `lastb`, que enumera todos los últimos intentos
de inicio de sesión incorrectos.

Los comandos `who` y `w` enumeran solo los inicios de sesión activos en el sistema.

El comando `w` muestra más información, incluida la siguiente:
- La hora actual y cuánto tiempo ha estado funcionando el sistema
- ¿Cuántos usuarios están conectados?
- Los promedios de carga (load averages) de los últimos 1, 5 y 15 minutos

Y la información adicional para cada sesión de un usuario activo.
- Seleccionar, tiempos totales de utilización de la CPU (`IDLE, JCPU` y `PCPU`).
- El proceso actual (`-bash`). El tiempo total de utilización de la CPU de ese proceso es el último elemento (`PCPU`).

#### Cambio de usuario y aumento de privilegios
En un mundo ideal, los usuarios nunca necesitarían escalar privilegios para completar
sus tareas. El sistema "simplemente funcionaría" y todo estaría configurado para
varios accesos.

En la mayoría de los sistemas Linux actuales, el comando `su` solo se usa para escalar
privilegios a `root`, que es el usuario predeterminado si no se específica un nombre
de usuario después del nombre del comando. Si bien se puede para cambiar a otro
usuario, no es una buena práctica: los usuarios deben iniciar sesión desde otro
sistema, a través de red, consola física o terminal en el sistema.

    su -

Después de ingresar la contraseña se superusuario (`root`), el usuario tiene un shell
de superusuario.

El símbolo de dólar (`$`) indica en la línea de comandos un shell de usuario no
privilegiado, mientras que el símbolo de libra (`#`) indica en la línea de comandos
un shell de superusuario (`root`). Se recomienda encarecidamente que el carácter final
de cualquier aviso nunca se cambie de este estándar de "comprensión universal", ya que
esta nomeclatura se utiliza en materiales de aprendizaje.

#### Archivos de control de acceso
Casi todos los sistemas operativos tienen un conjunto de ubicaciones utilizados para
almacenar controles de acceso. En Linux, estos son típicamente archivos de texto
ubicados en el directorio `/etc`, que es donde deben almacenarse los archivos de
configuración del sistema. Por defecto, todos los usuarios del sistema pueden leer
este directorio, pero solo `root` puede escribirlo.

Los archivos principales relacionados con cuentas de usuarios, atributos y control
de acceso son:

**`/etc/passwd`**: este archivo almacena información básica sobre los usuarios en el
sistema, incluyendo UID y GID, directorio de inicio, tipo de shell, etc. A pesar del
nombre, aquí no se almacenan contraseñas.

**`/etc/group`**: este archivo almacena información básica sobre todos los grupos de
usuarios en el sistema, como el nombre del grupo, GID y sus miembros.

**`etc/shadow`**: aquí es donde se almacenan las contraseñas de los usuarios. Por
seguridad son hash.

**`/etc/gshadow`**: este archivo almacena información más detallada sobre los grupos,
incluida una contraseña cifrada que permite a los usuarios convertirse temporalmente
en un miembro del grupo. En una lista de usuarios puede convertirse en un miembro
de inclusive una lista de administradores.

También hay archivos relacionados con la escalada de privilegios básicos en sistemas
Linux, como en los comandos `su` y `sudo`. Por defecto, estos solo son accesibles por
el usuario `root`.

**`/etc/sudoers`**: este archivo controla quién y cómo usar el comando `sudo`.

**`/etc/sudoers.d`**: este directorio puede contener archivos que complementan la
configuración del archivo `sudoers`.

> Aunque `/etc/sudoers` es un archivo de texto, nunca debe editarse directamente. Si necesita cambiar su contenido, debe de usar `visudo`.

#### `/etc/passwd`
El archivo `/etc/passwd` se conoce comúnmente como el "archivo de contraseñas". Cada
línea contiene múltiples campos delimitados por dos puntos (`:`). A pesar del nombre,
actualmente el hash real de la contraseña no se almacena en este archivo.

La sintaxis típica de una línea de este archivo es la siguiente:

    USERNAME:PASSWORD:UID:GID:GECOS:HOMEDIR:SHELL

**`USERNAME`**: el nombre de usuario conocido como login (nombre), como `root`,
`nobody`, `emma`.

**`PASSWORD`**: ubicación heredada del hash de contraseña. Casi siempre `x`, lo que
indica que la contraseña está almacenada en el archivo `/etc/shadow`.

**`UID`**: el ID del usuario (UID), como `0`, `99`, `1024`.

**`GID`**: el ID del grupo (GID), como `0`, `99`, `1024`.

**`GECOS`**: una lista CSV de información del usuario que incluye nombre, ubicación,
número de teléfono. Por ejemplo: `Emma Smith`, `42 Douglas St`, `555.555.555`.

**`HOMEDIR`**: ruta del directorio de inicio del usuario, como `/root`, `/home/emma`.

**`SHELL`**: el shell predeterminado para este usuario, como `/bin/bash` o `/bin/zsh`.

Por ejemplo, en la siguiente línea describe al usuario `emma`:

    emma:x:1000:1000:Emma Smith,42 Douglas St,555.555.5555:/home/emma:/bin/bash

#### Comprendiendo el campo `GECOS`
El campo GECOS contiene tres o más campos delimitados por una coma, también conocida
como una lista de valores separados por coma (CSV). Aunque no existe un estándar
obligatorio, los campos suelen estar en el siguiente orden:

    NAME,LOCATION,CONTACT

**`NAME`**: es el "nombre completo" del usuario o "nombre del software" en el caso de
una cuenta de servicio.

**`LOCATION`**: suele ser la ubicación física del usuario dentro de un edificio,
número de habitación, el departamento de contacto o persona en el caso de una cuenta
de servicio.

**`CONTACT`**: enumera información de contacto, como el número de teléfono del hogar
o del trabajo.

Los campos adicionales pueden incluir información de contacto adicional, como un
número de casa o dirección de correo electrónico. Para cambiar la información en el
campos GECOS, use el comando `chfn` y responda las preguntas. Si no se proporciona un
nombre de usuario después del nombre de comando, cambiará la información para el
usuario actual:

    chnf

#### `/etc/group`
El archivo `/etc/group` contiene siempre campos delimitados por dos puntos (`:`) que
almacenan información básica sobre los grupos en el sistema. A veces se le llama
"archivo de grupo". La sintaxis para cada línea es:

    NAME:PASSWORD:GID:MEMBERS

**`NAME`**: es el nombre del grupo, como `root`, `users`, `emma`, etc.

**`PASSWORD`**: ubicación heredada de un hash de contraseña de un grupo opcional. Casi
siempre `x`, lo que indica que la contraseña y se almacena en `/etc/gshadow`.

**`GID`**: el ID del grupo (GID), como `0`, `99`, `1024`.

**`MEMBERS`**: una lista de los nombres de usuario separados por comas que son
miembros del grupo, como `jsmith`, `emma`.

El siguiente ejemplo muestra una línea que contiene información sobre el grupo
`students`:

    students:x:1023:jsmith,emma

#### `/etc/shadow`
La siguiente tabla enumera los atributos almacenados en el archivo `/etc/shadow`,
comúnmente conocido como el "shadow file". El archivo contiene campos siempre
delimitados por dos puntos (`:`).

Las sintaxis básica para una línea en este archivo es:

    USERNAME:PASSWORD:LASTCHANGE:MINAGE:MAXAGE:WARN:INACTIVE:EXPDATE

**`USERNAME`**: el nombre del usuario, como `root`, `nobody`, etc.

**`PASSWORD`**: un hash unidireccional de la contraseña, precedido de un "salt". Por
ejemplo, `!!`, `!$1$01234567$ABC... , $6$012345789ABCDEF$012...`.

**`LASTCHANGE`**: fecha del último cambio de contraseña den días de la "epoca", como
`17909`.

**`MINAGE`**: minimo de días antes que al usuario se le permita cambiar la contraseña.

**`MAXAGE`**: máximo de días antes que al usuario se le permita cambiar la contraseña.

**`WARN`**: período de advertencia (en días) antes de que caduque la contraseña.

**`INACTIVE`**: antigüedad máxima (en días) de la contraseña después del vencimento.

**`EXPDATE`**: fecha de caducidad (en días) de la contraseña desde la "epoca".

Ejemplo de una línea en el archivo `/etc/shadow`:

    emma:$6$nP532JDDogQYZF8I$bjFNh9eT1xpb9/n6pmjlIwgu7hGjH/eytSdttbmVv0MlyTMFgBIXESFNUm
To9EGxxH1OT1HGQzR0so4n1npbE0:18064:0:99999:7::

La "epoca" en un sistema POSIX es la medianoche (0000), hora universal coordinada
(UTC), el jueves 1 de enero de 1970. La mayoría de las fechas y horas POSIX están
en segundos desde "epoca" o en caso del archivo `/etc/shadow`, en días desde la epoca.

Aunque existen diferentes soluciones de autenticación, el método elemental de
almacenamiento de contraseñas es la función hash unidireccional. Esto se hace para que
la contraseña nunca se almacene en texto sin cifrar en un sistema, ya que la función
de hash no es reversible. Puede convertir una contraseña en un hash, pero
(idealmente) no es posible volver a convertir un hash en una contraseña.

Como máximo, se requiere un método de fuerza bruta para trocear todas las
combinaciones de una contraseña, hasta que coincida. Para mitigar el problema de un
hash de contraseña sea decifrado en un sistema, los sistemas Linux usan un "salt"
aleatorio en cada hash de contraseña para un usuario. Por lo tanto, el hash para una
contraseña de usuario en un sistemas Linux generalmente no será el mismo que en otro
sistema Linux, incluso si la contraseña es la misma.

En el archivo `/etc/shadow`, la contraseña puede tomar varias formas. Estos
formularios generalmente incluyen los siguiente:

**`!!`**: esto significa una cuenta "deshabilitada" (sin posible autenticación) y sin
una contraseña hash almacenada.

**`!$1$01234567$ABC...`**: una cuenta "deshabilitada" (debido al signo de
exclamación inicial), con una función hash anterior, hash salt y cadena hash
almacenada.

**`$1$0123456789ABC$012...`**: cuenta "habilitada", con una función hash, hash salt y
cadena de hash almacenados.

La función hash. hash salt y la cadena hash están precedidas y delimitadas por un
símbolo de dólar (`$`). La longitud del hash salt debe ser entre ocho y dieciséis
caracteres. Ejemplos de los tres más comúnes:

**`$1$01234567$ABC...`**: una función hash de MD5 (1), con un ejemplo de longitud de
hash de ocho.

**`$5$01234567ABCD$012...`**: una función hash de SHA256 (5), con un ejemplo de
longitud hash de doce.

**`$6$01234567ABCD$012...`**: una función hash de SHA512 (6), con un ejemplo de
longitud de hash de doce.

## Crear usuarios y grupos
Administrar usuarios y grupos en un equipo con Linux es uno de los aspectos clave de
la administración del sistema. De hecho, Linux es un sistema operativo multiusuario en
el que varios de estos pueden usar la máquina al mismo tiempo.

La información sobre usuarios y grupos se almacena en cuatro archivos dentro del árbol
del directorio `/etc/`:

**`etc/passwd`**: es un archivo se siete campos delimitados por dos puntos que
contienen información básica sobre los usuarios.

**`/etc/group`**: es un archivo de cuatro campos delimitados por dos puntos que
contiene información básica sobre grupos.

**`/etc/shadow`**: es un archivo de nueve campos delimitados por dos puntos que
contiene contraseñas cifradas de los usuarios.

**`/etc/gshadow`**: es un archivo de cuatros campos delimitados por dos punto que
contiene contraseñas cifradas de los grupos.

Todos estos archivos se actualizan mediante un conjunto de herramientas de línea de
comandos para la administración de usuarios y grupos. También se pueden administrar
mediante aplicaciones gráficas según la distribución que proporcionan interfaces más
simples e inmediatas.

#### El archivo `/etc/passwd`
`/etc/passwd` es un archivo legible por todos que contiene una lista de usuarios en
cada línea:

    frank:x:1001:1001::/home/frank:/bin/bash

Cada línea consta de siete campos delimitados por dos puntos:

**`Username`**: el nombre utilizado cuando el usuario inicia sesión en el sistema.

**`Password`**: la contraseña cifrada (o una `x` si usa contraseñas ocultas).

**`User ID (UID)`**: el número de ID asignado al usuario en el sistema.

**`Group ID (GID)`**: el número de grupo primario del usuario en el sistema.

**`GECOS`**: un campo de comentario opcional, que se utiliza para agegar información
adicional sobre el usuario (como el nombre completo). El campo puede contener
múltiples entradas separadas por coma.

**`Home Directory`**: la ruta absoluta del directorio de inicio de usuario.

**`Shell`**: la ruta absoluta del programa que se inicia automáticamente cuando el
usuario inicia sesión en el sistema (generalmente un shell interactivo como `/bin/bash`).

#### El archivo `/etc/group`
`/etc/group` es un archivo legible por todos que contiene una lista de grupos, cada
una en una línea separada:

    developer:x:1002:

Cada línea consta de cuatro campos delimitados por dos puntos:

**`Group Name`**: el nombre del grupo.

**`Group Password`**: la contraseña cifrada del grupo (o una `x` si usa contraseñas ocultas).

**`Group ID (GID)`**: el número de identificación asignado al grupo en el sistema.

**`Member List`**: una lista delimitada por comas de los usuarios que pertenecen al
grupo, excepto aquellos para quienes este es el grupo primario.

#### El archivo `/etc/shadow`
`/etc/shadow` es un archivo legible solo por el usuario `root` y usuarios con
privilegios de `root`. Además contiene las contraseñas cifradas de los usuarios
separadas por una línea:

    frank:$6$i9gjM4Md4MuelZCd$7jJa8Cd2bbADFH4dwtfvTvJLOYCCCBf/.jYbK1IMYx7Wh4fErXcc2xQVU2N1gb97yIYaiqH.jjJammzof2Jfr/:18029:0:99999:7:::

Cada línea consta de nueve campos delimitados por dos puntos:

**`Username`**: el nombre utilizado cuando el usuario inicia sesión en el sistema.

**`Encrypted Password`**: la contraseña cifrada del usuario (si el valor es `!`, la
cuneta está bloqueada).

**`Date of last password change`**: la fecha del último cambio de contraseña, como
número de días desde 01/01/1970. Un valor `0` significa que el usuario debe cambiar
la contraseña en el siguiente acceso.

**`Minimum password age`**: el número mínimo de días después de un cambio de
contraseña que debe pasar antes de que el usuario pueda cambiar nuevamente.

**`Password warning period`**: el número de días antes de que caduque la contraseña,
durante los cuales se advierte al usuario que se debe cambiar.

**`Password inactivity period`**: el número de días de que caduca la contraseña el
cual el usuario debe actualizarla. Después de este período, si el usuario no cambia
la contraseña, la cuenta se deshabilitará.

**`Account expiration date`**: la fecha, como número de días desde el 01/01/1970 en
que se deshabilitará la cuennta de usuario. Un campo vacío significa que la cuenta de
usuario nunca caducará.

**`A reserved fiel`**: un campo reservado para uso futuro.

#### El archivo `/etc/gshadow`
`/etc/gshadow` es un archivo legible solo por el usuario `root` y por usuarios con
privilegios de `root` que contiene contraseñas cifradas para grupos, cada uno en una
línea separada:

    developer:$6$7QUIhUX1WdO6$H7kOYgsboLkDseFHpk04lwAtweSUQHipoxIgo83QNDxYtYwgmZTCU0qSCuCkErmyR263rvHiLctZVDR7Ya9Ai1::

Cada línea consta de cuatro campos delimitados por dos puntos:

**`Group name`**: el nombre del grupo.

**`Encrypted password`**: la contraseña cifrada para el grupo (se usa cuando un
usuario que no es miembro del grupo, desea unirse al grupo usando el comando `newgrp`,
si la contraseña comienza con `!`, nadie puede acceder al grupo con `newgrp`).

**`Group administrators`**: una lista delimitada por comas de los administradores del
grupo (puede cambiar la contraseña deñ grupo y puede agregar o eliminar miembros del
grupo con el comando `gpasswd`).

**`Group members`**: una lista delimitada por comas de los miembros del grupo.

#### Agregar y eliminar cuentas de usuario
En Linux, puede agragar una nueva cuenta de usuario con el comando `useradd` (es
mejor `adduser`) y puede eliminar una cuenta de usuario con el comando `userdel`.

Si desea crear una cuenta nueva de usuario llamada `frank` con una configuración
predeterminada, puede ejecutar lo siguiente:

    sudo useradd frank

Después de crear el nuevo usuario, puede establecer una contraseña usando `passwd`:

    sudo passwd frank

Las opciones más importantes que se aplican al comando `useradd` son:

- **`-c`**: crea una nueva cuenta de usuario con comentarios personalizados (por ejemplo, nombre completo).
- **`-d`**: craer una nueva cuenta de usuario con un directorio de inicio personalizado.
- **`-e`**: crea una nueva cuenta de usuario estableciendo una fecha específica en la que se deshabilitará.
- **`-f`**: crear una nueva cuenta de usuario estableciendo el número de días después de que caduque la contraseña durante los cuales el usuario debe actualizar la contraseña.
- **`-g`**: crea una nueva cuenta de usuario con un GID específico.
- **`-G`**: crea una nueva cuenta de usuario agragándola a múltiples grupos secundarios.
- **`-m`**: crea una nueva cuenta de usuario con su directorio de inicio.
- **`-M`**: crea una nueva cuenta de usuario sin su directorio de inicio.
- **`-s`**: crea una nueva cuenta de usuario con un shell de inicio de sesión específico.
- **`-u`**: crea una nueva cuenta de usuario con un UID específico.

Si deasea eliminar una cuenta de usuario, puede usar el comando `userdel`. La opción
`-r` también elimina el directorio de inicio de usuario y todo su contenido, junto
con la cola del correo del usuario. Otros archivos, ubicados en otro lugar, deben
buscarse y eliminarse manualmente.

#### El directorio `skel`
Cuando agrega una nueva cuenta de usuario, incluso al crear su directorio de inicio,
el directorio de inicio recién creado se llena con archivos creados y carpetas que
se copian del directorio de skel (por defecto `/etc/skel`). La idea detras de esto, es
simple: un administrador del sistema quiere agregar nuevos usuarios que tengan los
mismos archivos y directorios en su hogar. Por lo tanto, si desea personalizar los
archivos y carpetas que se crean automáticamente en el directorio de inicio de las
nuevas cuentas de usuario, debe agregar estos nuevos archivos y carpetas al directorio
skel.

El contenido de este directorio está oculto. Para enumerar el contenido usar `ls -Al`.

#### Agregar y eliminar grupos
En cuanto a la gestión de grupos, puede agregar o eliminar grupos utilizando
`groupadd` y `groupdel`.

Crear un nuevo grupo llamado `developer`:

    sudo groupadd -g 1090 developer

La opción `-g` de este comando crea un grupo con un GID específico.

Comando para eliminar el grupo `developer`:

    sudo groupdel developer

#### El comando `passwd`
este comando se usa principalmente para cambiar la contraseña de usuario. Cualquier
otro usuario puede cambiar su contraseña, pero solo el usuario `root` puede cambiar
la contraseña de cualquier usuario.

Dependiendo de la opción `passwd` utilizada, puede controlar aspectos específicos del
envejecimiento de la contraseña:
- **`-d`**: elimina la contraseña de una cuenta de usuario (estableciendo así una contraseña vacía y convirtiéndola en una cuenta sin contraseña).
- **`-e`**: fuerza a la cuenta de usuario a cambiar de contraseña.
- **`-l`**: bloquea la cuenta de usuario (coloca un signo de `!` al inicio del hash).
- **`-u`**: desbloquea la cuenta de usuario (elimina `!` al inicio del hash).
- **`-S`**: muestra información sobre el estado de la contraseña para una cuenta específica.

## Gestión de los permisos y la propiedad de los archivos
Al usar un sistema multiusuario, Linux necesita alguna forma de rastrear quién es
dueño de cada archivo y si un usuario puede o no realizar acciones. Lo anterior es
para garantizar la privacidad de los usuarios que deseen mantener la confidencialidad
del contenido de sus ficheros, así como garantizar la colaboración al hacer que
ciertos archivos sean accesibles para múltiples usuarios.

#### Consultar información sobre archivos y directorios
El comando `ls` se usa para obtener una lista de contenido de cualquier directorio.
```bash
ls
Another_Directory
picture.jpg
text.txt
```

```bash
ls -l
total 536
drwxrwxr-x 2 carol carol
4096 Dec 10 15:57 Another_Directory
-rw------- 1 carol carol 539663 Dec 10 10:43 picture.jpg
-rw-rw-r-- 1 carol carol
1881 Dec 10 15:57 text.txt
```

Cada columna en la salida anterior tiene un significado:
- La primera columna de la lista muestra el tipo de archivo y los permiso, por ejemplo, en `drwxrwxr-x`:
    - El primer carácter, `d` indica el tipo de archivo
    - Los siguientes tres caracteres, `rwx` indican los permisos para el propietario del archivo, también conocido como usuario o `u`.
    - Los siguientes tres caracteres, `rwx` indica los permisos del grupo que posee el archivo, también conocido como `g`.
    - Los últimos tres caraceres, `r-x` indican los permisos para cualquier otra persona, también conocidos como otros o `o`.
- La segunda columna indica el número de enlaces duros (hard links) que apuntan a ese archivo. Para un directorio, significa el número de subdirectorios, más un enlace a sí mismo (`.`) y al directorio padre (`..`).
- La tercera y cuarta columna muestra información de propiedad: el usuario y el grupo que posee el archivo.
- La quinta columna muestra el tamaño del archivo en bytes.
- La sexta columna muestra la fecha y la hora precisa, o la marca del tiempo cuando se modificó el archivo por última vez.
- La séptima y última columna muestran el nombre del archivo.

#### Directorios
Listar el contenido de un directorio:

    ls -l path/

Para listar información solo del directorio:

    ls -ld path/

Para los archivos ocultos:

    ls -la path/

#### Comprendiendo los tipos de archivos
**`-` (archivo noral)**: un archivo puede contener datos de cualquier tipo.

**`d` (directorio)**: un directorio contiene otros archivos o directorios y ayuda a
organizar el sistema de archivos.

**`l` (enlace suave)**: este archivo es un puntero a otro archivo o directorio en
otra parte del sistema de archivos.

**`b` (dispositivo de bloque)**: este archivo representa un archivo virtual o físico,
generalmente discos u otros tipos de dispositivos de almacenamiento.

**`c` (dispositivo de caracteres)**: representa un archivo virtual o físico. Los
terminales (como el terminal principal `/dev/ttyS0`) y los puertos seriales son
ejemplos comunes de dispositivos de caracteres.

**`s` (socket)**: los sockets sirven como "conductores" para pasar información entre
dos programas.

#### Numeric mode
En *modo númerico* los permisos se específican de manera diferente: como un valor
númerico de tres dígitos en notación octal, un sistema númerico de base 8.

Cada permiso tiene un valor correspondiente, y se específica en el siguiente orden:
primero lectura (`r`) que es 4; luego escritura (`w`), que es 2 y el último ejecución
(`x`), representado por 1. Si no hay permisos, use el valor cero (`0`). Entonces un
permiso `rwx` sería 7 (`4+2+1`) y `r-x` (`4+0+1`).

El primero de los tres dígitos representa los permisos para el usuario (`u`), el
segundo para el grupo (`g`) y el tercero para otros (`o`). Si quisiéramos establecer
los permisos para un archivo como `rw-rw----`, el valor octal sería `660`:

    chmod 660 file.txt

#### Modificando la propiedad del archivo
El comando `chown` se usa para modificar la propiedas de un archivo o directorio. La
sintaxis es bastante simple:

    chown username:groupname filename

Por ejemplo, verifiquemos un archivo llamado `text.txt`:
```bash
ls -l text.txt
-rw-rw---- 1 carol carol 1881 Dec 10 15:57 text.txt
```
El usuario propietario del archivo es `carol`, y el grupo también es `carol`. Ahora
modifiquemos el grupo que posee el archivo a otro grupo como `students`:
```bash
chown carol:students text.txt
ls -l text.txt
-rw-rw---- 1 carol students 1881 Dec 10 15:57 text.txt
```

Tenga cuenta que el usuario que posee un archivo no necesita pertenecer al grupo que
lo posee. En el ejemplo anterior, `carol` no necesita ser miembroo del grupo
`students`. Sin embargo, ella tiene que ser miembro del grupo para tranferir la
propiedad del archivo a ese grupo.

El grupo o usuario puede omitirse si no desea cambiarlos. Entonces, para cambiar solo
el grupo que posee un archivo, usaría `chown :students test.txt`. Para cambiar solo
el usuario, el comando sería `chown carol test.txt`. Alternativamente, puede usar el
comando `chgrp students test.txt` para cambiar solo el grupo.

A menos que sea el administrador del sistema (`root`), no puede cambiar la propiedad
de un archivo propiedad de otro usuario o grupo al que no pertenece.

#### Consultado los grupos
Antes de cambiar la propiedad de un archivo, puede ser útil saber qué grupos existen
en el sistema, qué usuarios son miembros de un grupo y a qué grupos pertenece un
usuario. Esas tareas se pueden realizar con dos comandos, `groups` y `groupmems`.

Para saber los grupos que existen en un sistema:

    groups

Para saber a qué grupos pertenece un usuario:

    groups carol

Para hacer lo contrario, mostrar qué usuarios pertenecen a un grupo, user `groupmems`.
El parámetro `-g` específica el grupo, y `-l` enumerará todos sus miembros:

    sudo groupmems -g sudo -l

#### Permisos especiales
Además de los permisos de lectura, escritura y ejecución para el usuario, grupos y
otros, cada archivo puede tener otros permisos especiales que pueden alterar la forma
en que funciona un directorio o cómo se ejecuta un programa. Se puede específicar en
modo simbólico o numérico, y son los siguientes:

**Stick bit**  
El stick bit, también llamado *restricted deletion flag*, tiene el valor octal `1` y
en modo simbólico está representado por una `t` dentro de los permisos del otro. Esto
se aplica solo a los directorios, y en Linux evita que los usuarios eliminen o cambien
el nombre de un archivo en un directorio a menos que sean dueños de ese archivo o 
directorios.

Los directorios con el conjunto de bits fijos muestran una `t` que reemplaza la `x`
en los permisos para otros a la hora de ver la salida de `ls -l`:
```bash
ls -ld Sample_Directory/
drwxr-xr-t 2 carol carol 4096 Dec 20 18:46 Sample_Directory/
```
En el modo numérico, los permisos especiales se específican usando una "notación de
4 dígitos", con el primer dígito representando el permiso especial para actuar. Por
ejemplo, para establecer el stick bit (valor `1`) para el directorio 
`Another_Directory` en modo numérico con permisos `755`, el comando sería:
```bash
chmod 1755 Another_Directory
ls -ld Another_Directory
```

**Set GID**  
Establecer GID, también conocido como SGID o "Set Group ID bit", tiene el valor octal
`2` y en modo simbólico está representado por una `s` en los permisos del grupo. Esto
se puede aplicar a archivos ejecutables o directorios. En los archivos ejecutables,
otorgará la ejecución del archivo con privilegios del grupo. Cuando se aplica los
directorios, hará que cada archivo o directorio creado debajo herede el grupo del
directorio principal.

Los archivos y directorios con bit SGID muestan una `s` que reemplaza la `x` en los
permisos para el grupo den la salida de `ls -l`:
```bash
ls -l test.sh
-rwxr-sr-x 1 carol carol 33 Dec 11 10:36 test.sh
```
Para agregar permisos SGID a un archivo en modo simbólico, el comando sería:
```bash
chmod g+s test.sh
ls -l test.sh
-rwxr-sr-x 1 carol root
33 Dec 11 10:36 test.sh
```
El siguiente ejemplo lo hará comprender mejor los efectos de SGID en un directorio.
Supongamos que tenemos un directorio llamado `Sample_Directory`, propiedad el usuario
`carol` y el grupo `users`, con la siguiente estructura de permisos:
```bash
ls -ldh Sample_Directory/
drwxr-xr-x 2 carol users 4,0K Jan 18 17:06 Sample_Directory/
```
Ahora, cambiemos de directorio y usando el comando `touch`, creemos un archivo vacío
dentro del él. 
```bash
cd Sample_Directory/
touch newfile
ls -lh newfile
-rw-r--r-- 1 carol carol 0 Jan 18 17:11 newfile
```
Como podemos ver, el archivo es propiedad del usuario `carol` y del grupo `carol`.
Pero, si el directorio tuviera establecido el permiso SGID, el resultado sería
diferente. Primero, agreguemos el bis SGID al `Sample_Directory` y verifiquemos los
resultados:
```bash
sudo chmod g+s Sample_Directory/
ls -ldh Sample_Directory/
drwxr-sr-x 2 carol users 4,0K Jan 18 17:17 Sample_Directory/
```
La `s` en los permisos del grupo indica que el bit SGID está establecido. Ahora,
cambiemos a este directorio y nuevamente creemos un archivo:
```bash
cd Sample_Directory/
touch emptyfile
ls -lh emptyfile
-rw-r--r-- 1 carol users 0 Jan 18 17:20 emptyfile
```
Como podemos ver, el grupo que posee el archivo es `users`. Esto se debe a que el bit
SGID hizo que el archivo heredara el propietario del grupo de su directorio principal
que es `users`.

**Set UID**  
SUID, también conocido como "Set User ID", tiene el valor octal `4` y está
representado por una `s` en los permisos user en modo simbólico. Solo se aplicaa los
archivos y su comportamiento es similar al bit SGID, pero el proceso se ejecutará con
los permisos del usuario que posee el archivo. Los archivos con el bit SUID muestran
una `s` que reemplaza la `x` en los permisos para el usuario en la salida `ls -l`:
```bash
ls -ld test.sh
-rwsr-xr-x 1 carol carol 33 Dec 11 10:36 test.sh
```
Puede cambiar múltiples permisos especiales en un parámetro agrgándolos juntos. 
Entonces, para establecer SGID (valor `2`) y un SUID (valor `4`) en modo numérico para
el script `test.sh` con permisos `755`, sebe escribir:

    chmod 6755

Y el resultado sería:
```bash
ls -lh test.sh
-rwsr-sr-x 1 carol carol 66 Jan 18 17:29 test.sh
```

## Directorios y archivos especiales
#### Archivos temporales
Los archivos temporales son archivos utilizados por los programas para almacenar datos
que solo se necesitan por un corto tiempo. Estos pueden ser datos de procesos en
ejecución, registros de bloqueo, archivos de memoria virtual de un autoguardado,
archivos intermedios utilizados durante una conversión de archivos, archivos de caché,
etc.

#### Ubicación de los archivos temporales
La versión 3.0 del *Filesystem Hierarchy Standard* (FHS) define las ubicaciones
estándar para los archivos temporales en los sistemas Linux. Cada ubicación tiene
un propósito y comportamiento diferente, y ser recomienda que los desarrolladores
sigan las convenciones establecidas por el FHS cuando escriban datos temporales en el
disco.

**`/tmp`**: según el FHS, los programas no deben asumir que los archivos escritos se
conservarán entre las invocaciones de un programa. La recomendación es que este
directorio sea limpiado durante el arranque del sistema, aunque esto no es obligatorio.

**`/var/tmp`**: otra ubicación para los archivos temporales, pero esta no debe ser
borrada durante el arranque del sistema, es decir, los archivos almacenados aquí
normalmente persistirán entre los reinicios.

**`/run`**: este directorio contiene variables en tiempo de ejecución utilizados por
los procesos en ejecución, como los archivos de identificación de los procesos 
(`.pid`). Los programas que necesitan más de un archivo de tiempo de ejecución pueden
crear subdirectorios aquí. Esta ubicación debe ser despejada durante el arranque del
sistema. El propósito de este directorio fue una vez servido por `/var/run` y en
algunos sistemas `/var/run` pueden ser un enlace simbólico a `/run`.

Tenga en cuenta que no hay nada que impida a un programa crear archivos temporales en
otra parte del sistema, pero es buena práctica respetar las convenciones establecidas
por FHS.

#### Permisos en los archivos temporales
Disponer de directorios temporales en todo el sistema en un sistema multiusuario,
presenta algunos problemas en lo que respecta a los permisos de acceso. Al principio
se pensó que tales directorio serían "escritor por todo el mundo", es decir, que
cualquier usuario podría escribir o borrar datos en ellos. Pero si esto fuera cieto,
¿cómo podríamos evitar que un usuario borre o modifique los archivos creados por
otros?

La solución es un permiso especial llamado *stick bit*, que se aplica tanto a
directorios como a archivos. Sin embargo, por razones de seguridad, el kernel de Linux
ingnora el bit fijo cuando se aplica a los archivos. Cuando este bit especial se
establece para un directorio, evita que los usuarios eliminen o renombren un archivo
dentro de ese directorio a menos que sea propietarios del archivo.

Los directorios con el stick bit muestran una `t` reemplazando la `x` en los permisos
de *otros* en la salida `ls -l`. Por ejemplo, revisemos los permisos de los
directorios `/tmp` y `/var/tmp`:
```bash
ls -ldh /tmp/ /var/tmp/
drwxrwxrwt 25 root root 4,0K Jun 7 18:52 /tmp/
drwxrwxrwt 16 root root 4,0K Jun 7 09:15 /var/tmp/
```
Como puedes ver por la `t` que reemplaza a la `x` en los permisos para otros, ambos
directorios tienen el stick bit.

Para poner el stick bit en un directorio usando `chmod` en modo númerico, usa la
notación de cuatro dígitos y `1` como primer dígito. Por ejemplo:

    chmod 1755 temp

Cuando use el modo simbólico, use el parámetro `t`. Entonces,`+t` para establecer el
stick bit, y `-t` para desactivarlo:

    chmod +t temp

#### Comprendiendo los enlaces
Ya hemos dicho que el Linux, todo se trata como un archivo. Pero hay un tipo especial
llamado *link*, y hay dos tipos de enlaces en un sistema Linux:

**Enlaces simbólicos**: también llamados *enlaces suaves* (soft links), apuntan a la
ruta de otro archivo. Si borras el archivo al que apunta el enlace (llamado *taget*),
el enlace seguirá existiendo, pero dejara de funcionar, ya que ahora apunta a nada.

**Enlaces duros**: piensa en un enlace duro como un segundo nombre para el archivo
original. No son duplicados, sino una entrada adicional en el sistema de archivos que
apunta al mismo lugar (inodo) en el disco.

#### Trabajar con enlaces duros (Hard Links)
#### Creación de enlaces duros
El comando para crear un enlace duro en Linux es `ln`. La sintaxis básica es:

    ls target link_name

El `target` debe existir (este es el archivo al que aputará el enlace), y si el
destino no está en el directorio actual, o si quieres crear un enlace en otro lugar,
debes definir la ruta completa. Por ejemplo, el comando:

    ln target.txt /home/carol/Documents/hardlink

Creará un archivo llamado `hardlink` en el directorio `/home/carol/Documents/`,
enlazado al archivo `target.txt` en el directorio actual.

Si se omite el último parámetro (`link_name`), se creará un enlace con el mismo
nombre del objetivo en el directorio actual.

#### Administrar los enlaces duros
Los enlaces duros son entradas en el sistema de archivos que tienen nombres
diferentes, pero que apuntan a los mismos datos en el disco. Todos esos nombre son
iguales y pueden ser usados para referirse a un archivo. Si se cambia el contenido de
unos de los nombres, el contenido de todos los demás nombres que apuntan a ese archivo
cambian, ya que todos estos nombres apuntan a los mismos datos. Si elimina uno de los
nombres, los otros nombres seguiran funcionando.

Esto sucede porque cuando borrar un archivo, los datos no se borral realmente del
disco. El sistema simplemente borra la entrada en la tabla del sistema de archivos que
apuntan al inodo correspondiente a los datos del disco. Pero si tiene una segunda
entrada que apunta al mismo inodo, todavía puedes llegar a los datos. Pienso en ello
como dos caminos que convergen en el mismo punto. incluso si bloqueas o redireccionas
uno de los caminos, todavía puedes llegar al destino usando el otro.

Puedes comprobarlo usando el parámetro `ls -i`:
```bash
ls -li
total 224
3806696 -r--r--r-- 2 carol carol 111702 Jun 7 10:13 hardlink
3806696 -r--r--r-- 2 carol carol 111702 Jun 7 10:13 target.txt
```
El número que precede a los permisos es el número `inode`. ¿Ves que tanto el archivo
`hardlink` como el archivo `target` tienen el mismo número (`38006696`)? Esto es
porque uno es un enlace duro del otro.

Pero, ¿cuál es el original y cuál es el enlace? No se puede decir realmente, ya que
para todos los porpósitos prácticos son los mismos.

Los archivos temporales son archivos utilizados por los programas para almacenar
datos que solo se necesitan por un corto tiempo. Estos pueden ser los datos en
procesos en ejecución, registros de bloqueo, archivos de memoria virtual de un
autoguardado, archivos intermedios utilizados durante una conversión de archivos,
archivos de caché, etc.

A diferencia de los enlaces simbólicos, solo puede crear enlaces duros a los archivos,
y tanto el enlace como el destino deben residir en el mismo sistema de archivos.

#### Mover y eliminar enlaces duros
Dado que los enlaces duros se tratan como archivos normales, pueden eliminarse con
`rm` y cambiarse de nombre o moverse por el sistema de archivos con `mv`. Y dado que
un enlace duro apunta al mismo inodo de destino (taget), puede moverse libremente,
sin temor a romper el enlace.

#### Enlaces simbólicos
#### Creando enlaces simbólicos
El comando utilizado para crear un enlace simbólico tambien es `ls`, pero agregando
el parámetro `-s`:

    ln -s target.txt /home/carol/Documents/softlink

#### Administrando los enlaces simbólicos
Los enlaces simbólicos apuntan a otra ruta en el sistema de archivos. Puede crear un
enlace simbólico a los archivos y directorios, incluso en diferentes particiones. Es
bastante fácil detectar un enlace simbólico en la salida `ls -l`:
```bash
ls -lh
total 112K
-rw-r--r-- 1 carol carol 110K Jun 7 10:13 target.txt
lrwxrwxrwx 1 carol carol 7 10:14 softlink -> target.txt
```
En el ejemplo anterior, el primer cáracter en los permisos del archivos `softlink` es
`l`, indica un enlace simbólico. Además, justo después del nombre del archivo vemos
el nombre del objetivo al que apunta el enlace, el archivo `taget.txt`.

#### Mover y eliminar enlaces simbólicos
Como los enlaces duros, los enlaces simbólicos pueden ser eliminados usnado `rm` y
movidos o renombrados usando `mv`. Sin embargo, se debe tener especial cuidado al
crearlos, para evitar romper el enlace si se mueve de su ubicación original.

Cuando se crean enlaces simbólicos hay que tener en cuenta que, a menos que se
especifique completamente un camino, la ubicación del objetivo se interpreta como
relativa a la ubicación del enlace. Esto puede crear problemas si el enlace o el
archivo al que apunta, se mueve.

La forma de evitarlo es especificar siempre el camino completo al objetivo al crear
el enlace:

    ln -s /home/carol/Documents/original.txt softlink

De esta manera, no importa a dónde muevas el enlace, este seguirá funcionando, porque
apunta a la ubicación absoluta del objetivo (`target`).
