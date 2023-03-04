# Linux 101

## Arquitectura del sistema
#### Activación de dispositivos
la utilidad de configuración del sistema se presenta después de presionar una tecla
específica cuando se enciende la computadora. La tecla que debe presionar varía de un
fabricante a otro, pero generalmente es `del` o una de las teclas de función, como
`F2` o `F12`. Generalmente, la combinación de teclas para iniciar la configuración del
BIOS se muestra en la patalla al iniciar la máquina.

En la utilidad de configuración del BIOS, es posible habilitar y deshabilitar
periféricos integrados, activar la protección básica contra errores y cambiar
configuraciones de hardware como IRQ (solicitud de interrupción) y DMA (acceso directo
a memoria). Raramente se necesita esta configuración en las máquinas modernas, pero
puede ser necesario hacer ajustes para abordar problemas específicos. Existen
tecnologías RAM, por ejemplo, que son compatible con velocidades de tranferencia de
datos más rápidas que los valores predeterminados, por lo que se recomienda cambiarlo
a los valores especificados por el fabricante. Algunas CPU ofrecen características que
pueden no ser necesarias para una instalación en particular y pueden desactivarse.
Las funciones deshabilitadas reducirán el consumo de energía y pueden aumentar la
protección del sistema, ya que las funciones del CPU que contienen errores conocidos
también se pueden deshabilitar.

Si la máquina está equipada con muchos dipositivos de almacenamiento, es importante
definir cuál tiene el gestor de arranque correcto y debe ser la primera entrada en el
orden de arranque del dispositivo. Es posible que el sistema operativo no se cargue si
el dispositivo incorrecto aparece primero en la lista de verificación de arranque del
BIOS.

#### Inspección de dispositivos en Linux
Una vez que los dispositivos se identifican correctamente, corresponde al sistema
operativo asociar los componentes de software correspondientes requeridos por ellos.
Cuando una característica de hardware no funciona como se esperaba, es importante
identificar dónde está sucediendo exactamente el problema. Cuando el sistema operativo
no detecta un dispositivo, lo más probable es que este, o al puerto que esté conectado,
esté defectuoso. Cuando el dispositivo se detecta correctamente, pero aún no funciona
como se espera, puede haber un problema en el lado del sistema operativo. Por lo tanto,
uno de los primeros pasos cuando se trata con problemas relacionados con el hardware
es verificar si el sistema operativo está detectando correctamente el dipositivo. Hay
dos formas básicas de identificar recursos de hardware en un sistema Linux: usar
comandos especializados o leer archivos específicos dentro de sistemas de archivos
especiales.

#### Comandos para inspección
Dos comandos escenciales para identificar dispositivos conectados en Linux son:

**`lspci`**: muestra todos los dipositivos actualmente conectados al bus PCI
(*Peripheral Component Interconnect*). Los dispositivos PCI pueden ser componentes
conectados a la placa base, como un controlador de disco o una tarjeta de expansión
instalada en una ranura PCI, como una tarjeta gráfica externa.

**`lsusb`**: enumera los dipositivos USB (*Universal Serial Bus*) actualmente
conectados a la maquina. Aunque existen dipositivos USB para casi cualquier porpósito
imaginable, la interfaz USB se utiliza en gran medida para conectar dipositivos de
entrada (teclados, dispositivos señaladores) y medios de almacenamiento extraíbles.

La salida de los comandos`lspci` y `lsusb` consiste en una lista de todos los
dispositivos PCI y USB identificados por el sistema operativo. Sin embargo, es posible
que el dispositivos aún no esté completamente operativo, porque cada parte del
hardware requiere de un componente de software para controlar los dipositivos
correspondientes. Este componente de software se denomina módulo del kernel y puede
formar parte del núcleo oficial de Linux o agregarse por separado de otras fuentes.

Los módulos del núcleo de Linux relacionados con dispositivos de hardware también se
denominan controladores, como en otros sistemas operativos. Sin embargo, los
controladores para Linux no siempre son suministrados por el fabricante del dipositivo.
Mientras que algunos fabricantes proporcionan sus propios controladores binarios para
que se instalen por separado, muchos controladores están escritos por desarrolladores
independientes. Históricamente, las partes que funcionan en Windows, por ejemplo,
pueden no tener un módulo del núcleo equivalente para Linux. Hoy en día, los sistemas
operativos basados en Linux tiene un fuerte soporte de hardware y la mayoría de los
dispositivos funcionan sin esfuerzo.

Los controladores directamente relacionados con el hardware a menudo requieren
privilegios de `root` para ejecutarse o solo mostrarán información limitada cuando los
ejecute un usuario normal, por lo que puede ser necesario iniciar sesión como `root` o
ejecutar el comando con `sudo`.

La siguiente salida del comando `lspci`, por ejemplo, muestra algunos dispositivos
identificados:
```bash
lspci
01:00.0 VGA compatible controller: NVIDIA Corporation GM107 [GeForce GTX 750 Ti]
(rev a2)
04:02.0 Network controller: Ralink corp. RT2561/RT61 802.11g PCI
04:04.0 Multimedia audio controller: VIA Technologies Inc. ICE1712 [Envy24] PCI
Multi-Channel I/O Controller (rev 02)
04:0b.0 FireWire (IEEE 1394): LSI Corporation FW322/323 [TrueFire] 1394a Controller
(rev 70)
```
La salida de dicho comando puede tener decenas de líneas. Los números hexadecimales al
principio de cada línea son las direcciones únicas del dispositivos PCI
correspondiente. El comando `lspci` muestra más detalles sobre un dispositivo
específico si su dirección se da con la opción `-s`, acompañado de la opción `-v`:
```bash
lspci -s 04:02.0 -v
04:02.0 Network controller: Ralink corp. RT2561/RT61 802.11g PCI
Subsystem: Linksys WMP54G v4.1
Flags: bus master, slow devsel, latency 32, IRQ 21
Memory at e3100000 (32-bit, non-prefetchable) [size=32K]
Capabilities: [40] Power Management version 2
kernel driver in use: rt61pci
```
El resultado ahora muestra muchos más detalles del dispositivo en la dirección
`04:02.0`. Es un controlador de red, cuyo nombre interno es 
`Ralink corp. RT2561/RT61 802.11g PCI`. `Subsystem` está asociado con la marca y el
modelo del dispositivo (`Linksys WMP54G v4.1`) y puede ser útil para fines de
diagnóstico.

