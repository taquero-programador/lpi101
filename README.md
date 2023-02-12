# Linux Professionl

# Linux 101
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

Unix se utilizó mucho en las empresas, pero hemos visto una disminuación en la suerte
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
    servidor web contiene una configuración de logrotate, esta configuracón ahora se
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

Por otro lado, existe el concepto de memoria virtual, que es una abstracciónd e la
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
- **`-u`**: muestra mensajes sobre una unidad especifica. Aproximadamente una unidad se puede definir como cualquier recurso manejado por systemd. Por ejemplo, `journalctl -u apache2.service` se usa para leer mensajes sobre el servicio web Apache2.
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

### DNS
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