El módulo del núcleo del sistema operativo se puede identificar en la línea 
`kernel driver in use`, que muestra el módulo `rt61pci`. De toda la información
recopilada, es correcto suponer que:
1. El dispositivo ha sido identificado.
2. Se cargó un módulo en el núcleo del sistema operativo.
3. El dispositivo debe estar listo para usarse.

La opción `-k`, disponible en versiones más recientes de `lspci`, porporciona otra
forma de verificar qué módulo del núcleo del sistema operativo está en uso para el
dipositivos especificado:
```bash
lspci -s 01:00.0 -k
01:00.0 VGA compatible controller: NVIDIA Corporation GM107 [GeForce GTX 750 Ti]
(rev a2)
kernel driver in use: nvidia
kernel modules: nouveau, nvidia_drm, nvidia
```
Para el dispositivo elegido, una placa GPU NVIDIA, `lspci` indica que el módulo en uso
se llama `nvidia`, en la línea `kernel driver un user`: `nvidia` y todos los módulos
del núcleo del sistema operativo correspondientes se enumeran en la línea 
`kernel modules`: `nouveau`, `nvidia_drm`, `nvidia`.

El comando `lsusb` es similar a `lspci`, pero enumera la información de USB
exclusivamente:
```bash
lsusb
Bus 001 Device 029: ID 1781:0c9f Multiple Vendors USBtiny
Bus 001 Device 028: ID 093a:2521 Pixart Imaging, Inc. Optical Mouse
Bus 001 Device 020: ID 1131:1001 Integrated System Solution Corp. KY-BT100
Bluetooth Adapter
Bus 001 Device 011: ID 04f2:0402 Chicony Electronics Co., Ltd Genius LuxeMate i200
Keyboard
Bus 001 Device 007: ID 0424:7800 Standard Microsystems Corp.
Bus 001 Device 003: ID 0424:2514 Standard Microsystems Corp. USB 2.0 Hub
Bus 001 Device 002: ID 0424:2514 Standard Microsystems Corp. USB 2.0 Hub
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
```
El comando `lsusb` muestra los canales USB disponibles y los dipositivos conectados a
ellos. Al igual que con `lspci`, la opción `-v` muestra una salida más detallada. Se
puede seleccionar un dispositivo específico para inspección porporcionando su ID a la
opción `-d`:
```bash
lsusb -v -d 1781:0c9f
Bus 001 Device 029: ID 1781:0c9f Multiple Vendors USBtiny
Device Descriptor:
bLength
18
bDescriptorType
1
bcdUSB
1.01
bDeviceClass
255 Vendor Specific Class
bDeviceSubClass 0
bDeviceProtocol 0
bMaxPacketSize0 8
idVendor 0x1781 Multiple Vendors
idProduct 0x0c9f USBtiny
bcdDevice
1.04
iManufacturer 0
iProduct 2 USBtiny
iSerial 0
bNumConfigurations 1
```
Con la opción `-t`, el comando `lsusb` muestra las asignaciones actuales de los
dispositivos USB en forma de árbol jerárquico:
```bash
lsusb -t
/:
Bus 01.Port 1: Dev 1, Class=root_hub, Driver=dwc_otg/1p, 480M
|__ Port 1: Dev 2, If 0, Class=Hub, Driver=hub/4p, 480M
|__ Port 1: Dev 3, If 0, Class=Hub, Driver=hub/3p, 480M
|__ Port 2: Dev 11, If 1, Class=Human Interface Device, Driver=usbhid,
1.5M
|__ Port 2: Dev 11, If 0, Class=Human Interface Device, Driver=usbhid,
1.5M
|__ Port 3: Dev 20, If 0, Class=Wireless, Driver=btusb, 12M
|__ Port 3: Dev 20, If 1, Class=Wireless, Driver=btusb, 12M
|__ Port 3: Dev 20, If 2, Class=Application Specific Interface,
Driver=, 12M
|__ Port 1: Dev 7, If 0, Class=Vendor Specific Class, Driver=lan78xx,
480M
|__ Port 2: Dev 28, If 0, Class=Human Interface Device, Driver=usbhid, 1.5M
|__ Port 3: Dev 29, If 0, Class=Vendor Specific Class, Driver=, 1.5M
```
Es posible que no todos los dispositivos tengan los módulos correspondientes asociados.
La comunicación con determinados dipositivos puede ser manejada directamente por la
aplicación, sin la intermediación que proporciona un módulo. Sin embargo, hay
información significativa en la salida proporcionada por `lsusb -t`. Cuando existe un
módulo coincidente, su nombre aparece al final de la línea del dispositivo, como
`Driver=btusb`. El dispositivo `Class` identifica la categoría general, como
`Human Interface Device, Wireless, Vendor Specific Class, Mass Storage`, entre otros.
Para verificar qué dipositivo está usando el módulo `btusb`, presente en la lista
anterior, se debe danto los números de `Bus` como de `Dev` a la opción `-s` del 
comando `lsusb`:
```bash
lsusb -s 01:20
Bus 001 Device 020: ID 1131:1001 Integrated System Solution Corp. KY-BT100
Bluetooth Adapter
```
Es común tener un gran conjunto de de módulos del núcleo del sistema operativo
cargados en un sistema operativo Linux estándar en cualquier momento. La forma
preferible de interactuar con ellos es usar los comandos proporcionados por el paquete
`kmod`, que es un conjunto de herramientas para manejar tarea comunes con módulos del
kernel de Linux, como insertar, eliminar, enumerar, verificar propiedades, resolver
dependencias y alias. El comando `lsmod`, por ejemplo, muestra todos los módulos
cargados actualmente:
```bash
lsmod
Module
Size
Used by
kvm_intel 138528 0
kvm 421021 1 kvm_intel
iTCO_wdt
iTCO_vendor_support
snd_usb_audio
13480 0
13419 1 iTCO_wdt
149112 2
snd_hda_codec_realtek 51465 1
snd_ice1712 75006 3
snd_hda_intel 44075 7
arc4 12608 2
snd_cs8427 13978 1 snd_ice1712
snd_i2c 13828 2 snd_ice1712,snd_cs8427
snd_ice17xx_ak4xxx 13128 1 snd_ice1712
snd_ak4xxx_adda 18487 2 snd_ice1712,snd_ice17xx_ak4xxx
microcode 23527 0
snd_usbmidi_lib 24845 1 snd_usb_audio
gspca_pac7302 17481 0
gspca_main 36226 1 gspca_pac7302
videodev
132348
2 gspca_main,gspca_pac7302
rt61pci 32326 0
rt2x00pci 13083 1 rt61pci
media 20840 1 videodev
rt2x00mmio 13322 1 rt61pci
hid_dr 12776 0
snd_mpu401_uart 13992 1 snd_ice1712
rt2x00lib 67108 3 rt61pci,rt2x00pci,rt2x00mmio
snd_rawmidi 29394 2 snd_usbmidi_lib,snd_mpu401_uart
```
La salida del comando `lsmod` se divide en tres columnas:
- **`Module`**: nombre del módulo
- **`Size`**: cantidad de memoria RAM ocupada por el módulo, en bytes.
- **`Used by`**: módulos dependientes

Algunos módulos requieren que otros módulos funcionen correctamente, como es el caso
de los módulos para los dispositivos de audio:
```bash
lsmod | fgrep -i snd_hda_intel
snd_hda_intel 42658
snd_hda_codec 155748
5
3 snd_hda_codec_hdmi,snd_hda_codec_via,snd_hda_intel
snd_pcm 81999 3 snd_hda_codec_hdmi,snd_hda_codec,snd_hda_intel
snd_page_alloc 13852 2 snd_pcm,snd_hda_intel
snd 59132 19
snd_hwdep,snd_timer,snd_hda_codec_hdmi,snd_hda_codec_via,snd_pcm,snd_seq,snd_hda_co
dec,snd_hda_intel,snd_seq_device
```
La tercera columna, `Used by`, muestra los módulos que requieren que el módulo de la
primera columna funcione correctamente. Muchos módulos de la arquitectura de sonido
de Linux, con el prefijo `snd`, son interdependientes. Al buscar problemas durante el
diagnóstico del sistema, puede ser útil descargar módulos específicos actualmente
cargados. El comando `modprobe` se puede usar tanto para cargar como para descargar
módulos del núcleo del sistema operativo: para descargar un módulo y sus módulos
relacionados, siempre que no estén siendo utilizados por un proceso en ejecución, se
debe usar el comando `modprobe -r`. Por ejemplo, para descargar el módulo 
`snd-hda-intel` (el módulo para un dispositivo de audio Intel HDA) y otros módulos con
el sistema de sonido:

    modprobe -r snd-hda-intel

Además de cargar y descargar módulos del núcleo del sistema operativo mientras el
sistema se está ejecutando, es posible cambiar los parámetros del módulo cuando se
está cargando el núcleo del sistema operativo, no muy diferente de pasar opciones a
comandos. Cada módulo acepta parámetros específicos, pero la mayoría de las veces se
recomiendam los valores predeterminados y no se necesitan parámetros adicionales.
Sin embargo, en algunos casos en necesario usar parámetros para cambiar el
comportamiento de un módulo para que funcione como se espera.

Usando el nombre del módulo como único argumento, el comando `modinfo` muestra una
descripción, el archivo, el autor, la licencia, la identificación, las dependencias y
los parámetros disponibles para el módulo dado. Los parámetros personalizados para un
módulo pueden hacerse persistentes al incluirlos en el archivo `/etc/modprobe.conf` o
en archivos individuales con la extensión `.conf` en el directorio `/etc/modprobe.d`.
La opción `-p` hará que el comando `modinfo` muestra todos los parámetros disponibles
e ignore la otra información:
```bash
modinfo -p nouveau
vram_pushbuf:Create DMA push buffers in VRAM (int)
tv_norm:Default TV norm.
Supported: PAL, PAL-M, PAL-N, PAL-Nc, NTSC-M, NTSC-J,
hd480i, hd480p, hd576i, hd576p, hd720p, hd1080i.
Default: PAL
NOTE Ignored for cards with external TV encoders. (charp)
nofbaccel:Disable fbcon acceleration (int)
fbcon_bpp:fbcon bits-per-pixel (default: auto) (int)
mst:Enable DisplayPort multi-stream (default: enabled) (int)
tv_disable:Disable TV-out detection (int)
ignorelid:Ignore ACPI lid status (int)
duallink:Allow dual-link TMDS (default: enabled) (int)
hdmimhz:Force a maximum HDMI pixel clock (in MHz) (int)
config:option string to pass to driver core (charp)
debug:debug string to pass to driver core (charp)
noaccel:disable kernel/abi16 acceleration (int)
modeset:enable driver (default: auto, 0 = disabled, 1 = enabled, 2 = headless)
(int)
atomic:Expose atomic ioctl (default: disabled) (int)
runpm:disable (0), force enable (1), optimus only default (-1) (int)
```
La salida muestra todos los parámetros disponibles para el módulo `nouveau`, un módulo
del kernel proporcionado por *Nouveau Project* como alternativa a los controladores
propietarios para tarjetas GPU NVIDIA. La opción `modeset`, por ejemplo, permite
controlar si la resolución o la profundidad de pantalla se establecerán en el espacio
del kernel en lugar del espacio del usuario. Agregar `Options nouveau modeset=0` al archivo `/etc/modprobe.d/nouveau.conf` deshabilitará la función del kernel de `modeset`.

Si un módulo está causando problemas, el archivo `/etc/modprobe.d/blacklist.conf` puede
usarse para bloquear la carga del módulo. Por ejemplo, para evitar la carga
automática del módulo `nouveau`, la línea `blacklist nouveau` debe agregarse al
archivo `/etc/modprobe.d/blacklist.conf`. Esta acción es necesaria cuando el módulo
propietario `nvidia` está instalado y el módulo predeterminado `nouveau` debe
ignorarse.

#### Archivos de información y archivos de dispositivo
Los comandos `lspci`, `lsusb` y `lsmod` actúan como interfaz para leer la información
del dispositivo almacenada por el sistema operativo. Este tipo de información se
guarda en archivos especiales en los directorios `/proc` y `/sys`. Estos directorios
son puntos de montaje para sistemas de archivos que no están presentes en una
partición de dispositivo, sino solo en el espacio RAM utilizado por el núcleo del
sistema operativo para almacenar la configuración en tiempo de ejecución y la
información sobre los procesos en ejecución. Dichos sistemas de archivos no están
destinados al almacenamiento convencional de archivos, por lo que se denominan
pseudo-sistemas de archivos y solo existe mientras el sistema se está ejecutando.
El directorio `/proc` contiene archivos con información sobre procesos en ejecución y
recursos de hardware. Algunos de los archivos importantes en `/proc` para inspeccionar
el hardware son:

**`/proc/cpuinfo`**: enumera información detallada sobre las CPU encontradas por el
sistema operativo.

**`/proc/interrupts`**: una lista de números de las interrupciones por dispositivo de
entrada/salida para cada CPU.

**`/proc/ioports`**: enumera los puertos de entrada/salida registrados actualmente
en uso.

**`/proc/dma`**: enumera los canales DMA (acceso directo a memoria) registrados en uso.

Los archivos dentro del directorio `/sys` tienen roles similares a los de `/proc`.
Sin embargo, el directorio `/sys` tiene el propósito específico de almacenar
información del dispositivo y datos del kernel relacionados con el hardware, mientras
que `/proc` también contiene información sobre varias estructuras de datos del kernel,
incluidos los procesos en ejecución y la configuración.

Otro directorio directamente relacionado con dispositivos en un sistema Linux estándar
es `/dev`. Cada archivo dentro de `/dev` está asociado con un dispositivo del sistema,
particularmente de almacenamiento. Un disco duro IDE heredado, por ejemplo, cuando está
conectado al primer canal IDE de la placa base, está representado por el archivo
`/dev/hda`. Cada partición en este disco será identificado por `/dev/hda1`, 
`/dev/hda2` hasta la última partición encontrada.

Los dispositivos extraíbles son manejados por el subsistema *udev*, que crea los
dispositivos correspondientes en `/dev`. El núcleo de Linux captura el evento de
detección de hardware y lo pasa al proceso udev, que luego identifica el dispositivo y
crea dinámicamente los archivos correspondientes en `/dev`, utilizando reglas
predefinidas.

En las distribuciones actuales de Linux, udev es responsable de la identificación y
configuración de los dispositivos que ya están presentes durante el encendido de la
máquina (*coldplug detection*) y los dipositivos identificados mientras el sistema está
en funcionamiento (*hotplug detection*). Udev se basa en *SysFS*, el psuedo sistema
de archivos para la información relacionada con los dipositivos montados en `/sys`.

A medida que se detectan nuevos dispositivos, udev busca una regla coincidente en las
reglas predefinidas almacenadas en el directorio `/etc/udev/rules.d/`. La distribución
proporcionan las reglas más importantes, pero se pueden agregar nuevas para casos
específicos.

#### Dispositivos de almacenamiento
En Linux, los dispositivos de almacenamiento se denominan genéricamente dispositivos de
bloque, porque los datos se leen desde y hacia estos dispositivos en bloques de datos
almacenados en búfer con diferentes tamaños y posiciones. Cada dispositivo de bloque
se identifica mediante un archivo en el directorio `/dev`, con el nombre del archivo
según el tipo de dipositivo (IDE, SATA, SCSI, etc.) y sus particiones. Los dipositivos
de CD/DVD y disquetes, por ejemplo, recibirán sus nombres correspondientes en `/dev`:
una unidad de CD/DVD conectado al segundo canal IDE se identificará como `/dev/hdc`
(`/dev/hda` y `/dev/hda` están reservados para los dispositivos maestro y esclavo en
el primer canal IDE) y una unidad de disquete antigua se identificará como `/dev/fd0`,
`/dev/fd1`, etc.

Desde la versión 2.4 del kernel de Linux en adelante, la mayoría de los dipositivos de
almacenamiento ahora se identifican como si fueran dispositivos SCSI,
independientemente de su tipo de hardware. Los dipositivos de bloque IDE, SSD y USB
tendrán el prefíjo `sd`. Para los discos IDE, se utilizará el prefíjo `sd`, pero se
elegirá la tercera letra dependiendo si la unidad es maestra o esclava (en el primer
canal IDE, el maestro será `sda` y el esclavo será `sdb`). Las particiones se listan
númericamente. Las rutas `/dev/sda1`, `/dev/sda2`, etc. se usan para la primera y
segunda partición del dispositivo de bloque identificado primero y `/dev/sdb1`,
`/dev/sdb2`, etc. identifica las particiones primer y segunda del dispositivo de
bloque identificado en segundo lugar. La excepción a este patrón ocurre con las
tarjetas de memoria (tarjetas SD) y los dipositivos NVMe (SSD conectados la bus PCI
Express). Para las tarjetas SD, las rutas `/dev/mmcblk0p1`, `/dev/mmcblk0p2`, etc.
se utilizan para la primera y segunda partición del dispositivo identificado primero y
`/dev/mmcblk1p1`, `/dev/mmcblk1p2`, etc. utilizado para identificar la primera y
segunda partición del dispositivo identificado en segundo lugar. Los dipositivos NVMe
reciben el prefíjo `nvme`, como en `/dev/nvme0n1p1` y `/dev/nvme0n1p2`.

## Arranque del sistema
Para controlar una máquina, el componente principal del sistema operativo, el núcleo,
debe cargarse mediante un programa llamado *bootloader*, que a su vez lo carga un
firmware preinstalado como BIOS o UEFI. El gesto de arranque se puede personalizar para
pasar párametros al núcleo, como qué partición contiene el sistema de archivos raíz o
en qué modo debe ejecutarse el sistema operativo. Una vez cargado, el núcleo continúa
el proceso de arranque identificando y configurando el hardware. Por último, el núcleo
del sistema operativo llama a la utilidad responsable de iniciar y administrar los
servicios del sistema.

#### BIOS o UEFI
Los procedimientos ejecutados por las máquinas x86 para ejecutar el gestor de arranque
son diferentes si usa BIOS o UEFI, abreviatura de *Basic Input/Output System*, es un
programa almacenado en un chip de memoria no volátil conectado a la placa base, que se
ejecuta cada vez que se enciende la computadora. Este tipo de programa se llama
firmware y su ubicación de almacenamiento es independiente de los otros dispositivos
de almacenamiento que pueda tener el sistema. El BIOS supone que los primeros 440 bytes
en el primer dispositivo corresponden a almacenamiento, siguiendo el orden definido en
la utilidad de configuración de BIOS, la primera etapa del cargador de arranque
(también llamado *bootstrap*). Los primeros 512 bytes de un dispositivo de
almacenamiento se denomina MBR (*Master Boot Record*), de dispositivos de
almacenamiento que utilizan el esquema de partición estándar DOSy, además de la primera
etapa del gestor de arranque, contiene la tabla de particiones. Si el MBR no contiene
los datos correctos, el sistema no podrá arrancar, a menos que se emplee un método
alternativo.

En términos generales, los pasos previos a la operación para un sistema equipado con
BIOS son:

1. El proceso POST (*power-on self-test*) se ejecutan para identificar fallas de dipositivos simples tan pronto se encienda la máquina.
2. El BIOS activa los componentes básicos para cargar el sistema, como salida de video, teclado y medios de almacenamiento.
3. El BIOS carga la primera etapa del gestor de arranque desde el MBR (los primeros 440 bytes del primer dispositivo, como se define en la utilidad de configuración de BIOS).
4. La primera etapa del gestor de arranque llama a la segunda etapa del gestor de arranque, responsable de presentar las opciones de arranque y cargar el núcleo del sistema operativo.

El UEFI, abreviatura de *Unified Extensible Firmware Interface*, difiere del BIOS en
algunos puntos claves. Como BIOS, el UEFI también es un firmware, pero puede
identificar particiones y leer muchos sistemas de archivos que se encuentran en ellas.
EL UEFI no se base en MBR, teniendo en cuenta solo la configuración almacenada en su
memoria no volátil (NVRAM) conectada a la placa base. Estas definiciones indican la
ubicación de los programas compatibles con UEFI, llamados EFI applications, que se
ejecutan automáticamente o se llamarán desde un menú de arranque. Las aplicaciones EFI
pueden ser gestores de arranque, selectores de sistema operativos, herramientas para
el diagnóstico y reparación del sistema, etc. Deben estar en un partición de
dispositivos de almacenamiento convencional y un sistema de archivos compatible. Los
sistemas de archivos compatibles estándar son FAT12, FAT6 y FAT31 para dipositivos de
bloque e ISO-9660 para medios ópticos. Este enfoque permite la implementación de
herramientas muchos más sofisticadas que las posibles con BIOS.

La partición que contiene las particiones EFI se llaman *EFI System Partition* o
simplemente ESP. Esta partición no debe compartirse con otros sistemas de archivos del
sistema, como el sistema de archivos raíz o los sistemas de archivos de datos del
usuario. El directorio EFI en la partición ESP contiene las aplicaciones señaladas por
las entradas guardadas en la NVRAM.

En términos generales, los pasos de arranque del sistema preoperativo en un sistema
como UEFI son:

1. El proceso POST (*power-on self-test*) se ejecuta para identificar fallas del dipositivo simples tan pronto cómo se encienda la máquina.
2. El UEFI activa los componentes básicos para cargar el sistema, como salida de video, teclado y medios de almacenamiento.
3. El firmware de UEFI lee las definiciones almacenadas en NVRAM para ejecutar la aplicación EFI predefinida almacenada en el sistema de archivos de la partición ESP. Por lo general, la aplicación EFI predefinida es un gesto de arranque.
4. Si la aplicación EFI predefinida es un gestor de arranque, cargará el núcleo para iniciar el sistema operativo.

El estándar UEFI también admite una característica llamada *Secure Boot*, que solo
permite la ejecución de aplicaciones EFI firmadas, es decir, aplicaciones EFI
autorizadas por el fabricante de hardware. Esta características aumenta la protección
contra software malicioso, pero puede dificultar la instalación de sistemas operativos
no cubiertos por la garantía del fabricante.

#### El cargador de arranque
El gestor de arranque más popular para Linux en la arquitectura x86 es GRUB
(*Grand Unified Bootloader*). Tan pronto como lo llame el BIOS o UEFI, GRUB muestra una
lista de los sistemas operativos disponibles para arrancar. A veces, la lista no
aparece automáticamente, pero se puede invocar precionando `Shift` mientras el BIOS
está llamando a GRUB. En los sistemas UEFI, la tecla `Esc` debería usarse en su lugar.

Desde el menú GRUB es posible elegir cuál de los núcleos instalados debe cargarse y
pasarle los parámetros. La mayoría de los parámetros del núcleo siguen el patrón
`option=value`. Algunos de los parámetros de Linux más útiles son:

**`acpi`**: habilita/deshabilita el soporte ACPI. `acpi=off` deshabilitará
compatibilidad con ACPI.

**`init`**: establece un iniciador de sistema operativo. Por ejemplo, `init=/bin/bash`
establecerá shell Bash como iniciados. Esto significa que se iniciará una sesión de
shell justo después del proceso de arranque del kernel.

**`systemd.unit`**: establece un *systemd* para que se active. Por ejemplo,
`systemd.unit=graphical.target`. Systemd también acepta los niveles de ejecución
numéricos definidos para SysV. Para activar el nivel de ejecución1, por ejemplo, solo
es necesario incluir el número `1` o la letra `S` (abreviatura de "single") como
parámetro del núcleo.

**`mem`**: establece la cantidad de RAM disponible para el sistema. Este parámetro es
útil cuando se utilizan máquinas virtuales, limitando la cantidad de RAM disponible
para cada una de ellas. El uso de `mem=512` limitará a 512 megabytes la cantidad de
RAM disponible para un sistema virtual en particular.

**`maxcpus`**: Limita el número de procesadores (o núcleos de procesador) visibles para
el sistema en máquinas simétricas multiprocesador. También es útil en máquinas
virtuales. Un valor de `0` desactiva el soporte para máquinas multiprocesador y tiene
el mismo efecto que el parámetro del núcleo `nosmp`. El parámetro `maxcpus=2` limitará
el número de procesadores disponibles para el sistema operativo a dos.

**`quiet`**: oculta la mayoría de los mensajes de arranque.

**`vga`**: selecciona un modo de video. El parámetro `vga=ask` mostrará una lista de
modos disponibles.

**`root`**: establece la partición raíz, disitinta de la preconfigurada en el gestor
de arranque. Por ejemplo, `root=/dev/sda3`.

**`rootflags`**: opciones de montaje para el sistema de archivos raíz.

**`ro`**: hace que el montaje inicial del sistema de archivos raíz sea de solo lectura.

**`rw`**: permite escribir en el sistema de archivos raíz durante el montaje inicial.

Por lo general, no es necesario cambiar los parámetros del núcleo de Linux, pero puede
ser útill para detectar y resolver problemas relacionados con el sistema operativo.
Los parámetros del núcleo del sistema operativo deben agregarse al archivo
`/etc/default/grub` en la línea `GRUB_CMDLINE_LINUX` para que sean persistenten durante
los reinicios. Se debe generar un nuevo archivo de conficuración para el gestor de
arranque cada vez que `/etc/default/grub` cambie, lo cual se logra mediante el comando
`grub-mkconfig -o /boot/grub/grub.cfg`. Una vez que el sistema operativo se está
ejecutando, los parámetros del núcleo utilizados para cargar la sesión actual están
disponibles en el archivo `/proc/cmdline`.

#### Inicialización del sistema
Además del núcleo, el sistema operativo depende de otros componentes que proporcionan
las características esperadas. Muchos de estos componentes se cargan durante el proceso
de inicialización del sistema, que varía desde simples scripts de consola hasta
programas de servicio más complejos. Las secuencias de comandos a menudo se utilizan
para realizar tareas de corta duración que se ejecutarán y finalizarán durante el
proceso de inicialización del sistema. Los servicios, también conocidos como demonios,
pueden estar activos todo el tiempo, ya que pueden ser responsables de los aspectos
intrínsecos del sistema operativo.

La diversidad de formas en que los scripts de inicio y los demonios con las
características más diferentes se pueden integrar en una distribución de Linux es
enorme, un hecho que historicamente obstaculizó el desarrollo de una solución única
que cumplieran con las expectativas de los mantenedores y usuarios de todas las
distribuciones de Linux. Sin embargo, cualquier herramienta que los encargados de las
distribuciones hayan elegido para realizar esta función al menos podrá iniciar, detener
y reiniciar los servicios del sistema. El sistema mismo suele realizar estas acciones
después de una actualización de software, por ejemplo, pero el administrador del
sistema casi siempre necesitará reiniciar manualmente el servicio de realizar modificaciones en su archivo de configuración.

También es conveniente que un administador de sistemas pueda activar un conjunto
particular de demonios, dependiendo de las circunstancias. Debería ser posible, por
ejemplo, ejecutar solo un conjunto mínimo de servicios para realizar tareas de
mantenimiento del sistema.

La inicialización del sistema operativo comienza cuando el gestor de arranque carga el
núcleo en la RAM. Luego, el núcloe se hará cargo de la CPU y comenzará a detectar y
configurar los aspectos fundamentales del sistema operativo, como la configuración
básica de los dispositivos y el direccionamiento de la memoria.

El núcleo del sistema operativo abrirá el *initramfs* (*initial RAM filesystem*).
Initramfs es un archivo que contiene un sistema de archivos utilizados como un sistema
de archivos raíz temporal durante el proceso de arranque. El objetivo principal de un
archivo initramfs es proporcionar los módulos necesarios para que el núcleo pueda
acceder al sistema de archivos raíz "real" del sistema operativo.

Tan pronto como el sistema de archivos raíz esté disponible, el núcleo montara todos
los sistemas de archivos configurados en `/etc/fstab` y luego ejecutara el primer
programa. una utilidad llamada `init`. El programa `init` es responsable de ejecutar
todos los scripts de inicialización y demonios del sistema. Existen implementaciones
distintas de tales iniciadores de sistemas aparte del init tradicional, como `systemd`
y `Upstart`. Una vez que se carga el programa init, initramfs se elimina de la RAM.

**SysV standard**: un administrador de servicios basado en el estándar *SysVinit*
controla qué demonios y recursos estarán disponiles empleando el concepto de 
*runlevels*. Los niveles de ejecución están numerados del 0 al 6 y están diseñados por
los encargados de la distribución para cumplir con propósitos específicos. Las únicas
definiciones de nivel de ejecución compartidas entre todas las distriuciones son los
niveles de ejecución 0, 1 y 6.

**systemd**: systemd es un administrador moderno de sistemas y servicios con una capa
de compatibilidad para los comandos y niveles de ejecución de SysV. systemd tiene una
estructura concurrente, emplead sockets y D-Bus para la activación de los servicios,
ejecución de demonios a demanda, monitoreo de procesos con cgroups, snapshot support,
recuperación se sesión del sistema, control de punto de montaje y un control de
servicio basado en la dependencia. En los últimos años, la mayoría de las principales
distribuciones de Linux han adapatado gradualmente systemd como su administrador de
sistema predeterminado.

**Upstart**: al igual que systemd, Upstart es un sustituto de init. El objetivo de
Upstart es acelerar el proceso de arranque paralelizando el proceso de carga de los
servicios del sistema. Upstart fue utilizado por distribuciones basadas en Ubuntu
en versiones anteriores, pero hoy dio paso a systemd.

#### Inspección de inicialización
Pueden ocurrir errores durante el proceso de arranque, pero pueden no ser tan críticos
para detener completamente el sistema operativo. No obstante, estos errores pueden
comrometer el comportamiento esperado del sistema. Todos los errores pueden comprometer
el comportamiento esperado del sistema. Todos los errores dan como resultado mensajes
que pueden usarse para futuras investigaciones, ya que contienen información valiosa
sobre cuándo y cómo ocurrió el error. Incluso cuando no se generan mensajes de error,
la información recopilada durante el proceso de arranque puede ser útil para fines de
ajuste y configuración.

El espacio de memoria donde el kernel almacena sus mensajes, incluidos sus mensajes de
arranque, se llamada *kernel rinf buffer*. Los mensajes se guardan en el búfer del
anillo del núcleo incluso cuando no se muestran durante el proceso de inicialización,
como cuando se muestra una animación en su lugar. Sin embargo, el buffer del anillo
del núcleo pierde todos los mensajes cuando es sistema está apagado o al ejecutar el
comando `dmesg --clear`. Sin opciones, el comando `dmsesg` muestra los mensajes
actuales en el búfer del núcleo:
```bash
dmesg
[
5.262389
] EXT4-fs (sda1): mounted filesystem with ordered data mode. Opts:
(null)
[ 5.449712 ] ip_tables: (C) 2000-2006 Netfilter Core Team
[ 5.460286 ] systemd[1]: systemd 237 running in system mode.
[ 5.480138 ] systemd[1]: Detected architecture x86-64.
[ 5.481767 ] systemd[1]: Set hostname to <torre>.
[ 5.636607 ] systemd[1]: Reached target User and Group Name Lookups.
[ 5.636866 ] systemd[1]: Created slice System Slice.
[ 5.637000 ] systemd[1]: Listening on Journal Audit Socket.
[ 5.637085 ] systemd[1]: Listening on Journal Socket.
[ 5.637827 ] systemd[1]: Mounting POSIX Message Queue File System...
[ 5.638639 ] systemd[1]: Started Read required files in advance.
[ 5.641661 ] systemd[1]: Starting Load Kernel Modules...
[ 5.661672 ] EXT4-fs (sda1): re-mounted. Opts: errors=remount-ro
[ 5.694322 ] lp: driver loaded but no devices found
[ 5.702609 ] ppdev: user-space parallel port driver
[ 5.705384 ] parport_pc 00:02: reported by Plug and Play ACPI
[ 5.705468 ] parport0: PC-style at 0x378 (0x778), irq 7, dma 3
[PCSPP,TRISTATE,COMPAT,EPP,ECP,DMA]
[ 5.800146 ] lp0: using parport0 (interrupt-driven).
[ 5.897421 ] systemd-journald[352]: Received request to flush runtime journal
from PID 1
```
La salida de `dmseg` puede tener cientos de líneas, por lo que la lista anterior
contiene solo el extracto que muestra el núcleo que llama al administrador del servicio
systemd. Los valores al comienzo de las líneas son la cantidad de segundos relativos
al inicio de la carga del núcleo.

En sistemas basados en systemd, el comando `journalctl` mostrará los mensajes de
inicialización con la opción `-b`, `--boot`, `-k` o `--dmesg`. El comando
`journalctl --list-boots` muestra una lista de números de arranque relativos al
arranque actual, su hash de identifiación y las marcas de tiempo del primero y el
último mensaje correspondientes:
```bash
journalctl --list-boots
-4 9e5b3eb4952845208b841ad4dbefa1a6 Thu 2019-10-03 13:39:23 -03—Thu 2019-10-03
13:40:30 -03
-3 9e3d79955535430aa43baa17758f40fa Thu 2019-10-03 13:41:15 -03—Thu 2019-10-03
14:56:19 -03
-2 17672d8851694e6c9bb102df7355452c Thu 2019-10-03 14:56:57 -03—Thu 2019-10-03
19:27:16 -03
-1 55c0d9439bfb4e85a20a62776d0dbb4d Thu 2019-10-03 19:27:53 -03—Fri 2019-10-04
00:28:47 -03
0 08fbbebd9f964a74b8a02bb27b200622 Fri 2019-10-04 00:31:01 -03—Fri 2019-10-04
10:17:01 -03
```
Los registros de inicialización anteriores también se mantienen en sistemas basados en
systemd, por lo que los mensajes de sesiones anteriores del sistema operativo aún se
pueden inspeccionar. Si se proporcionan las opciones `-b` o `--boot=0`, se mostrarán
los mensajes para el arranque actual. Las opciones `-b -1` o `--boot=-1`, mostrarán
mensajes de la inicialización anterior. Las opciones `-b -2` o `--boot=-2`, mostrarán
los mensajes de la inicialización antes de eso y así sucesivamente. El siguiente
extracto muestra el llamado del sistema operativo al administrador del servicio
systemd para el último proceso de inicialización:
```bash
journalctl -b 0
oct 04 00:31:01 ubuntu-host kernel: EXT4-fs (sda1): mounted filesystem with ordered
data mode. Opts: (null)
oct 04 00:31:01 ubuntu-host kernel: ip_tables: (C) 2000-2006 Netfilter Core Team
oct 04 00:31:01 ubuntu-host systemd[1]: systemd 237 running in system mode.
oct 04 00:31:01 ubuntu-host systemd[1]: Detected architecture x86-64.
oct 04 00:31:01 ubuntu-host systemd[1]: Set hostname to <torre>.
oct 04 00:31:01 ubuntu-host systemd[1]: Reached target User and Group Name Lookups.
oct 04 00:31:01 ubuntu-host systemd[1]: Created slice System Slice.
oct 04 00:31:01 ubuntu-host systemd[1]: Listening on Journal Audit Socket.
oct 04 00:31:01 ubuntu-host systemd[1]: Listening on Journal Socket.
oct 04 00:31:01 ubuntu-host systemd[1]: Mounting POSIX Message Queue File System...
oct 04 00:31:01 ubuntu-host systemd[1]: Started Read required files in advance.
oct 04 00:31:01 ubuntu-host systemd[1]: Starting Load Kernel Modules...
oct 04 00:31:01 ubuntu-host kernel: EXT4-fs (sda1): re-mounted. Opts:
commit=300,barrier=0,errors=remount-ro
oct 04 00:31:01 ubuntu-host kernel: lp: driver loaded but no devices found
oct 04 00:31:01 ubuntu-host kernel: ppdev: user-space parallel port driver
oct 04 00:31:01 ubuntu-host kernel: parport_pc 00:02: reported by Plug and Play
ACPI
oct 04 00:31:01 ubuntu-host kernel: parport0: PC-style at 0x378 (0x778), irq 7, dma
3 [PCSPP,TRISTATE,COMPAT,EPP,ECP,DMA]
oct 04 00:31:01 ubuntu-host kernel: lp0: using parport0 (interrupt-driven).
oct 04 00:31:01 ubuntu-host systemd-journald[352]: Journal started
oct 04 00:31:01 ubuntu-host systemd-journald[352]: Runtime journal
(/run/log/journal/abb765408f3741ae9519ab3b96063a15) is 4.9M, max 39.4M, 34.5M free.
oct 04 00:31:01 ubuntu-host systemd-modules-load[335]: Inserted module 'lp'
oct 04 00:31:01 ubuntu-host systemd-modules-load[335]: Inserted module 'ppdev'
oct 04 00:31:01 ubuntu-host systemd-modules-load[335]: Inserted module 'parport_pc'
oct 04 00:31:01 ubuntu-host systemd[1]: Starting Flush Journal to Persistent
Storage...
```
La inicialización y otros mensajes emitidos por el sistema operativo se almacenan en
archivos dentro del directorio `/var/log/`. Si ocurre un error crítico y el sistema
operativo no puede continuar el proceso de inicialización después de cargar el kernel
y el initramfs, se podría usar medio de arranque alternativo para iniciar el sistema
y acceder al sistema de archivos correspondiente. Luego, los archivos almacenados en
`/var/log/` pueden buscarse por posibles razones que causan la interrupción del proceso
de arranque. Las opciones `-D` o `--directory` del comando `journalctl` se pueden usar
para leer mensajes de registro en directorios que no sean `/var/log/journal`, que es
la ubicación predeterminada para los mensajes de registro de systemd. Como los mensajes
de registro de systemd se almacena en texto sin formato, se requiere el comando
`journalctl` para leerlos.

pg 47
