# Linux 101

## Arquitectura del sistema
#### Activación de dispositivos
la utilidad de configuración del sistema se presenta después de presionar una tecla
especifica cuando se enciende la computadora. La tecla que debe presionar varía de un
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
numéricos definidos para SysV. Para activar el nivel de ejecución 1, por ejemplo, solo
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
*runlevel*. Los niveles de ejecución están numerados del 0 al 6 y están diseñados por
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

## Cambiar los niveles de ejecución/objetivos de arranque y apagar y reiniciar el sistema
Una característica común entre los sistemas operativos que siguen los principios de
diseño de Unix es el empleo de procesos separados para controlar disitintas funciones
del sistema. Estos procesos, llamados daemons (o, más generalmente, services), también
son responsables de las características extendidas subyacentes al sistema operativo,
como los servicios de aplicaciones de red (servidor HTTP, intercambio de archivos,
correo electrónico, etc.), bases de datos, configuración sobre demanda, etc. Aunque
Linux utiliza un núcleo monolítico, muchos aspectos de bajo nivel del sistema operativo
se ven afectados por demonios, como el equilibrio de carga y la configuración de
firewall.

Los demonios (daemons) que deberían estar activos dependen del propósito del sistema.
El conjunto de demonios activos también deben poder modificarse en tiempo de ejecución,
de modo que los servicios se puedan iniciar y detener sin tener que reiniciar todo el
sistema. Para poder abordar este problema, cada distribución principal de Linux ofrece
alguna forma o utilidad de administración de servicios para controlar el sistema.

Los servicios pueden ser controlados por scripts de shell o por un programa y sus
archivos de configuración compatibles. El primer método lo implementa el estándar
*SysVinit*, también conocido como System V o simplemente SysV. El segundo método es
implementado por systemd y Upstart. Historicamente, los administradores de servicios
basados en SysV fueron los más utilizados por las distribuciones Linux. Hoy en día,
los administradores de servicos basados en sistemas se encuentran con mayor frecuencia
en la mayoría de las distribuiciones de Linux. El administrador de servicios es el
primer programa lanzado por el núcleo durante el proceso de arranque, por lo que su PID
(número de identificación de proceso) siempre es `1`.

#### Estándar SysVinit
Un administrador de de servicios basado en el estándar Sysvinit proporcionará conjuntos
predefinidos del estado del sistema, llamados *runlevels*, y sus correspondientes
archivos de script se servico para ser ejecutados. Los niveles de ejecución están
numerados de `0` a `6`, y generalmente se asignan a los siguientes propósitos.

**Runlevel 0**: apagado del sistema.

**Runlevel 1, s o usuario único**: modo de usuario único, sin red y otras capacidades
no esenciales (modo de mantenimiento).

**Runlevel 2, 3 y 4**: modo multiusuario. Los usuarios pueden iniciar sesión por
consola o por red. Los niveles de ejecución 2 y 4 no se usan con frecuencia.

**Runlevel 5**: modo multiusuario. Es equivalente a 3, más el inicio de sesión en modo
gráfico.

**Runlevel 6**: reinicio del sistema.

El programa responsable de administrar los niveles de ejecución y los demonios/recursos
asociados es `/sbin/init`. Durante la inicialización del sistema, el proragama `init`
identifica el nivel de ejecución solicitado, definido por un parámetro del núcleo del
sistema operativo o en el archivo `/etc/initab` , y carga los scripts asociados que
se enumeran allí para el nivel de ejecución dado. Cada nivel de ejecución puede tener
muchos archivos de servico asociados, generalmente scripts en el directorio
`/etc/init.d/`. Como no todos los niveles de ejecución son equivalentes a través de
diferentes distribuciones de Linux, también se puede encontrar una breve descripción
del propósito del nivel de ejecución en las distribuciones basadas en SysV.

La sintaxis del archivo `/etc/initab` usa este formato:

    id:runlevel:action:process

El `id` es un nombre genérico de hasta cuatro caracteres de longitud utilizados para
identificar la entrada. La entrada `runlevel` es una lista de números de niveles para
los que se deben ejecutar una acción específica. El término `action` define cómo `init`
ejecutará el proceso indicado por el término `process`. Las acciones diponibles son:

**`boot`**: el proceso se ejecutará durante la inicialización del sistema. El campo
`runlevel` se ignora.

**`bootwait`**: el proceso se ejecutará durante la inicialización del sistema e `init`
esperará hasta que termine para continuar. El campo `runlevel` se ignora.

**`sysinit`**: el proceso se ejecutará después de la inicialización del sistema,
independientemente del nivel de ejecución. El campo `runlevel` se ingora.

**`wait`**: el proceso se ejecutará para los niveles de ejecución dados e `init`
esperará hasta que termine para continuar.

**`respawn`**: el proceso se reiniciará si finaliza.

**`ctrlaltdel`**: el proceso se ejecutará cuando el proceso init reciba la señal
`SIGINT`, que activará cuando se presione la secuencia de teclas `Ctrl + Alt + Supr`.

El nivel de ejecución predeterminado, el que se elegirá si no se proporciona otro como
parámetro del núcleo, también se defiene en `/etc/inittab`, en la entrada
`id:x:initdefault`. La `x` es el número del nivel de ejecución predeterminado. Este
número nunca debe ser `0` o `6`, ya que provocaría que el sistema se apague o reinicie
tan pronto y como finalice el proceso de arranque. A continuación se muestra un archivo
típico `/etc/inittab`:
```bash
# Nivel de ejecución predeterminado
id:3:initdefault:
# Script de configuración ejecutado durante el arranque
si::sysinit:/etc/init.d/rcS
# Acción tomada en el nivel de ejecución S (usuario único)
~:S:wait:/sbin/sulogin
# Configuración para cada nivel de ejecución
l0:0:wait:/etc/init.d/rc 0
l1:1:wait:/etc/init.d/rc 1
l2:2:wait:/etc/init.d/rc 2
l3:3:wait:/etc/init.d/rc 3
l4:4:wait:/etc/init.d/rc 4
l5:5:wait:/etc/init.d/rc 5
l6:6:wait:/etc/init.d/rc 6
# Acción tomada sobre teclado ctrl+alt+del
ca::ctrlaltdel:/sbin/shutdown -r now
# Habilitar consolas para los niveles de ejecución 2 y 3
1:23:respawn:/sbin/getty tty1 VC linux
2:23:respawn:/sbin/getty tty2 VC linux
3:23:respawn:/sbin/getty tty3 VC linux
4:23:respawn:/sbin/getty tty4 VC linux
# Para el nivel de ejecución 3, también habilite serial
# terminales ttyS0 y ttyS1 (módem) consolas
S0:3:respawn:/sbin/getty -L 9600 ttyS0 vt320
S1:3:respawn:/sbin/mgetty -x0 -D ttyS1
```
El comando `telinit q` debe ejecutarse cada vez que de modifica el archivo 
`/etc/inittab`. El argumento `q` (o `Q`) le dice a init que vuelva a cargar su
configuración. Este paso es importante para evitar que el sistema se detenga debido
a una configuración incorrecta en `/etc/inittab`.

Los scripts utilizados por `init` para configurar cada nivel de ejecución se alamcena
en el directorio `/etc/init.d/`. Cada nivel de ejecución tiene un directorio asociado
en `/etc/`, llamado `/etc/rc0.d/`, `/etc/rc1.d`, `/etc/rc2.d`, etc., con los scritpts
eso debería ejecutarse cuando comienze el nivel de ejecución, los archivos de esos
directorios son enlaces simbólicos a los scripts reales en `/etcinit.d/`. Además, la
primera letra del nombre de archivo del enlace en el directorio del nivel de ejecución
indica si el servicio debe iniciarse o terminarse para el nivel de ejecución
correspondiente. El nombre de archivo de un enlace que comienza con la letra `K`
determina que el servicio se eliminará al ingresar el nivel de ejecución (kill).
Comenzando con la letra `S`, el servicio se iniciará al ingresar el nivel de ejecución
(inicio). El directorio `/etc/rc1.d/`, por ejemplo, tendrá muchos enlaces a los scripts
de red que comienzan con la letra `K`, considerando que el nivel de ejecución `1`
corresponde a un solo usuario, sin conectividad de red.

El comando `runlevel` muestra el nivel de ejecución actual para el sistema. El comando
`runlevel` muestra dos valores, el primero es el nivel de ejecución anterior y el
segundo es el nivel de ejecución actual:
```bash
runlevel
N 3
```
La letra `N` en la salida muestra que el nivel de ejecución no ha cambiado desde el
último arranque. En el ejemplo, el `runlevel 3` es el nivel de ejecución actual del
sistema.

El mismo programa `init` puede usarse para alternar entre niveles de ejecución en un
sistema en ejecución, sin la necesidad de reiniciar. El comando `telinit` también se
puede usar para alternar entre los niveles de ejecución. Por ejemplo, los comandos
`telinit 1`, `telinit s` o `telinit S` cambiarán el sistema al nivel de ejecución 1.

#### systemd
Actualmente, systemd es el conjunto de herramientas más utilizado para administrar los
recursos y servicios del sistema, que systemd denomina unidades (*units*). Una unidad
consta de nombre, tipo y archivo de configuración correspondiente. Por ejemplo, la
unidad para un proceso de servidor httpd (como el servidor web Apache) será
`httpd.service` en distribuciones basadas en Red Hat, su archivo de configuración
también se llamará `httpd.service` (en Distribuciones basadas en Debian esta unidad
es llamada `apache2.service`).

Hay siete tipos distintos de unidades systemd:

**service**: el tipo de unidad más común, para recursos activos del sistema que se
pueden iniciar, interrumpir y recargar.

**socket**: el tipo de unidad socket puede ser un socket de sistema de archivos o un
socket de red. Todas las unidades de socket tienen una unidad de servicio
correspondiente, cargada cuando el socket recibe una solicitud.

**device**: una unidad de dispositivo está asociada con un dispositivo de hardware
identificado por el núcleo. Un dispositivo solo se tomará como una unidad systemd si
existe una regla udev para este porpósito. Se puede usar una unidad de dispositivo
para resolver dependencias de configuración cuando se detecta cierto hardware, dado
que las propiedades de la regla udev se pueden usar como parámetros para la unidad de
dispositivo.

**mount**: una unidad de montaje es una definición de punto de montaje en el sistema
de archivos, similar a una entrada en `/etc/fstab`.

**automount**: una unidad de montaje automático también es una definición de punto de
montaje en el sistema de archivos, pero se monta automáticamente. Cada unidad de
montaje automático tiene una unidad de montaje correspondiente, que se inicia cuando
se accede al punto de montaje automático.

**target**: una unidad target es una agrupación de unidades, administradas como una
sola unidad.

**snapshot**: una unidad snapshot es un estado guardado del administrador del sistema
(no disponible en todas las distriuciones Linux).

El comando principal para controlar las unidades systemd es `systemctl`. El comando
`systemctl` se usa para ejecutar todas la tareas relacionadas con la activación,
desactivación, ejecución, interrupción, monitoreo de la unidad, etc. Para una unidad
ficticia llamada `unit.service`, por ejemplo, las acciones más comúnes de `systemctl`
serán:

**`systemctl start unit.service`**: inicia una unidad (`unit`).

**`systemctl stop unit.service`**: detiene una unidad (`unit`).

**`systemctl restart unit.service`**: reinicia una unidad (`unit`).

**`systemctl status unit.service`**: muestra el estado de la unidad (`unit`), 
incluyendo si está en ejecución o no.

**`systemctl is-active unit.service`**: muestra *active* si la unidad (`unit`) está
en ejecución o inactiva.

**`systemctl enable unit.service`**: la unidad (`unit`) se cargará durante la
inicialización del sistema.

**`systemctl disable unit.service`**: la unidad (`unit`) no se cargará durante la
inicialización del sistema.

**`systemctl is-enable unit.service`**: verifica si la unidad (`unit`) comienza con el
sistema. La respuesta se almacena en la variable `$?`. El valor `0` indica que `unit`
comienza con el sistema y el valor `1` indica que `unit` no comienza con el sistema.

Si no existen otras unidades con el mismo nombre en el sistema, el sufijo después del
punto se puede quitar. Si, por ejemplo, solo hay una unidad `htppd` de tipo `service`,
entonces solo `httpd` es suficiente como parámetro de unidad para `systemctl`.

El comando `systemctl` también puede controlar *system targets*. La unidad 
`multi-user.taget`, por ejemplo, combina todas las unidades requeridas por el entorno
multiusuario. Es similar al nivel de ejecución número 3 en un sistema que utiliza SysV.

El comando `systemctl isolate` alterna entre diferentes objetivos. Por lo tanto, para
alternar manualmente al objetivo `multiusuario`:

    systemctl isolate multi-user.target

Hay objetivos correspondientes a los niveles de ejecución de SysV, comenzado con
`runlevel0.target` hasta `runlevel6.target`. Sin embargo, systemd no utiliza el archivo
`/etc/inittab`. Para cambiar el objetivo predeterminado del sistema, la opción
`systemd.unit` se puede agregar a la lista de parámetros del núcleo del sistema
operativo. Por ejemplo, para usar `multi-user.target` como objetico estándar, el
parámetro del núcleo del sistema operativo debe ser `systemd.unit=multi-user.taget`.
Todos los parámetros del kernel pueden hacerse persistentes cambiando la configuración
del gestor de arranque.

Otra forma de cambiar el objetivo predeterminado es modificar el enlace simbólico
`/etc/systemd/system/default.target` para que apunte al objetivo deseado. La
redefinición del enlace se puede hacer con el comando `systemctl` por sí mismo:

    systemctl set-default multi-user.target

Del mismo modo, puede determinar cuál es el objetivo de arranque predeterminado de su
sistema con el siguiente comando:

    systemctl get-default

Similar a los sistemas que adoptan SysV, el objetivo predeterminado nunca debe apuntar
a `shutdown.target`, ya que corresponde al nivel de ejecución 0 (apagado).

Los archivos de configuración asociados con cada unidad de pueden encontrar en el
directorio `/lib/systemd/system/`. El comando `systemctl list-unit-files` enumera todas
las unidades disponibles y muestra si están habilitadas para iniciarse cuando se inicia
el sistema. La opción `--type` seleccionará solo las unidades para un tipo dado, como
en `systemctl list-unit-files --type=service` y 
`systemctl list-unit-files --type=target`.

Las unidades activas o unidades que han estado activas durante la sesión actual del
sistema se pueden enumerar con el comando `systemctl -list-units`. Al igual que la
opción `list-unit-files`, el comando `systemctl list-units --type=service`
seleccionará solo unidades de tipo `service` y el comando 
`system list-units --type=target` seleccionará solo unidades del tipo `target`.

systemd también es responsable de desencadenar y responder a eventos relacionados con
a energía del sistema. El comando `systemctl suspend` pondrá el sistema en modo de
bajo consumo, manteniendo los datos actuales en la memoria. El comando
`systemctl hibernate` copiará todos los datos de la memoria al disco, por lo que el
estado actual del sistema se puede recuperar después de apagarlo. Las acciones
asociadas con tales eventos se definen en el archivo `/etc/systemd/logind.conf` o en
archivos separados dentro del directorio `/etc/systemd/logind.conf.d/`. Sin embargo,
esta función systemd solo se puede usar cuando no hay otro administrador de energía
ejecutándose en el sistema, como el demonio `acpid`. El demonio `acpid` es el
principal administrador de energía de Linux y permite ajustes más precisos a las
acciones posteriores a eventos relacionados con la energía, como cerrar la tapa del
portátil, batería baja o niveles de carga de la batería.

#### Upstart
Los scripts de inicialización utilizados por Upstart se encuentran en el directorio
`/etc/init/`. Los servicios del sistema de pueden enumerar con el comando
`initctl list`, que también muestra el estado actual de los servicios y, si está
disponible, su número PID.
```bash
initctl list
avahi-cups-reload stop/waiting
avahi-daemon start/running, process 1123
mountall-net stop/waiting
mountnfs-bootclean.sh start/running
nmbd start/running, process 3085
passwd stop/waiting
rc stop/waiting
rsyslog start/running, process 1095
tty4 start/running, process 1761
udev start/running, process 1073
upstart-udev-bridge start/running, process 1066
console-setup stop/waiting
irqbalance start/running, process 1842
plymouth-log stop/waiting
smbd start/running, process 1457
tty5 start/running, process 1764
failsafe stop/waiting
```
Cada acción de Upstart  tiene un propio comando independiente. Por ejemplo, el
comando `start` puede usarse para iniciar la sexta terminal virtual:

    start tty6

El estado actual de un recurso se puede verificar con el comando `status`:
```bash
status tty6
tty6 start/running, process 3282
```
Y la interrupción de un servicio se realiza con el comando `stop` :

    stop tty6

Uptart no usa el archivo `/etc/inittab` para definir los niveles de ejecución, pero
los comandos heredados `runlevel` y `telinit` todavía pueden usarse para verificar
y alternar entre los niveles de ejecución.

#### Apagado y reinicio
Un comando muy tradicional utilizado para apagar o reiniciar el sistema se llama
`shutdown`. El comando `shutdown` agregara funciones adicionales al proceso de
apagado: notifica automáticamente a todos los usuarios conectados con un mensaje de
advertencia en sus sesiones de shell y se evitan nuevos inicios de sesión. El comando
`shutdown` actúa como un intermediario para los procedimientos SysV o systemd, es
decir, ejecuta la acción seleccionada llamando a la acción correspondiente en el
administrador de servicios adoptado por el sistema.

Después de ejecutar `shutdown`, todos los procesos reciben la señal `SIGTERM`,
seguida de la señal `SIGKILL`, luego el sistema se apaga o cambia su nivel de
ejecución. Por defecto, cuando no se utilizan las opciones `-h` o `-r`, el sistema
alterna al nivel de ejecución 1, es decir, el modo de usuario único. Para cambiar
las opciones predeterminadas para `shutdown`, el comando debe ejecutarse con la
siguiente sintaxis:

    shutdown [option] time [message]

Solo se requiere el parámetro `time`. El parámetro `time` define cuándo se ejecutará
la acción solicitada, aceptando los siguientes formatos:

**`hh:mm`**: este formato especifica el tiempo de ejecución como hora y minutos.

**`+m`**: este formato especifica cuántos minutos esperar antes de la ejecución.

**`now o +0`**: este formato determina la ejecución inmediata.

El parámetro `message` es el texto de advertencia enviado a todas las sesiones de
terminal de los usuarios conectados.

La implementación de SysV permite limitar a los usuarios que podrán reiniciar la
máquina presionando `Ctrl + Alt + Supr`. Esto es posible colocando la opción `-a`
para el comando `shutdown` presente en la línea con respecto a `ctrlaltdel` en el
archivo `/etc/inittab`. Al hacer esto, solo los usuarios cuyos nombres estén en el
archivo `/etc/shutdown.allow` podrán reiniciar el sistema con la combinación de
teclas `Ctrl + Alt + Supr`.

El comando `systemctl` también se puede usar para apagar o reiniciar la máquina en
sistemas que emplean systemd. Para reiniciar el sistema, se debe usar el comando
`systemctl reboot`. Para apagar el sistema, se debe usar el comando 
`systemctl poweroff`. Ambos comandos requieren privilegios de root para ejecutarse,
ya que los usuarios comunes no pueden realizar dichos procedimientos.

No todas las actividades de mantenimiento requieren que el sistema se apague o
reinicie. Sin embargo, cuando es necesario cambiar el estado del sistema al modo de
usuario único, es importanten advertir a los usuarios que han iniciado sesión para
que no sea vean perjudicados por la finalización brusca de sus actividades.

De manera similar a lo que hace el comando `shutdown` cuando se apaga o reinicia el
sistema, el comando `wall` puede enviar un mensaje a las sesiones de terminal de todos
los usuarios conectados. Para hacerlo, el administrador del sistema solo necesita
proporcionar un archivo o escribir directamente el mensaje como parámetros para
ordenar `wall`.

## Instalación de Linux y gestión de paquetes
#### Diseño del esquema de particionado del disco duro
Antes de que un disco pueda ser usado por una computadora, necesita ser particionado.
Una partición es un subconjunto lógico del disco físico, como una cerca "lógica".
Particionar es una forma de compartimentar la información almacenada en el disco,
separando, por ejemplo, los datos del sistema operativo de los datos del usuario.

Cada disco necesita al menos una partición, pero puede tener varias particiones, y la
información en ella se almacena en una tabla de particiones. Esta tabla incluye
información sobre el primer y último sector de la partición y su tipo, así como más
detalles sobre cada partición.

Dentro de cada partición hay un sistema de archivos. El sistema de archivos describe
la forma en que la información se almacena realmente en el disco. Esta información
incluye cómo están organizados los directorios, cuál es la relación entre ellos,
dónde están los datos para cada archivo, etc.

Las particiones no pueden abarcar varios discos. Pero al usar *Logical Volume Manager*
(LVM) se pueden combinar varias particiones, incluso a través de discos, para formar
un único volumen lógico.

Los volúmenes lógicos eliminan las limitaciones de los dispositivos físicos y le
permiten trabajar con "grupos" de espacio en disco que se pueden combinar o distribuir
de una manera mucho más flexible que las particiones tradicionales. LVM es útil en
situaciones en las que necesitaría agregar más espacio a una partición sin tener que
migrar los datos a un dispositivo más grande.

#### Puntos de montaje
Para poder acceder a un sistema de archivos Linux, debe estar montado. Esto significa
adjuntar el sistema de archivos a un punto específico en el árbol del directorio de
su sistema, llamado punto de montaje (*mount point*).

Cuando esté montado, el contenido del sistemas de archivos estará disponible en el
punto de montaje. Por ejemplo, imagine que tiene un partición con los datos personales
de sus usuarios (sus directorios de inicio), que contiene los directorios `/john`,
`/jack` y `/carol`. Cuando se monta en `/home`, el contenido de esos directorios
estará disponible en `/home/john`, `/home/jack` y `/home/carol`.

El punto de montaje debe existir antes de montar el sistema de archivos. No puede
montar una partición en `/mnt/userdata` si este directorio no existe. Sin embargo, si
el directorio si existe y contiene archivos, esos archivos no estarán disponibles
hasta que desmonte el sistema de archivos. Si lista los contenidos del directorio,
verá los archivos almacenados en el sistema de archivos montados, no los contenidos
originales del directorio.

Los sistemas de archivos se pueden montar en cualquier lugar que desee. Sin embargo,
hay algunas buenas prácticas que deben seguirse para facilitar la administración del
sistema.

Tradicionalmente `/mnt` era el directorio en el que se montaban todos los dispositivos
externos y una cantidad de puntos de anclaje preconfigurados para dispositivos
comunes, como unidades de CD-ROM (`/mnt/cdrmo`) y disquetes (`/mnt/floppy`).

Esto ha sido reemplzado por `/media`, que ahora es el punto de montaje predeterminado
para cualquier medio extraíble por el usuario (por ejemplo, discos externos, unidades
flash USB, lectores de tarjetas de memoria, discos ópticos, etc.) conectados al
sistema.

En la mayoría de las distribuciones modernas de Linux y entornos de escritorio, los
dispositivos extraíbles se montan automáticamente en `/media/user/LABEL` cuando se
conecta al sistema, donde `user` es el nombre del usuario y `LABEL` es la etiqueta
del dispositivo. Por ejemplo, una unidad flash USB con la etiqueta `FlashDrive`
conectada por el usuario `john` se montaría en `/media/john/FlashDrive`. La forma en
que se maneja esto es diferente según el entorno de escritorio.

Dicho esto, cada vez que necesite manualmente montar un sistema de archivos, es una
buena práctica montarlo en `/mnt`.

#### Manteniendo las cosas separadas
En Linux, hay algunos directorios que deberían mantenserse en particiones separadas.
Hay muchas razones para esto: por ejemplo, al montar los archivos relacionados con el
gestor de arranque (almacenados en `/boot`) en una partición de arranque, se asegura
de que su sistema aún pueda arrancar en caso de un bloqueo en el sistema de archivos
raíz.

Mantener los directorios personales del usuario (en `/home`) en una partición separada
facilita la reinstalación del sistema sin el riesgo de tocar accidentalmente los datos
del usuario. Mantener los datos relacionados con un servidor web o de base de datos
(generalmente en `/var`) es una partición separada (o incluso en un disco separado)
facilita la administración del sistema si necesita agregar más espacio en disco para
esos casos de uso.

Incluso puede haber razones de rendimiento para mantener ciertos directorios en
particiones separadas. Es posible que desee mantener el sistema de archivos raíz (`/`)
en una unidad SSD rápida y directorios más grandes como `/home` y `/var` en discos
duros más lentos que ofrecen mucho más espacio por una fracción del costo.

#### La partición de arranque (`/boot`)
La partición de arranque contiene archivos utilizados por el gestor de arranque para
cargar el sistema operativo. En sistemas Linux, el gestor de arranque suele ser
GRUB2 o, en sistemas más antiguos, GRUB Legacy. La partición generalmente se monta en
`/boot` y sus archivos se almacena en `/boot/grub`.

Técnicamente, no se necesita un partición de arranque, ya que en la mayoría de los
casos GRUB puede montar la partición raíz (`/`) y cargar los archivos desde un
directorio separado `/boot`.

Sin embargo, puede desear una partición de arranque separada por seguridad 
(garantizando que el sistema se inicie incluso en caso de bloqueo del sistema de
archivos raíz), o si desea utilizar un sistema de archivos que el gestor de arranque
no puede entender en la partición raíz, o si utiliza un método de cifrado o
compresión no compatible.

La partición de arranque suele ser la primera partición del disco. Esto se debe a que
el BIOS original de la PC de IBM definió los discos usando cilindros, cabezas y
sectores (*cylinders, head y sectors* (CHS)), con un máximo de 1024 cilindros, 256
cabezas y 63 sectores, lo que resulta en un tamaño de disco máximo de 528 MB
(504 MB en MS-DOS). Esto significa que nada más allá de esta marca no sería accesible
en los sistemas heredados, a menos que usaran un esquema de direccionamiento de disco
diferente (como *logical Block Addressing*, LBA).

Por lo tanto, para una máxima compatibilidad, la partición de arranque generalmente se
encuentra al comienzo del disco y termina antes del cilindro 1024 (528 MB), asegurando
que pase lo que pase, la máquina siempre podrá cargar el núcleo.

Dado que la partición de arranque solo almacena los archivos que necesita el gestor de
arranque, el disco RAM inicial y las imágenes del núcleo, puede ser bastante pequeño
para los estándares actuales. Un buen tamaño es alrededor de 300 MB.

#### La partición del sistema EFI (ESP)
La *EFI System Partition* (ESP) es utilizado por máquinas basadas en la interfaz de
firmware extensible unificada (*Unified Extensible Firmware Interface* (UEFI)) para
almacenar cargadores de arranque e imágenes del núcleo de los sistemas operativos
instalados.

Esta partición está formateada en un sistema de archivos basado en FAT. En un disco
particionado con una tabla de particiones GUID, tiene un identificador único global
`C12A7328-F81F-11D2-BA4B-00A0C93EC93B`. Si el disco fue formateado bajo el esquema de
particón MBR, la ID de la partición es `0xEF`.

En las máquinas que ejecutan Microsoft Windows, esta partición suele ser la primera en
el disco, aunque esto no es obligatorio. El sistema opeativo crea (o completa) el ESP
después de la instalación, y en un sistema Linux se monta en `/boot/efi`.

#### La partición `/home`
Cada usuario en el sistema tiene un directorio de inicio para almacenar archivos
personales y preferencias, y la mayoría de ellos se encuentran en `/home`. Por lo
general, el directorio de inicio es el mismo que el nombre de usuario, por lo que el
usuario John tendría un directorio en `/home/john`.

Sin embargo, hay excepciones. Por ejemplo, el directorio de inicio para el usuario
raíz es `/root` y algunos servicios del sistema pueden tener usuarios asociados con
directorios de inicio en otros lugares.

No existe una regla para determinar el tamaño de una partición para el directorio
`/home`. Debe tener en cuenta la cantidad de usuarios en el sistema y cómo se
utilizara. Un usuario que solo navega por la web y procesa textos requerirá menos
espacio que uno que trabaje con la edición de video, por ejemplo.

#### Información variable (`/var`)
Este directorio contiene "datos variables", o archivos y directorios en los que el
sistema debe poder escribir durante la operación. Esto incluye registros del sistema
(en `/var/log`), archivos temporales (`/var/tmp`) y datos de aplicaciones de caché
(en `/var/cache`).

`/var/www/html` también es el directorio predeterminado para los archivos de datos del
servidor web Apache y `/var/lib/mysql` es la ubicación predeterminada para los
archivos de base de datos del servidor MySQL. Sin embargo, ambos pueden ser cambiados.

Una buena razón para poner `/var` en una partición separada es la estabilidad. Muchas
aplicaciones y procesos escriben en `/var` y subdirectorios, como `/var/log` o
`/var/tmp`. Un proceso con un comportamiento anormal puede escribir datos hasta que no
quede espacio libre en el sistema de archivos.

Si `/var` está en `/` esto puede desencadenar un estado de emergencia del núcleo del
sistema operativo (Kernel Panic) y corrupción del sistema de archivos, causando una
situación de las que es díficil recuperarse. Pero si `/var` se mantiene en una
partición separada, el sistema de archivos raíz no se verá afectada.

Al igual que en `/home`, no existe una regla universal para determinar el tamaño de
una partición para `/var`, ya que variará con la forma en la que se utiliza el
sistema. En un sistema doméstico, puede tomar solo unos pocos gigabytes. Pero en una
base de datos o servidor web se puede necesitar mucho más espacio. En tales
escenarios, puede ser conveniente colocar `/var` en una partición en un disco
diferente al de la partición raíz, agragando una capa adicional de protección contra
fallas en el disco físico.

#### Partición de intercambio (Swap)
La partición de intercambio se utiliza para intercambiar páginas de memoria RAM a
disco según sea necesario. Esta partición debe ser de un tipo específico y
configurarse con una utilidad adecuada llamada `mkswap` antes de poder usarse.

La partición de intercambio no se puede montar como las demás, lo que significa que no
puede acceder a ella como un directorio normal y echar un vistazo a su contenido.

Un sistema puede tener múltiples particiones de intercambio (aunque esto es poco
común) y Linux también admite el uso de archivos de intercambio en lugar de
particiones, lo que puede ser útil para aumentar rápidamente el espacio de
intercambio cuando sea necesario.

El tamaño de la partición de intercambio es una polémica. Es posible que la antigua
regla de los primeros días de Linux ("dos veces la cantidad de RAM") ya no se aplique
dependiendo de cómo se esté utilizando el sistema y la cantidad de RAM física
instalada.

En la documentación para Red Hat Enterprise Linux 7, Red Hat recomienda lo siguiente:

Cantidad de RAM | Tamaño de intercambio recomendado | Intercambio c/ hibernación
--|--|--
< 2 GB of RAM | 2x cantidad de RAM | 3x cantidad de RAM
2-8 GB of RAM | mismo tamaño RAM | 2x cantidad de RAM
8-64 GB of RAM | al menos 4 GB | 1.5x cantidad de RAM
> 64 GM of RAM | al menos 4 GB | no recomendado

Por supuesto, la cantidad de intercambio puede depender de la carga de trabajo. Si la
máquina está ejecutando un servicio crítico, como una base de datos, un servidor web
o SAP, es aconsejale consultar la documentación de estos servicios para obtener una
recomendación.

#### LMV
Una desventaja de la partición es que el administrador del sistema tiene que decidir
de antemano cómo se distribuirá el espacio disponible en un dispositivo de
almacenamiento. Esto puede presentar algunos desafíos más adelante, si una partición
requiere más espacio de lo planeado originalmente. Por supuesto, las particiones
pueden redimensionarse, pero esto puede no ser posible si, por ejemplo, no hay
espacio libre en el disco.

*Logical Volume Managmente* (LVM) es una forma de virtualización de almacenamiento que
ofrece a los administradores de sistemas un enfoque más flexible para administrar el
espacio en disco que la partición tradicional. El objetivo de LVM es facilitar la
gestión de las necesidades de almacenamiento de sus usuarios finales. La unidad básica
es el *Physical Volume* (PV), que es un dispositivo de bloque en un sistema como una
partición de disco o un arreglo RAID.

Los PV se agrupan en Grupos de volúmenes (VG) que abtraen los dispositivos subyacentes
y se ven como un único dispositivo lógico, con la capacidad de almacenamiento
combinada de los componentes del PV.

Cada volumen en un grupo de volúmenes se subdivide en partes de tamaño fijo llamada
*extents*. Las extensiones en un PV se denominan *Physical Extents* (PE), mientras
que las del volumen lógico son *Logical Extents* (LE). En general, cada extensión
lógica se asigna a una extensión física, pero esto puede cambiar si se utilizan
características como la duplicación de disco.

Los grupos de volúmenes se pueden subdivir en volúmenes lógicos (LV), que funcionan de
forma similar a las particiones pero con más flexibilidad.

El tamaño de un volumen lógico, tal como se especificó durante su creación, esta
definido por el tamaño de las extensiones físicas (4 MB por defecto) multiplicado por
el número de extensiones en el volumen. A partir de esto, es fácil comprender que para
aumentar un Volumen Lógico, por ejemplo, todo lo que el administrador del sistema
tiene que hacer es agregar más extensiones del grupo disponible en el Grupo de
Volúmenes. Del mismo modo, se pueden eliminar extensiones para reducir LV.

Después de crear un volumen lógico, el sistema operativo lo ve como un dispositivo de
bloque normal. Se creará un directorio en `/dev`, nombrado como `/dev/VGNAME/LVNAME`,
donde `VGNAME` es el nombre de grupo de volúmenes y `LVNAME` es el nombre del volumen
lógico.

Estos dispositivos pueden formatearse con el sistema de archivos deseado utilizando
utilidades estándar (como `mkfs.ext4`, por ejemplo) y montarse usando los métodos
habituales, ya sea manualmente con el comando `mount` o automáticamente agregándolos
al archivo `/etc/fstab`.

## Instalar un gestor de arranque
Cuando una computadora se enciende, el primer software que se ejecuta es el cargador
de arranque. Este es un código cuyo único proposito es cargar el núcleo del sistema
operativo y entregarle el control. El núcleo cargara los controladores necesarios,
inicializará el hardware y luego cargará el resto del sistema operativo.

GRUB es el gestor de arranque utilizado en la mayoría de las distribuciones Linux.
Puede cargar el núcleo de Linux u otros sistemas operativos, como Windows, y puede
manejar múltiples imágenes y parámetros del núcleo del sistema operativo como entradas
de menú separadas. La selección del núcleo en el arranque se realiza a través de una
interfaz controlado por teclado, y hay una interfaz de línea de comandos para editar
las opciones y paŕametros de arranque.

La mayoría de las distribuciones de Linux instalan y configuran GRUB (en realidad,
GRUB 2) automáticamente, por lo que un usuario normal no necesita pensar en eso.
Sin embargo, como administrador del sistema, es vital saber cómo controlar el proceso
de arranque para poder recuperar el sistema de una falla de arranque después de una
actualización fallida del núcleo, por ejemplo.

#### GRUB Legacy vs GRUB 2
La versión original de GRUB (*Grand Unified Bootloader*), ahora cononcida como GRUB
legacy se desarrolló en 1995 como parte del proyecto GNU Hurd, y más tarde se adoptó
como el gestor de arranque predeterminado de muchas distribuciones de Linux,
reemplazando alternativas anteriores como LILO.

GRUB 2 es una reescritura completa de GRUB con el objetivo de ser más limpio, más
seguro, más robusto y más potente. Entre las muchas ventajas sobre GRUB Legacy se
encuentra un archivo de configuración mucho más flexible (con muchos más comandos y
sentencias condicionales, similar a un lenguaje de script), un diseño más modular y
una mejor localización/internacionalización.

También hay soporte para temas y menús gráficos de arranque con pantallas de
presentación, la capacidad de arrancar archivos ISO de LiveCD directamente desde el
disco duro, mejor soporte para arquitecturas que no son x86, soporte universal para
UUID (lo que facilita la identificación de discos y particiones) y mucho más.

GRUB Legacy ya no está en desarrollo activo (la última versión fue 0.97, en 2005), y
hoy la mayoría de las principales distribuciones de Linux instalan GRUB 2 como el
gestor de arranque predeterminado. Sin embargo, aún puede encontrar sistemas que
utilicen GRUB Legacy, por lo que es importante saber cómo usarlo y dónde es diferente
de GRUB 2.

#### ¿Dónde se ubica el cargador de arranque?
Históricamente, los discos duros en los sistemas compatibles con PC de IBM se
particionaron utilizando el esquema de particiones MBR, creado en 1982 para IBM
PC-DOS (MD-DOS) 2.0.

En este esquema, el primer sector de 512 bytes del disco se llama *Master Boot Record*
y contiene una tabla que describe las particiones en el disco (la tabla de
particiones) y también el código de arranque, llamado cargador de arranque.

Cuando se enciende la computadora, este código de gestor de arranque mínimo (debido
a restricciones de tamaño) se carga, ejecuta y pasa el control a un cargador de
arranque secundario en el disco, generalmente ubicado en un espacio de 32 KB entre el
MBR y la primera partición, que cargará los sistemas operativos.

En un disco con particiones MBR, el código de arranque para GRUB está instalado en
el MRB. Esto carga y pasa el control a una imagen núcleo instalada entre el MBR y la
primera partición. Desde este punto, GRUB es capaz de cargar el resto de los recursos
necesarios (definiciones de menú, archivos de configuración y módulos adicionales)
desde el disco.

Sin embargo, MBR tiene limitaciones en el número de particiones (originalmente un
máximo de 4 particiones primarias, luego un máximo de 3 particiones primarias con 1
partición extendida subdividida en un número de particiones lógicas) y tamaños de
disco máximos de 2 TB. Para superar estas limitaciones, se creó un nuevo esquema de
particionamiento llamado GPT (*GUID Partition Table*), parte del estándar UEFI
(*Unifided Extensible Firmware Interface*).

Los discos con particiones GPT se pueden usar con computadoras con el BIOS de PC
tradicional o con el firmware UEFI. En máquinas con BIOS, la segunda parte de GRUB
se almacena en una partición especial de arranque del BIOS.

En los sistemas con firmware UEFI, GRUB se cargara mediante el firmware desde los
archivos `grubia32.efi` (para sistemas de 32 bits) o `grubx64.efi` (para sistemas
de 64 bits) desde una partición llamada ESP (*EFI System Partition*).

#### La particion `/boot`
En Linux, los archivos necesarios para el proceso de arranque generalmente se
almacenan en una partición de arranque, montados en el sistema de archivos raíz y
colequialmente denominados `/boot`.

No se necesita una partición de arranque en los sistemas actuales, ya que los
cargadores de arranque como GRUB generalmente pueden montar el sistema de archivos
raíz y buscar los archivos necesarios dentro de un directorio `/boot`, pero es una
buena práctica ya que separa los archivos necesarios para procesos de arranque desde
el resto del sistema de archivos.

Esta partición suele ser la primera en el disco. Esto se debe a que el BIOS original
de la PC de IBM diseño los discos usando Cilindros, Cabezas y Sectores
(*Cylinders, Heads y Sectors* (CHS)), con un máximo de 1024 cilindros, 256 cabezas y
63 sectores, lo que resulta en un tamaño de disco máximo de 528 MB (504 MB sobre
MS-DOS). Esto sifnifica que nada más allá de esta marca no sería accesible, a menos
que se utilizara un esquema de direccionamiento de disco diferente (como LBA,
*Logical Block Addressing*).

Entonces, para una máxima compatibilidad, la partición `/boot` generalmente se
encuentra al comienzo del disco y termina antes del cilindro 1024 (528 MB),
asegurando que la máquina siempre pueda cargar el kernel. El tamaño recomendado para
esta partición en una máquina actual es de 300 MB.

Otras razones para una partición `/boot` separada son el cifrado y la compresión, ya
que algunos métodos pueden ser no compatibles con GRUB 2 todavía, o si se necesita
tener la partición raíz del sistema (`/`) formateada utilizando un sistema de archivos
no compatible.

#### Contenido de la partición de arranque
El contenido de la partición `/boot` puede variar con la arquitectura del sistema o
el cargador de arranque en uso, pero en un sistema basado en x86, generalmente
encontrará los archivos a continuación. La mayoría de estos se nombran con un sufijo
`-VERSION`, donde `-VERSION` es la versión del núcleo de Linux correspondiente.
Entonces, por ejemplo, un archivo de configuración para la versión del núcleo Linux
`4.15.0-65-generic` se llamaría `config-4.15.0-65-generic`.

**Archivo de configuración**: este archivo, generalmente llamado `config-VERSION`,
almacena los parámetros de configuración para el núcleo de Linux. Este archivo se
genera automáticamente cuando se compila o instala un nuevo núcleo y el usuario no
debe modificarlo directamente.

**Mapa del sistema**: este archivo es una tabla de búsqueda que combina nombre de
símbolos (como variables o funciones) con su posición correspondiente en la memoria.
Esto es útil al depurar un tipo de falla del sistema conocida como *kernel panic*, ya
que permite al usuario saber qué variable o función se estaba llamando cuando
ocurrió la falla. Al igual que el archivo de configuración, el nombre suele ser
`System.map-VERSION` (por ejemplo, `System.map-4.15.0-65-generic`).

**Kernel de Linux**: este es el núcleo del sistema operativo propiamente dicho. El
nombre suele ser `vmlinux-VERSION` (por ejemplo, `vmlinux-4.15.0-65-generic`).
También puede encontrar el nombre `vmlinuz` en lugar de `vmlinux`, la `z` al final
significa que el archivo ha sido comprimido.

**Disco RAM inicial**: esto generalmente se llama `initrd.img-VERSION` y contiene un
sistema de archivos raíz mínimo cargado en un disco RAM, que contiene utilidades y
módulos necesarios para que el núcleo pueda montar el sistema de archivos raíz real.

**Archivos relacionados con el cargador de arranque**: en los sistemas con GRUB
instalado, estos generalmente se encuentran en `/boot/grub` e incluyen el archivo de
configuración GRUB (`/boot/grub/grub.cfg`) para GRUB 2 o `/boot/grub/menu.lts` en caso
de GRUB Legacy), módulos (en `/boot/grub/i386-pc`), archivos de traducción (en
`/boot/grub/locale`) y fuentes (en `/boot/grub/fonts`).

#### GRUB 2 - Instalando GRUB 2
GRUB 2 se puede instalar utilizando la utilidad `grub-install`. Si tiene un sistema
que no arranca, necesitara arrancar usando un Live CD o un disco de rescate, averiguar
cuál es la partición de arranque de su sistema, montarlo y luego ejecutar la utilidad.

El primer disco de un sistema suele ser el *boot device* y es posible que necesite
saber si hay una *boot partition* en el disco. Esto se puede hacer con la utilidad
`fdisk`. Para enumerar todas las particiones en el primer disco de su máquina, use:
```bash
fdisk -l /dev/sda
Disk /dev/sda: 111,8 GiB, 120034123776 bytes, 234441648 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x97f8fef5

Device Boot    Start End Sectors Size Id Type
/dev/sda1 * 2048 2000895 1998848 976M 83 Linux
/dev/sda2   2002942 234440703 232437762 110,9G
/dev/sda5   2002944 18008063   16005120 7,6G 82 Linux swap / Solaris
/dev/sda6   18010112 234440703 216430592 103,2G 83 Linux
```
La partición de arranque se identifica con el `*` debajo de la columna de arranque.
En el ejemplo anterior, es `/dev/sda1`.

Ahora, cree un directorio temporal en `/mnt` y monte la partición en él:
```bash
mkdir /mnt/tmp
mount /dev/sda1 /mnt/tmp
```
Luego ejecute `grub-install`, apuntándolo al dispositivo de arranque (no la partición)
y al directorio donde está montada la partición de arranque. Si su sistema tiene una
partición de arranque dedicada, el comando es:

    grub-install --boot-directory=/mnt/tmp /dev/sda

Si está instalado en un sistema que no tiene una partición de arranque, sino solo un
directorio `/boot` en el sistema de archivos raíz, utilice `grub-install`. Entonces,
el comando es:

    grub-install --boot-directory=/boot /dev/sda

#### COnfigurando GRUB 2
El archivo de configuración predeterminado para GRUB 2 es `/boot/grub/grub.cfg`. Este
archivo se genera automáticamente y no se recomienda la edición manual. Para realizar
cambios en la configuración de GRUB, debe editar el archivo `/etc/default/grub` y
luego ejecutar la utilidad `update-grub` para generar un archivo compatible.

Hay algunas opciones en el archivo `/etc/default/grub` que controlan el comportamiento
de GRUB 2, como el kernel predeterminado para arrancar, el tiempo de espera, los
parámetros adicionales de la línea de comandos, etc. Los más importantes son:

**`GRUB_DEFAULT=`**: la entrade de menú predeterminada para arrancar. Puede ser un
valor numérico (como `0`, `1`, etc.), el nombre de una entrada de menú (como `debian`)
o `saved`, que se usa junto con `GRUB_SAVEDEFAULT=`, explicado a continuación. Tenga
en cuenta que las entradas del menú comienzan en cero, por lo que la primera entrada
del menú es `0`, la segunda es `1`, etc.

**`GRUB_SAVEDEFAULT=`**: si esta opción se establece en `true` y `GRUB_DEFAULT=` se
establece en `saved`, entonces la opción de inicio predeterminada siempre será la
última seleccionada en el menú de inicio.

**`GRUB_TIMEOUT=`**: el tiempo de espera, en segundos, antes de que se seleccione la
entrada de menú predeterminada. Si se establece en `0`, el sistema iniciará la entrada
predeterminada sin montrar un menú. Si se establece en `-1`, el sistema esperará
hasta que el usuario seleccione una opción, sin importar cuánto tiempo tarde.

**`GRUB_CMDLINE_LINUX=`**: esto enumera las opciones de línea de comandos que se
agregarán a las entradas para el kernel de Linux.

**`GRUB_CMDLINE_LINUX_DEFAULT=`**: Por defecto, se generan dos entradas de menú para
cada núcleo de Linux, una con las opciones predeterminadas y una entrada para la
recuperación. Con esta opción, puede agregar parámetros adicionales que se agregarán
solo a la entrada predeterminada.

**`GRUB_ENABLE_CRYPTODISK=`**: si se establece en `y`, los comandos como
`grub-mkconfig`, `update-grub` y `grub-install` buscarán discos cifrados y agregarán
los comandos necesarios para acceder a ellos duranete el arranque. Esto desactiva el
arranque automático (`GRUB_TIMEOUT=` con cualquier valor que no sea `-1`) porque se
necesita una contraseña para decifrar los discos antes de que se pueda acceder a
ellos.

#### Administrar entradas de menú
Cuando se ejecuta `update-grub`, GRUB 2 buscará nucleo y sistema operativos en la
máquina y generarán las entradas de menú correspondientes en el archivo
`/boot/grub/grub.cfg`. Se pueden agregar nuevas entradas manualmente a los archivos de
script dentro del directorio `/etc/grub.d/`.

Estos archivos deben ser ejecutables y son procesados en orden numérico por
`update-grub`. Por lo tanto, `05_debina_theme` se procesa antes que `10_linux` y así
sucesivamente. Las entradas de menú personalizadas generalmente se agregan al archivo
`40_custom`.

Las sintaxis básica para una entrada de menú se muestra a continuación:
```bash
menuentry "Default OS" {
set root=(hd0,1)
linux /vmlinuz root=/dev/sda1 ro quiet splash
initrd /initrd.img
}
```
La primea línea siempre comenza con `menuentry` y termina con `{`. El texto entre
comillas se mostrará como la etiqueta de entrada en el menú de arranque de GRUB 2.

El parámatro `set root` define el disco y la partición donde se encuentra el sistema
de archivos raíz para el sistema operativo. Tenga en cuenta que en GRUB 2 los discos
están enumerados desde cero, por lo que `hd0` es el primer disco (`sda` en Linux),
`hd1` el segundo, y así sucesivamente. Las particiones, sim embargo, están
enumeradas a partir de uno. En el ejemplo anterior, el sistema de archivos raíz se
encuentra en el primer disco (`hd0`), la primera partición (`,1`) o `sda1`.

En lugar de especificar directamente el dispositivo y la partición, también puede
hacer que GRUB 2 busque un sistema de archivos con una etiqueta específica o UUID
(*Universally Unique Identifier*). Para eso, utilice el parámetro 
`search --set=root` seguido del parámetro `--label` y la etiqueta del sistema de
archivos para buscar, o `--fs-uuid` seguido del UUID del sistema de archivos.

Puede encontrar el UUID de un sistema de archivos con el siguiente commando:
```bash
ls -l /dev/disk/by-uuid/
total 0
lrwxrwxrwx 1 root root 10 nov
4 08:40 3e0b34e2-949c-43f2-90b0-25454ac1595d ->
../../sda5
lrwxrwxrwx 1 root root 10 nov
4 08:40 428e35ee-5ad5-4dcb-adca-539aba6c2d84 ->
../../sda6
lrwxrwxrwx 1 root root 10 nov 5 19:10 56C11DCC5D2E1334 -> ../../sdb1
lrwxrwxrwx 1 root root 10 nov 4 08:40 ae71b214-0aec-48e8-80b2-090b6986b625 ->
../../sda1
```
En el ejemplo anterior, el UUID para `/dev/sda1` es `ae71b214-0aec-48e8-80b2-090b6986b625`.
Si desea establecerlo como el dispositivo raíz para el GRUB 2, el comendo sería
`search --set=root --fs-uuid ae71b214-0aec-48e8-80b2-090b6986b625`.

Cuando se usa el comando `search`, es común agragar el parámetro `--no-floppy` para
que GRUB no pierda el tiempo buscando disquetes.

La línea `linux` indica dónde se encuentra el núcleo del sistema operativo (en este
caso, el archivo `vmlinuz` en la raíz del sistema de archivos). Después de eso, puede
pasar los parámetros de la líndea de comandos al núcleo del sistema operativo.

En el ejemplo anterior, especificamos la partición raíz (`root=/dev/sda1`) y pasamos
tres parámetros del kernel: la partición raíz debe montarse como solo lectura (`ro`),
la mayoría de los mensajes de registro deben estar deshabilitados (`quiet`) y se debe
mostrar una pantalla de bienvenidad (`splash`).

La línea `initrd` indica dónde se encuentra el disco RAM inicial. En el ejemplo
anterior, el archivo es `initrd.img`, ubicado en la raíz del sistema de archivos.

La última línea de una entrada de menú debe contener solo el carácter `}`.

#### Interactuando con GRUB 2
Al iniciar un sistema con GRUB 2, verá un menú de opciones. Use las teclas de flecha
para seleccionar una opción y `Enter` para confirmar y arrancar la entrada
seleccionada.

> Si ve solo una cuenta regresiva, pero no un menú, presione `Shift` para que aparezca el menú.

Para editar una opción, seleccionéla con las flechas y presiones `E`. Esto motrará
una ventana del editor con el contenido del `menuentry` asociado con esa opción,
como se define en `/boot/grub/grub.cfg`.

Después de editar una opción, escriba `Ctrl + X` o `F10` para arrancar, o `Esc` para
volver al menú.

Para ingresar al shell de GRUB 2, precione `C` en la pantalla del menú (o `Ctrl + C`
en la ventana de edición). Verá un símbolo del sistema como este: `grub>`.

Escriba `help` para ver una lista de todos los comandos disponibles, o presione `Esc`
para salir del shell y volver a la pantalla del menú.

#### Arrancando desde la consola del GRUB 2
Puede usar el shell GRUB 2 para arrancar el sistema en caso de una configuración
incorrecta en una entrada del menú haga que falle.

Lo primero que debes hacer es averiguard dónde está la partición de arranque. Puede
hacerlo con el comando `ls`, que le mostrará una lista de particiones y discos que
GRUB 2 ha encontrado.
```sh
grub> ls
(proc) (hd0) (hd0,msdos1)
```
En el ejemplo anterior, las cosas son fáciles. Solo hay un disco (`hd0`) con solo una
partición: (`hd0`, `msdos1`).

Los discos y particiones enumerados serán diferentes en un sistema. En nuestro ejemplo,
la primera partición de `hd0` se llama `msdos1` porque se particionó utilizando el
esquema de partición MBR. Si se particionara usando GPT, el nombre sería `gpt1`.

Para arrancar Linux, necesitamos un kernel y un disco RAM inicial (initrd). Veamos
el contenido de (`hd0`, `msdos1`):
```sh
grub> ls (hd0,msdos1)/
lost+found/ swapfile etc/ media/ bin/ boot/ dev/ home/ lib/ lib64/ mnt/ opt/ proc/
root/ run/ sbin/ srv/ sys/ tmp/ usr/ var/ initrd.img initrd.img.old vmlinuz cdrom/
```
Puede agregar el parámetro `-l` a `ls` para obtener una lista larga, similar a lo que
obtendría en una terminal Linux. User `Tab` para autocompletar los nombres de disco,
partición y archivo.

Tenga en cuenta que tenemos imágenes de kernel (`vmlinuz`) e initrd (`initrd.img`)
directamente en el directorio raíz. Si no, podríamos verificar el contenido de `/boot`
con `list (hd0,msdos1)/boot/`.

Ahora, configure la partición de arranque:

    grub> set root=(hd0,msdos1)

Cargue el kernel de Linux con el comando `linux`, seguido de la ruta al kernel y la
opción `root=` para decirle al kernel dónde se encuentra el sistema de archivos raíz
para el sistema operativo.

    grub> linux /vmlinuz root=/dev/sda1

Cargue el disco RAM inicial con `initrd`, seguido de la ruta completa al archivo
`initrd.img`:

    grub> initrd /initrd.img

Ahora, inicie el sistema con `boot`.

#### Arranque desde la consola de rescate
En caso de una falla de arranque, GRUB 2 puede cargar un shell de rescate, una
versión simplificada del shell. Lo reconocera mediante el símbolo del sistema, que se
muestra como `grub rescue>`.

El proceso para iniciar un sistema desde esta consola es casi el mismo que se muestra
anteriormente. Sin embargo, deberá cargar algunos módulos GRUB 2 para que todo
funcione.

Después de descubrir cuál es la partición de arranque (con `ls`), use el comando
`set prefix=`, seguido de la ruta completa al directorio que contiene los archivos
GRUB 2.

Usualmente `/boot/grub/`:

    grub rescue> set prefix=(hd0,msdos1)/boot/grub

Ahora, cargue los módulos `normal` y `linux` con el comando `insmod`:
```sh
grub rescue> insmod normal
grub rescue> insmod linux
```
Luego, cargue la partición de arranque con `set root=` como se indicó anteriormente,
cargue el kernel de Linux (con `linux`), el disco RAM inicial (`initrd`) e intente
arrancar con `boot`.

#### GRUB Legacy
#### Instalación de GRUB Legacy desde un sistema en ejecución
Para instalar GRUB Legacy en un disco desde un sistema en ejecución, utilizaremos la
utilidad `grub-install`. El comando básico es `grub-install DEVICE` donde `DEVICE` es
el disco donde desea instalar GRUB Legacy. Un ejeplo sería `/dev/sda`.

    # grub-install /dev/sda

Tenga en cuenta que debe especificar el *device* donde se instalará GRUB Legacy, como
`/dev/sda`, no la partición como en `/dev/sda1`.

Por defecto GRUB copiará los archivos necesarios al directorio `/boot` en el
dispositivo especificado. Si desea copiarlos a otro directorio, use el parámetro
`--boot-directory=`, seguido de la ruta completa a donde se deben copiar los archivos.

#### Instalación de GRUB Legacy desde un GRUB shell
Si no puede iniciar el sistema por algún motivo y necesita reinstalar el GRUb Legacy,
puede hacerlo desde la consola de GRUB en un disco de inicio de GRUB Legacy.

Desde el shell de GRUB (escriba `C` en el menú de arranque para acceder al indicador
`grub>`), el primer paso es configurar el dispositivo de arranque, que contiene el
directorio `/boot`. Por ejemplo, si este directorio está en la primera partición del
primer disco, el comando sería:

    grub> root (hd0,0)

Si no se sabe qué dispositivo contiene el directorio `/boot`, puede pedirle a GRUB
que lo busque con el comando `find`, como se muestra a continuación:
```sh
grub> find /boot/grub/stage1
(hd0,0)
```
Luego, configure la particiónd de arranque como se indicó anteriormente y use el
comando `setup` para instalar GRUB Legacy en el MBR y copie los archivos necesarios
en el disco:

    grub> setup (hd0)

Cuando termine, reinicie el sistema y debería arrancar normalmente.

#### Configuración de entradas y ajustes de menú GRUB Legacy
Las entradas y configuraciones de menú de GRUB Legacy se almacena en el archivo
`/boot/grub/menu.lts`. Este es un archivo de texto simple con una lista de comandos
y parámetros, que pueden editarse directamente con un editor de texto.

Las líneas que comienzan con `#` se consideran comentarios y las líneas en blanco se
ingroran.

Una entrada del menú tiene al menos tres comandos. El primero, `title`, establece el
titulo del sistema operativo en la pantalla del menú. El segundo, `root`, le dice a
GRUB legado desde que dispositivo o partición arrancar.

La tercera entrada, `kernel`, especifica la ruta completa a la imagen del núcleo del
sistema operativo que debe cargarse cuando se selecciona la entrada correspondiente.
Tenga en cuenta que esta ruta es relativa al dispositivo especificado en el
parámetro `root`.

A continuación, un ejemplo simple:
```sh
# This line is a comment
title My Linux Distribution
root (hd0,0)
kernel /vmlinuz root=/dev/hda1
```
A diferencia de GRUB 2, en GRUB Legacy ambos discos y particiones están enumerados
desde cero. Entonces, el comando `root (hd0, 0)` establecerá la partición de
arranque como la primera partición (`0`) del primer disco (`hd0`).

Puede omitir la instrucción `root` si especifica el dispositivo de arranque antes de
la ruta en el comando `kernel`. La sintaxis es las misma, entonces:

    kernel (hd0,0)/vmlinuz root=dev/hda1


Es equivalente a:
```sh
root (hd0,0)
kernel /vmlinuz root=/dev/hda1
```

Ambos cargarán el archivo `vmlinuz` desde el directorio raíz (`/`) de la primera
partición del primer disco (`hd0,0`).

El parámetro `root=/dev/sda1` después del comando `kernel` le dice al kernel de
Linux qué partición debe usarse como sistema de archivos raíz. Este es un parámetro
del núcleo de Linux, no un comando GRUB Legacy.

Es posible que deba especificar la ubicación de la imagen de disco RAM inicial para
el sistema operativo con el parámetro `initrd`. La ruta completa al archivo puede
especificarse como el en parámetro `kernel`, y también puede especificar un
dispositivo o un partición antes de la ruta, por ejemplo:
```sh
# This line is a comment
title My Linux Distribution
root (hd0,0)
kernel /vmlinuz root=/dev/hda1
initrd /initrd.img
```
GRUB Legacy tiene un diseño modular, donde los módulos (generlamente almacenados
como archivos `.mod` en `/boot/grub/i386-pc`) se pueden cargar para agregar funciones
adicionales, como soporte para hardware inusual, sistema de archivos o nuevos
algoritmos de compresión.

Los módulos se cargan utilizando el comando `module` seguido de la ruta completa al
archivo `.mod` correspondiente. Tenga en cuenta que, al igual que los núcleos y las
imágenes `initrd`, esta ruta es relativo al dispositivo especificado en el comando
`root`.

El siguiente ejemplo cargará el módulo `915resolution`, necesario para establecer
correctamente la resolución de framebuffer en sistemas con conjuntos de chips de
video Intel de las series 800 o 900.

    module /boot/grub/i386-pc/915resolution.mod

#### Carga en cadena de otros sistemas operativos
GRUB Legacy se puede usar para cargar sistemas operativos no compatibles, como
Windows, mediante un proceso llamado *chainloading*. GRUB Legacy se cargara primero,
y cuando se selecciona la opción correspondiente, se cargara el gestor de arranque
para el sistema deseado.

Una entrada típica para cargar Windows en cadena se vería como la siguiente:
```sh
# Load Windows
title Windows XP
root (hd0,1)
makeactive
chainload +1
boot
```
Pasemos por cada parámetro. Como antes, `root (hd0,1)` especifica el dispositivo y la
partición donde se encuentra el cargador de arranque para el sistema operativo que
deseamos cargar. En este ejemplo, la segunda partición del primer disco.

**`makeactive`**: establecerá una bandera que indica que esta es una partición
activa. Esto solo funciona en particiones primarias de DOS.

**`chainload +1`**: le dice a GRUB que cargue el primer sector de la partición de
arranque. Aquí es donde generalmente se encuentras los gestores de arranque.

**`boot`**: ejecutará el gestor de arranque y cargará el sistema operativo
correspondiente.

## Gestión de librerías compartidas
Bibliotecas compartidas (*shared libraries*), también conocidas como objetos
compartidos (*shared objects*): partes de código compilado y reutilizable como
funciones o clases, que varios programas utilizan de manera recurrente.

#### Concepto de bibliotecas compartidas
Al igual que sus contrapartes físicas, las bibliotecas de software son colecciones de
código que están destinadas a ser utilizadas por muchos programas diferentes; así
como las bibliotecas físicas guardan libros y otros recursos para ser utilizados
por muchas personas diferentes.

Para construir un archivo ejecutable a partir del código de fuente de un programa,
son necesarios dos pasos importantes. Primero, el compilador convierte el código
fuente den código máquina que se almacena en los llamados *object files*. En segundo
lugar, el *linker* combina los archivos de objetos y los vincula a las bibliotecas
para generar el archivo ejecutable final. Este enlace puedes hacerse *statically*
(estáticamente) o *dynamically* (dinámicamente). Dependiendo del método que
utilicemos, hablaremos de bibliotecas estáticas o, en caso de vinculación dinámica,
de bibliotecas compartidas. Expliquemos diferencias.

**Bibliotecas estáticas**: una biblioteca estática se fusiona con el programa en el
momento del enlace. Una copia del código de la biblioteca se incrusta en el programa
y se convierte en parte de él. Por lo tanto, el programa no tiene dependencias de la
biblioteca en tiempo de ejecución porque el programa ya contiene el código de la
biblioteca. No tender dependencias puede verse como una ventaja, ya que no tiene que
preocuparse por asegurarse que las bibliotecas utilizadas siempre estén disponibles.
En el lado negativo, los programas vinculados estáticamente son más pesados.

**Bibliotecas compartidas (o dinámicas)**: en el caso de las bibliotecas compartidas,
el enlazador simplemente se encarga de que el programa haga referencia a las
bibliotecas correctamente. Sin embargo, el vinculador no copia ningún código de
biblioteca en el archivo del programa. Sin embargo, en tiempo de ejecución, la
biblioteca compartida debe estar disponible para satisfaces las dependencias del
programa. Este es un enfoque económico para administrar los recursos del sistema, ya
que ayuda a reducir el tamaño de los archivos de programa y solo se carga una copia
de la biblioteca en la memoria, incluso cuando es utilizando por varios programas.

#### Convenciones de nomeclatura de archivos de objetos compartidos
El nombre de una biblioteca compartida, también conocida como *soname*, sigue un
patrón que se compone de tres elementos:
- Nombre de la biblioteca (normalmente precedido por `lib`)
- `so` (que significa "objeto compartido")
- Número de versión de la biblioteca

Por ejemplo: `libpthread.so.0`

Por el contrario, los nombres de las bibliotecas estáticas terminan en `.a`, por
ejemplo, `libpthread.a`.

`glibc` (Bibliotecas GNU C) es un buen ejemplo de una biblioteca compartida. En un
sistema Debian GNU/Linux 9.9, su archivos se llama `libc.so.6`. Tales nombres de
archivo bastante generales son normalmente enlaces simbólicos que apuntan al archivo
real que contiene una biblioteca, cuyo nombre contiene el número de versión exacto.
En el caso de `glibc`, este enlace simbólico se ve así:
```sh
ls -l /lib/x86_64-linux-gnu/libc.so.6
lrwxrwxrwx 1 root root 12 feb 6 22:17 /lib/x86_64-linux-gnu/libc.so.6 -> libc-2.24.so
```
Este patrón de hacer referencia a archivos de biblioteca compartida nombrados por una
versión específica por nombres de archivos más generales es una práctica común.

Otros objetos de bibliotecas compartidas incluyen `libreadline` (que permite a los
usuarios editar líneas de comando a medida que se escriben e incluyen soporte para
los modos de edición Emacs y vi), `libcrypt` (que contiene funciones relacionadas con
el cifrado, el hash y la codificación), o `libcurl` (que es una biblioteca de
tranferencia de archivos multiprotocolo).

Las ubicaciones comunes para las bibliotecas compartidas en un sistema Linux son:
- `/lib`
- `/lib32`
- `/lib64`
- `/usr/lib`
- `/usr/local/lib`

> El concepto de bibliotecas compartidas no es exclusivo de Linux. En Windows, por ejemplo, se denominan DLL, que significa *dynamic linked libraries*.

#### Configuración de rutas de bibliotecas compartidas
Las referencias contenidas en los programas vinculados dinánamicamente se resuelve
mediante el vinculador dinámico (`ld.so` o `ld-linux.so`) cuando se ejecuta el
programa. El vinculador dinámico busca biblioteca en varios directorios. Estos
directorios están especificados por la ruta de la biblioteca. La ruta de la
biblioteca se configura en el directorio `/etc`, es decir, en el archivo
`/etc/ld.so.conf` y, más común hoy en día, en archivos que residen en el directorio
`/etc/ld.so.conf.d`. Normalmente, el primero incluye una línea `include` para los
archivos `*.conf` en el segundo:
```sh
cat /etc/ld.so.conf
include /etc/ld.so.conf.d/*.conf
```
El directorio `/etc/ld.so.conf.d/` contiene archivos `*.conf`:
```sh
ls /etc/ld.so.conf.d/
libc.conf x86_64-linux-gnu.conf
```
Estos archivos `*.conf` deben incluir las rutas absolutas a los directorios de las
bibliotecas compartidas:
```sh
cat /etc/ld.so.conf.d/x86_64-linux-gnu.conf
# Multiarch support
/lib/x86_64-linux-gnu
/usr/lib/x86_64-linux-gnu
```
El comando `ldconfig` se encarga de leer estos archivos de configuración, creando
el conjunto de enlaces simbólicos antes mencionados que ayudan a localizar las
bibliotecas individuales y finalmente a actualizar el archivo de caché
`/etc/ld.so.cache`. Por lo tanto, `ldconfig` debe ejecutarse cada vez que se agregan
o actualizan archivos de configuración.

Las opciones útiles para `ldconfig` son:

**`-v, --verbose`**: muestras los números de la versión de la biblioteca, el nombre
de cada directorio y los enlaces que se crean:
```sh
sudo ldconfig -v
/usr/local/lib:
/lib/x86_64-linux-gnu:
libnss_myhostname.so.2 -> libnss_myhostname.so.2
libfuse.so.2 -> libfuse.so.2.9.7
libidn.so.11 -> libidn.so.11.6.16
libnss_mdns4.so.2 -> libnss_mdns4.so.2
libparted.so.2 -> libparted.so.2.0.1
(...)
```
Así podemos ver, por ejemplo, cómo se vincula `libfuse.so.2` con el archivo de objeto
compartido real `libfuse.so.2.9.7`.

**`-p, --print-cache`**: imprime las listas de directorios y bibliotecas candidatas
almacenadas en la caché actual:
```sh
sudo ldconfig -p
1094 libs found in the cache `/etc/ld.so.cache'
libzvbi.so.0 (libc6,x86-64) => /usr/lib/x86_64-linux-gnu/libzvbi.so.0
libzvbi-chains.so.0 (libc6,x86-64) => /usr/lib/x86_64-linux-gnu/libzvbi-
chains.so.0
libzmq.so.5 (libc6,x86-64) => /usr/lib/x86_64-linux-gnu/libzmq.so.5
libzeitgeist-2.0.so.0 (libc6,x86-64) => /usr/lib/x86_64-linux-
gnu/libzeitgeist-2.0.so.0
(...)`
```
Observe como la caché usa el nombre completo del soname en el enlace:
```sh
sudo ldconfig -p |grep libfuse
libfuse.so.2 (libc6,x86-64) => /lib/x86_64-linux-gnu/libfuse.so.2
```
Si hacemos una lista larga de `/lib/x86_64-linux-gnu/libfuse.so.2`, encontraremos la
referencia al archivo de objeto compartido real `libfuse.so.2.9.7` que está
almacenado en el mismo directorio:
```sh
ls -l /lib/x86_64-linux-gnu/libfuse.so.2
lrwxrwxrwx 1 root root 16 Aug 21 2018 /lib/x86_64-linux-gnu/libfuse.so.2 -> libfuse.so.2.9.7
```
Además de los archivos de configuración, la variable de entorno `LD_LIBRARY_PATH` se
puede usar para agregar nuevas rutas para bibliotecas compartidas temporalmente.
Está formado por un conjunto de directorios separados por dos puntos (`:`) donde se
buscan las bibliotecas. Para agregar, por ejemplo, `/usr/local/mylib` a la ruta de la
biblioteca en la sesión de shell actual, puede teclear:

    LD_LIBRARY_PATH=/usr/local/mylib

Ahora puede verificar su valor:
```sh
echo $LD_LIBRARY_PATH
/usr/local/mylib
```
Para agregar `/usr/local/mylib` a la ruta de la biblioteca en la sesión de shell
actual y exportarlo a todos los procesos secundarios generados desde ese shell, debe
usar el siguiente comando:

    export LD_LIBRARY_PATH=/usr/local/mylib

Para eliminar las variables de entorno `LD_LIBRARY_PATH`, simplemente utilice el
siguiente comando:

    unset LD_LIBRARY_PATH

Para que los cambios sean permantentes, usar el siguiente comando:

    export LD_LIBRARY_PATH=/usr/local/mylib

En unos de los script de inicialización de Bash como `/etc/bash.bashrc` o `~/.bashrc`.

#### Buscando las dependencias de un ejecutable en particular
Para buscar las bibliotecas compartidas requeridas por un programa específico, use el
comando `ldd` seguido de la ruta absoluta del programa. El resultado muestra la ruta
del archivo de la biblioteca compartida, así como las direcciones de memoria
hexadecimal en la que se carga:
```sh
ldd /usr/bin/gin
nux-gate.so.1 (0xb7fa0000)
libpcre2-8.so.0 => /lib/i386-linux-gnu/libpcre2-8.so.0 (0xb7ae8000)
libz.so.1 => /lib/i386-linux-gnu/libz.so.1 (0xb7aca000)
libpthread.so.0 => /lib/i386-linux-gnu/libpthread.so.0 (0xb7aa8000)
libc.so.6 => /lib/i386-linux-gnu/libc.so.6 (0xb78bf000)
/lib/ld-linux.so.2 (0xb7fa2000)
```
Del mismo modo, usamos `ldd` para buscar las dependencias de un objeto compartido:
```sh
ldd /lib/x86_64-linux-gnu/libc.so.6
/lib64/ld-linux-x86-64.so.2 (0x00007fbfed578000)
linux-vdso.so.1 (0x00007fffb7bf5000)
```
Con la opción `-u` (o `--unused`) `ldd` imprime las dependencias directas no utilizadas
(si existen):
```sh
ldd -u /usr/bin/git
Unused direct dependencies:
/lib/x86_64-linux-gnu/libz.so.1
/lib/x86_64-linux-gnu/libpthread.so.0
/lib/x86_64-linux-gnu/librt.so.1
```
La razón de las dependencias no utilizadas está relacionado con las opciones utilizadas
por el vinculador al construir el binario. Aunque el programa no necesita una
biblioteca no utilizada, todavía esta vinculado y etiquetado como `NEEDED` en la
información sobre el archivo objeto. Puede investigar esto usando comandos como
`readelf` u `objdump`.

## Gestión de paquetes Debian
Hace mucho tiempo, cuando Linux todavía estaba en su infancia, la forma más común de
distribuir software era un archivo comprimidio (generalmente un archivo `.tar.gz`) con
el código fuente, que usted mismo debía desempacar y compilar.

#### La herramienta de paquetería en Debian (`dpkg`)
La herramienta Debian Package (`dpkg`) es la utilidad esencial para instalar,
configurar, mantener y eliminar paquetes de software en sistemas basados en Debian.
La operación más básica es instalar un paquete `.deb`, que se puede hacer con:

    dpkg -i PACKAGENAME

Donde `PACKAGENAME` es el nombre del archivo `.deb` que desea instalar.

Las actualizaciones de paquetes se manejan de la misma manera. Antes de instalar un
paquete, `dpkg` verificará si ya existe una versión anterior en el sistema. Si es así,
el paquete se actualizará a la nueva versión. Si no, se instalará una copia nueva.

#### Manejo de dependencias
La mayoría de las veces, un paquete puede depender de otro para que funcione. Por
ejemplo, un editor de imágenes puede necesitar bibliotecas para abrir archivos JPEG, u
otra utilidad puede necesitar un kit de herramientas como Qt o GTK para su interfaz de
usuario.

`dpkg` verificará si esas dependencias están instaladas en su sistemas y no podrá
instalar el paquete si no lo están. En este caso, `dpkg` listará qué paquetes faltan.
Sin embargo, no puede resolver dependencias por sí mismo. Depende del usuario
encontrar los paquetes `.deb` con las dependencias correspondientes e instalarlos.

En el siguiente ejemplo, el usuario intenra instalar el paquete de editor de video
OpenShot, pero faltan algunas dependencias:
```sh
dpkg -i openshot-qt_2.4.3+dfsg1-1_all.deb
(Reading database ... 269630 files and directories currently installed.)
Preparing to unpack openshot-qt_2.4.3+dfsg1-1_all.deb ...
Unpacking openshot-qt (2.4.3+dfsg1-1) over (2.4.3+dfsg1-1) ...
dpkg: dependency problems prevent configuration of openshot-qt:
openshot-qt depends on fonts-cantarell; however:
Package fonts-cantarell is not installed.
openshot-qt depends on python3-openshot; however:
Package python3-openshot is not installed.
openshot-qt depends on python3-pyqt5; however:
Package python3-pyqt5 is not installed.
openshot-qt depends on python3-pyqt5.qtsvg; however:
Package python3-pyqt5.qtsvg is not installed.
openshot-qt depends on python3-pyqt5.qtwebkit; however:
Package python3-pyqt5.qtwebkit is not installed.
openshot-qt depends on python3-zmq; however:
Package python3-zmq is not installed.
dpkg: error processing package openshot-qt (--install):
dependency problems - leaving unconfigured
Processing triggers for mime-support (3.60ubuntu1) ...
Processing triggers for gnome-menus (3.32.0-1ubuntu1) ...
Processing triggers for desktop-file-utils (0.23-4ubuntu1) ...
Processing triggers for hicolor-icon-theme (0.17-2) ...
Processing triggers for man-db (2.8.5-2) ...
Errors were encountered while processing:
openshot-qt
```

#### Eliminar paquetes
Para eliminar un paquete, pase el parámetro `-r` a `dpkg`, seguido del nombre del
paquete. por ejemplo, el siguiente comando eliminará el paquete `unrar` del sistema:
```sh
dpkg -r unrar
(Reading database ... 269630 files and directories currently installed.)
Removing unrar (1:5.6.6-2) ...
Processing triggers for man-db (2.8.5-2) ...
```
La operación de eliminación también ejecuta una verificación de dependencias, y un
paquete no se puede eliminar a menos que también se elimine cualquier otro paquete
que dependa de él. Si intenta hacerlo, recibirá un mensaje de error como el siguiente:
```sh
dpkg -r p7zip
dpkg: dependency problems prevent removal of p7zip:
winetricks depends on p7zip; however:
Package p7zip is to be removed.
p7zip-full depends on p7zip (= 16.02+dfsg-6).
dpkg: error processing package p7zip (--remove):
dependency problems - not removing
Errors were encountered while processing:
p7zip
```
Puede pasar varios nombres de paquetes a `dpkg -r`, por lo que se eliminarán todos a la
vez.

Cuando se elimna un paquete, los archivos de configuración se dejan en el sistema. Si
desea eliminar todo lo relacionado con el paquete, use la opción `-P` (purgar) en
lugar de `-r`.

#### Obtener información de paquetes
Para obtener información sobre un paquete `.deb`, como su versión, arquitectura,
mantenedor, dependencias y más, use el comando `dpkg -I`, seguido del nombre de
paquete que desea inspeccionar:
```sh
dpkg -I google-chrome-stable_current_amd64.deb
new Debian package, version 2.0.
size 59477810 bytes: control archive=10394 bytes.
1222 bytes, 13 lines
control
16906 bytes, 457 lines * postinst #!/bin/sh
12983 bytes, 344 lines * postrm #!/bin/sh
1385 bytes, 42 lines * prerm #!/bin/sh
Package: google-chrome-stable
Version: 76.0.3809.100-1
Architecture: amd64
Maintainer: Chrome Linux Team <chromium-dev@chromium.org>
Installed-Size: 205436
Pre-Depends: dpkg (>= 1.14.0)
Depends: ca-certificates, fonts-liberation, libappindicator3-1, libasound2 (>=
1.0.16), libatk-bridge2.0-0 (>= 2.5.3), libatk1.0-0 (>= 2.2.0), libatspi2.0-0 (>=
2.9.90), libc6 (>= 2.16), libcairo2 (>= 1.6.0), libcups2 (>= 1.4.0), libdbus-1-3
(>= 1.5.12), libexpat1 (>= 2.0.1), libgcc1 (>= 1:3.0), libgdk-pixbuf2.0-0 (>=
2.22.0), libglib2.0-0 (>= 2.31.8), libgtk-3-0 (>= 3.9.10), libnspr4 (>= 2:4.9-2~),
libnss3 (>= 2:3.22), libpango-1.0-0 (>= 1.14.0), libpangocairo-1.0-0 (>= 1.14.0),
libuuid1 (>= 2.16), libx11-6 (>= 2:1.4.99.1), libx11-xcb1, libxcb1 (>= 1.6),
libxcomposite1 (>= 1:0.3-1), libxcursor1 (>> 1.1.2), libxdamage1 (>= 1:1.1),
libxext6, libxfixes3, libxi6 (>= 2:1.2.99.4), libxrandr2 (>= 2:1.2.99.3),
libxrender1, libxss1, libxtst6, lsb-release, wget, xdg-utils (>= 1.0.2)
Recommends: libu2f-udev
Provides: www-browser
Section: web
Priority: optional
Description: The web browser from Google
Google Chrome is a browser that combines a minimal design with sophisticated
technology to make the web faster, safer, and easier.
```

#### Listar paquetes instalados y contenido del paquete
Para obtener una lista de cada paquete instalado en su sistema, use la opción
`--get-selections`, como por ejemplo `dpkg --get-selections`. También puede obtener
una lista de cada archivo instalado por un paquete específico pasando el parámetro
`-l PACKAGENAME` a `dpkg`:
```sh
sudo dpkg -L rclone
/usr
/usr/bin
/usr/bin/rclone
/usr/share
/usr/share/doc
/usr/share/doc/rclone
/usr/share/doc/rclone/README.html
/usr/share/doc/rclone/README.txt
/usr/share/man
/usr/share/man/man1
/usr/share/man/man1/rclone.1
```

#### Averiguar qué paquete posee un archivo específico
A veces es posible que necesite averiguar qué paquete posee un archivo específico en su
sistema. Puede hacerlo utilizando la utilidad `dpkg-query`, seguida del parámetro `-S`
y la ruta al archivo en cuestión:
```sh
dpkg-query -S /usr/bin/unrar-nonfree
unrar: /usr/bin/unrar-nonfree
```

#### Reconfigurar paquetes instalados
Cuando instala un paquete, hay un paso de configuración llamado *post-install* donde
se ejecuta un script para configurar todo lo necesario para que el software se ejecute,
como permisos, ubicación de archivos de configuración, etc. Esto también puede generar
algunas preguntas de configuración al usuario para establecer preferencias sobre cómo
se ejecutará el software.

A  veces, debido a un archivo de configuración dañado o con formato incorrecto, es
posible que desee restaurar las configuraciones de un paquete a su estado "funcional".
O puede que desee cambiar las respuestas que dio a las preguntas de configuración
inicial. Para hacer esto, ejecute la utilidad `dpkg-reconfigure`, seguida del nombre
del paquete.

Este programa realizará una copia de seguridad de los archivos de configuración
antiguos, descomprimirá los nuevos en los directorios correctos y ejecutara el script
*post-install* proporcionado por el paquete, como si el paquete se hubiera instalado
por primera vez. Intente reconfigurar el paquete `tzdata`:

    dpkg-reconfigure tzdata

#### Herramienta de paquetería anazada (`apt`)
*Advanced Package Tool* (APT) es un sistema de adminstración de paquetes, que incluye
un conjunto de herramientas, que simplifican enormemente la instalación, actualización,
eliminación y administración de paquetes. APT proporciona características como
capacidades de busqueda avanzada y resolución de dependencias automática.

APT no es un sustituto de `dpkg`. Puede considerarlo como una interfaz (front end), que
optimiza las operaciones y llena los vacíos de la funcionalidad de `dpkg`, como la
resolución de dependencias.

APT trabaja en conjunto con los repositorios de software que contienen los paquetes
disponibles para instalar. Dichos repositorios pueden ser un servidor local o remoto
o disco CD-ROM.

Las distribuciones de Linux, como Debian y Ubuntu, mantienen sus propios repositorios,
y los desarrolladores o grupos de usuarios pueden mantener otros repositorios para
proporcionar software que no está disponible en los principales repositorios de
distribuciones.

Existen muchas utilidades que interactúan con APT, siendo los principales:

**`apt-get`**: se utiliza para descargar, instalar, actualizar o eliminar paquetes del
sistema.

**`apt-cache`**: se utiliza para realizar operaciones, como búsquedas, en el indice de
paquetes.

**`apt-file`**: para buscar archivos dentro de los paquetes.

#### Commands
```sh
# actualizar paquetes
sudo apt-get update
sudo apt-get upgrade
sudo apt update -y && sudo apt upgrade -y

# limpiar cache /var/cache/apt/archives/ y/o /var/cache/apt/archives/partial/
sudo apt clean

# buscar paquetes
sudo apt search PACKAGENAME
```

#### Lista de fuentes
APT utiliza una lista de fuentes para saber de dónde obtener paquetes. Esta lista de
almacena en el archivo `source.list`, ubicado en `/etc/apt`. Este archivo se puede
editar directamente con un editor de texto, o con utilidades gráficas como `aptitude` o
`synaptic`.

Una línea típica dentro de `source.list` se ve así:

    deb http://us.archive.ubuntu.com/ubuntu/ disco main restricted universe multiverse

La sintaxis es tipo de archivo, URL, distribución y uno o más componentes:

**URL**: la URL del repositorio.

**Distribución**: el nombre (o nombre en clave) de la distribución para la que se
proporcionan los paquetes. Un repositorio puede alojar paquetes para múltiples
distribuciones. En el ejemplo anterior, `disco` es el nombre en clave de Ubuntu 19.04
Disco Dingo.

**Componentes**: cada componente representa un conjunto de paquetes. Estos componentes
puden ser diferentes en diferentes distribuciones de Linux. Por ejemplo, en Ubuntu y
derivados, son:

**`main`**: contiene paquetes de código abierto con soporte oficial.

**`restricted`**: contiene software de código cerrado con soporte oficial, como
controladores de dispositivo para tarjetas gráficas, por ejemplo.

**`universe`**: contiene software de código abierto mantenido por la comunidad.

**`multiverse`**: contiene software no compatible, de código cerrado o con patente
gravada.

En Debian, los paquetes principales son:

**`main`**: consiste en paquetes que cumplen con las Directrices de Software libre de
Debian (DSFG), que no dependen de software fuera de esta área para operar. Los
paquetes incluidos aquí se consideran parte de la distribución Debian.

**`contrib`**: contiene paquetes compatibles con DSFG, pero que dependen de otros
paquetes que no están en `main`.

**`non-free`**: contiene paquetes que no son compatibles con DSFG.

**`security`**: contiene actualizaciones de seguridad.

**`backports`**: contiene versiones más recientes de paquetes que están en `main`. El
ciclo de desarrollo de las versiones estables en Debian es bastante largo (alrededor
de dos años), y esto asegura que los usuarios puedan obtener los paquetes más
actualizados sin tener que modificar el repositorio principal `main`.

Para agregar nuevos repositorios de paquetes, simplemente puede agregar la línea
correspondiente (generalmente proporcionado por el responsable del repositorio) al
final de `sources.list`, guarde el archivo y vuelva a cargar el índice del paquete con
`sudo apt update`. Después de eso, los paquetes en el nuevo repositorio estarán
disponibles para la instalación con `sudo apt install <PACKAGENAME>`.

#### El directorio `/etc/apt/sources.list.d/`
Dentro del directorio `/etc/apt/sources.list.d` puede agregar archivos con repositorios
adicionales para ser utilizados por APT, sin la necesidad de modificar el archivo
principal `/etc/apt/sources.list`. Estos son archivos de texto simples, con la misma
sintaxis descrita anteriormente y la extensión de archivo `.list`.

A continuación puede ver el contenido de un archivo llamado
`/etc/apt/sources.list.d/buster-backports.list`:
```sh
deb http://deb.debian.org/debian buster-backports main contrib non-free
deb-src http://deb.debian.org/debian buster-backports main contrib non-free
```

#### Listar el contenido de paquetes y búsqueda de archivos
Una utilidad llamdad `apt-file` pueden usarse para realizar más operaciones en el
índice de paquetes, como listar el contenido de un paquete o encontrar otro paquete
que contenga un archivo específico. Es posible que esta utilidad no esté instalada de
manera predeterminada en un sistema. En este caso, generalmente puede instalarlo:

    sudo apt install apt-file -y

Después de la instalación, deberá actualizar la caché del paquete utilizada para
`apt-file`:

    sudo apt-file update

Esto generalmente toma solo unos segundos. Después de eso, `apt-file` estará listo
para usarse.

Para enumerar el contenido de un paquete, use el parámetro `list` seguido del nombre
del paquete:
```sh
sudo apt-file list rclone
rclone: /usr/bin/rclone
rclone: /usr/share/bash-completion/completions/rclone
rclone: /usr/share/doc-base/rclone
rclone: /usr/share/doc/rclone/MANUAL.html
rclone: /usr/share/doc/rclone/MANUAL.md.gz
rclone: /usr/share/doc/rclone/changelog.Debian.gz
rclone: /usr/share/doc/rclone/changelog.Debian.i386.gz
rclone: /usr/share/doc/rclone/copyright
rclone: /usr/share/doc/rclone/logo_on_light__horizontal_color.svg
rclone: /usr/share/man/man1/rclone.1.gz
```
Puede buscar un archivo en todos los paquetes utilizando el parámetro `search`,
seguido del nombre del archivo. Por ejemplo, si desea saber qué paquete porporciona
un paquete llamado `libSDL2.so`, puede usar:
```sh
sudo apt-file search libSDL2.so
libsdl2-dev: /usr/lib/i386-linux-gnu/libSDL2.so
```
La respuesta es el paquete `libsdl2-dev`, que proporciona el archivo
`/usr/lib/i386-linux-gnu/libSDL2.so`.

La diferencia entre `apt-file` y `dpkg-query` es que `apt-file search` también
buscará paquetes desinstalados, mientras que `dpkg-search` solo puede monstrar
archivos que pertenecen a un paquete instalado.

## Gestión de paquetes RPM y YUM
#### El gestor de paquetes RPM (`rpm`)
El gestor de paquete RPM es la herramienta escencial para administrar paquetes de
software en sistemas basados en Red Hat (o derivados).

#### Instalar, actualizar y eliminar paquetes
La operación más básica es instalar un paquete, que se puede hacer con:

    rpm -i PACKAGENAME

Donde `PACKAGENAME` es el nombre del paquete `.rpm` que desea instalar. Si hay una
versión anterior de un paquete en el sistema, puede actualizar a una versión más
nueva utilizando el parámetro `-U`:

    rpm -U PACKAGENAME

Si no hay instalada una versión anterior de `PACKAGENAME`, se instalará una copia
nueva. Para evitar esto y solo actaulizar un paquete instalado, user la opción `-F`.

En ambas operaciones, puede agregar el parámetro `-v` para obtener una salida
detallada (se muestra más información durante la instalación) y `-h` para obtener
signos hash (`#`) impresos como una ayuda visual para rastrear el progreso de la
instalción. Se pueden combinar varios parámetros en uno, por lo que `rpm -i -v -h`
es lo mismo que `rpm -ivh`.

Para eliminar un paquete instalado, pase el parámetro `-e` (como en "erase") a
`rpm`, seguido del nombre de paquete que desea eliminar:

    rpm -e wget

Si un paquete instalado depende del paquete que se está eliminando, recibirá un
mensaje de error:
```sh
rpm -e unzip
error: Failed dependencies:
/usr/bin/unzip is needed by (installed) file-roller-3.28.1-2.el7.x86_64
```
Para completar la operación, primero deberá eliminar los paquetes que dependen del
que desea eliminar, Puede pasar varios nombre a `rpm -e` para eliminar varios
paquetes a la vez.

#### Manejo de dependencias
La mayoría de la veces, un paquete puede depender de otros para que funcione según lo
previsto. Por ejemplo, un editor de imágenes puede necesitar bibliotecas para abrir
archivos JPG, o una utilidad puede necesitar un kit de herramientas de widgets como
Qt o GTK para su interfaz de usuario.

`rpm` verificará si esas dependencias están instaladas en un sistema y no podrá
instalar el paquete si no lo están. En este caso, `rpm` listará lo que falta. Sin
embargo, no puede resolver dependencias por sí mismo.

En el ejemplo a continuación, el usuario intentó instalar un paquete para el editor de
imágenes GIMP, pero faltaban algunas dependencias:
```sh
rpm -i gimp-2.8.22-1.el7.x86_64.rpm
error: Failed dependencies:
babl(x86-64) >= 0.1.10 is needed by gimp-2:2.8.22-1.el7.x86_64
gegl(x86-64) >= 0.2.0 is needed by gimp-2:2.8.22-1.el7.x86_64
gimp-libs(x86-64) = 2:2.8.22-1.el7 is needed by gimp-2:2.8.22-1.el7.x86_64
libbabl-0.1.so.0()(64bit) is needed by gimp-2:2.8.22-1.el7.x86_64
libgegl-0.2.so.0()(64bit) is needed by gimp-2:2.8.22-1.el7.x86_64
libgimp-2.0.so.0()(64bit) is needed by gimp-2:2.8.22-1.el7.x86_64
libgimpbase-2.0.so.0()(64bit) is needed by gimp-2:2.8.22-1.el7.x86_64
libgimpcolor-2.0.so.0()(64bit) is needed by gimp-2:2.8.22-1.el7.x86_64
libgimpconfig-2.0.so.0()(64bit) is needed by gimp-2:2.8.22-1.el7.x86_64
libgimpmath-2.0.so.0()(64bit) is needed by gimp-2:2.8.22-1.el7.x86_64
libgimpmodule-2.0.so.0()(64bit) is needed by gimp-2:2.8.22-1.el7.x86_64
libgimpthumb-2.0.so.0()(64bit) is needed by gimp-2:2.8.22-1.el7.x86_64
libgimpui-2.0.so.0()(64bit) is needed by gimp-2:2.8.22-1.el7.x86_64
libgimpwidgets-2.0.so.0()(64bit) is needed by gimp-2:2.8.22-1.el7.x86_64
libmng.so.1()(64bit) is needed by gimp-2:2.8.22-1.el7.x86_64
libwmf-0.2.so.7()(64bit) is needed by gimp-2:2.8.22-1.el7.x86_64
libwmflite-0.2.so.7()(64bit) is needed by gimp-2:2.8.22-1.el7.x86_64
```
Depende del usuario encontrar los paquetes `.rpm` con la dependencias correspondientes
e instalarlos. Los administradores de paquetes como `yum`, `zypper` y `dnf` tienen
herramientas que pueden indicar qué paquete proporciona un archivo específico.

#### Obtener información de paquetes
```sh
# obtener información sobre un paquete instalado
rpm -qi unzip
# obtener una lista de los archivos que están dentro de un paquete instalado
rpm -ql unzip
# lo mismo, pero de un paquete no instalado
rpm -qip atom.x86_64.rpm
rmp -qlp atom.x86_64.rpm
```

#### Averiguar qué paquetes posee un archivo específico
Para averiguar qué archivo posee un paquete instalado, use `-qf` seguido de la ruta
completa del archivo:
```sh
rpm -qf /usr/bin/unzip
unzip-6.0-19.el7.x86_64
```
En el ejemplo anterior, el archivo `/usr/bin/unzip` pertenece al paquete
`unzip-6.0-19.el7.x86_64`.

#### YelowDog Updater modificado (YUM)
`yum` se desarrolló originalmente como *Yelow Dog Updater* (YUP), una herramienta para
la gestión de paquetes en la distribución de Yellow Dog Linux. Con el tiempo,
evolucionó para administrar paquetes en otros sistemas basados en RPM, como Fedora,
CentOS, Red Hat Enterprise Linux y Oracle Linux.

Funcionalmente, es similar a la utilidad `apt` en los sistemas basados en Debian,
pudiendo buscar, instalar, actualizar, eliminar paquetes y manejar automáticamente las
dependencias. `yum` se puede usar para instalar un solo paquete o para actualizar un
sistema completo a la vez.

#### Buscar paquetes
Para instalar un paquete, necesita saber su nombre. Para esto, puede realizar una
búsqueda con `yum search PATTERN`, donde `PATTERN` es el nombre del paquete que está
buscando. El resultado es una lista de paquetes cuyos nombres o resúmene contiene
el patrón de búsqueda especificado. Por ejemplo, si necesita una utilidad para
manejar archivos comprimidos 7zip (con la extensión `./z`) puede usar:
```sh
yum search 7zip
Loaded plugins: fastestmirror, langpacks
Loading mirror speeds from cached hostfile
* base: mirror.ufscar.br
* * epel: mirror.globo.com
* * extras: mirror.ufscar.br
* * updates: mirror.ufscar.br
* =========================== N/S matchyutr54ed: 7zip ============================
* p7zip-plugins.x86_64 : Additional plugins for p7zip
* p7zip.x86_64 : Very high compression ratio file archiver
* p7zip-doc.noarch : Manual documentation and contrib directory
* p7zip-gui.x86_64 : 7zG - 7-Zip GUI version
* Name and summary matches only, use "search all" for everything.
```

#### Instalar, actualizar y eliminar paquetes
Para instalar un paquete usando `yum`, use el comando `yum install PACKAGENAME`.
`yum` buscará el paquete y las dependencias correspondientes de un repositorio en
línea e instalará todo en su sistema.
```sh
yum install p7zip
Loaded plugins: fastestmirror, langpacks
Loading mirror speeds from cached hostfile
* base: mirror.ufscar.br
* * epel: mirror.globo.com
* * extras: mirror.ufscar.br
* * updates: mirror.ufscar.br
* Resolving Dependencies
* --> Running transaction check
* ---> Package p7zip.x86_64 0:16.02-10.el7 will be installed
* --> Finished Dependency Resolution
* Dependencies Resolved
* ==========================================================================
* Package
* Arch
* Version
* Repository
* Size
* ==========================================================================
* Installing:
* p7zip
* x86_64
* 16.02-10.el7
* epel
* 604 k
* Transaction Summary
* ==========================================================================
* Install
* 1 Package
* Total download size: 604 k
* Installed size: 1.7 M
* Is this ok [y/d/N]:
```
Para actualizar un paquete instalado, use `yum update PACKAGENAME`. Por ejemplo:

    yum update wget

Si omite el nombre del paquete, puede actualizar cada paquete en el sistema si existen
actualizaciones disponibles.

Para verificar si hay una actualización disponible para un paquete específico, use
`yum check-update PACKAGENAME`. Si omite, `yum` buscará actualizaciones para todos
los paquetes.

Para eliminar un paquete, use `yum remove PACKAGENAME`.

#### Encontrar qué paquete porporciona un archivo específico
```sh
yum whatprovides libgimpui-2.0.so.0
# other
yum whatprovides /etc/hosts
```

#### Obtener información sobre un paquete

    yum info firefox

#### Gestión de repositorios de software
Para `yum`, los repos se enumeran en el directorio `/etc/yum.repos.d/`. Cada
repositorio está representado por un archivo `.repo`, como `CentOs-Base.repo`.

El usuario puede agregar repositorios adicionales agregando un archivo `.repo` en el
directorio mencionado anteriormente, o al final de `/etc/yum.conf`. Sin embargo, la
forma recomendada de agregar o administrar repositorios es con la herramienta
`yum-config-manager`.

Para agregar un repositorio, use el parámetro `--add-repo`, seguido de la URL a un
archivo `.repo`:
```sh
yum-config-manager --add-repo https://rpms.remirepo.net/enterprise/remi.repo
Loaded plugins: fastestmirror, langpacks
adding repo from: https://rpms.remirepo.net/enterprise/remi.repo
grabbing file https://rpms.remirepo.net/enterprise/remi.repo to
/etc/yum.repos.d/remi.repo
repo saved to /etc/yum.repos.d/remi.repo
```
Para obtener una lista de todos los repositorios disponibles, use `yum repolist all`.
Obtendrá una lista similar a esta:
```sh

list all
Loaded plugins: fastestmirror, langpacks
Loading mirror speeds from cached hostfile
* base: mirror.ufscar.br
* * epel: mirror.globo.com
* * extras: mirror.ufscar.br
* * updates: mirror.ufscar.br
* repo id repo name status
* updates/7/x86_64 CentOS-7 - Updates enabled:
* updates-source/7 CentOS-7 - Updates Sources disabled 2,500
```
Los repositorios `disable` serán ignorados al instalar o actualizar el software. Para
habilitar o deshabilitar, use la utilidad `yum-config-manager`, seguido de la
identificación del repositorio.

En el resultado anterior, la identifiación del repositorio se muestra en la primera
columna (`repo id`) de cada línea. Utilice solo la parte anterior a la primera `/`,
por lo que la identificación para el repositorio `CentOS-7 - Updates` es `updates`
y no `/updates/7/x86_64`.

    yum-config-manager --disable updates

El comando anterior deshabilitará el repositorio `updates`. Para vover a habailitarlo
user:

    yum-config-manager --enable updates

#### DNF
`dnf` es la herramienta de administración de paquetes utilizada en Fedora, y es una
bifurcaricación de `yum`. Como tal, muchos de los comandos y parámetros son similares.
```sh
# buscar paquetes
dnf search PATTERN
# obtener información de un paquete
dnf infro PACKAGENAME
# instalar un paquete
dnf install PACKAGENAME
# eliminar un paquete
dnf remove PACKAGENAME
# actualizar un paquete
dnf upgrade PACKAGENAME
# encontrar qué paquete proporciona un archivo específico
dnf provide FILENAME
# obtener una lista de todos los paquetes instalados en el sistema
dnf list --installed
# listar el contenido de un paquete
dnf repoquery -l PACKAGENAME
```

#### Gestión de repositorios de software
Al igual que con `yum` y `zypper`, `dnf` funciona con repositorios de software
(repos). Cada distribución tiene una lista de repositorios predeterminados, y los
administradores pueden agregar o eliminar repositorios según sea necesario.

Para obtener una lista de los repositorios disponibles, use `dnf repolist`. Para
listar solo los repositorios habilitados, agregue la opción `-enabled`, y para
habilitar solo los repositorios deshabilitados, agregue la opción `--disabled`:

    dns repolist

Para agregar una repositorio, use `dnf config-manager add-repo URL`. Para habilitar
un repositorio, use `dnf config-manager --set-enabled REPO_ID`.

Del mismo modo, para deshabilitar un repositorio user `dnf config-manager --set-disabled REPO_ID`.

#### Zypper
`zypper` es la herramienta de gestión de paquetes utilizada en SUSE Linux y OpenSUSE.
En cuanto a las características, es similar a `apt` y `yum`, pudiendo instalar,
actualizar, eliminar con resolución de dependencias automatizada.

#### Actualización de los indices de paquetes
Al igual que otras herramientas de administración de paquetes, `zypper` funciona con
repositorios que contienen paquetes y metadatos. Estos metadatos deben actualizarse
de vez en cuando, para que la utilidad conozca los últimos paquetes disponibles.
Para hacer una actualización, simplemente utilice el siguiente comando:
```sh
zypper refresh
Repository 'Non-OSS Repository' is up to date.
Repository 'Main Repository' is up to date.
Repository 'Main Update Repository' is up to date.
Repository 'Update Repository (Non-Oss)' is up to date.
All repositories have been refreshed.
```
`zypper` tiene una función de actualización automática que se puede habilitar por
repositorio, lo que significa que algunos repositorios pueden actualizarse
automáticamente antes de una consulta o instalación de paquete, y otros pueden
necesitar actualizarse manualmente.
```sh
# buscar paquetes
zypper se gnumeric
# obtener información de un paquete o todos
zypper se -i PACKAGENAME
zypper se -i # obtener información de todos los paquetes del sistema
# instalar un paquete
zypper in unrar # install or in
# actualizar
zypper update
# obtener una lista
zypper list-update
# elminiar
zypper rm unrar
# 
zypper se --provides /usr/lib64/libgimpmodule-2.0.so.0
# información de un paquete
zypper info gimp
# gestión de repos
zypper repos
zypper modifyrepo -d repo-non-oss
zypper modifyrepo -e repo-non-oss
zypper modifyrepo -F repo-non-oss
zypper modifyrepo -f repo-non-oss
```

#### Agregar y quitar repositorios
Para agregar un nuevo repositorio de software para `zypper`, use el operador `addrepo`
seguido de la URL del repositorio y el nombre del repositorio:

    zypper addrepo http://packman.inode.at/suse/openSUSE_Leap_15.1/ packman

Al agregar un repositorio puede habilitar las actualizaciones automáticas con el
parámetro `-f`. Los repositorios agregados están habilitados de manera predetermianda,
pero puede agregar o deshabilitar un repositorio al mismo tiempo utilizando el
parámetro `-d`.

Para eliminar un repositorio, use el operador `removerepo`, seguido del nombrel del
repositorio (Alias):

    zypper removerepo packman

## Linux como sistema virtualizado
Una de las grandes fortalezas de Linux es su versatilidad. Un aspecto de esta
versatilidad es la capacidad de usar Linux como medio para alojar otros sistemas
operativos, o aplicaciones individuales, en un entorno completamente aisaldo y seguro.

#### Descripción general de virtualización
La virtualización es una tecnología que permite que una plataforma de software,
llamada hipervisor, ejecute procesos que contienen un sistema informático completamente
emulado. El hipervisor es responsable de administrar los recursos del hardware
físico que pueden ser utilizados por máquinas virtuales individuales. Estas máquinas
virtuales se denomian *guests* del hipervisor. Una maquina virtual tiene muchos
aspectos de una computadora física emulada en software, como el BIOS del sistema y 
los controladores de disco del disco duro. Una máquina virtual a menuso usará
imágenes de disco duro que se almacenan como archivos individuales, y tendran acceso
a la RAM y CPU de la máquina host a través del software del hipervisor. El
hipervisor separa los accesos a los recursos de hardware del sistema host entre los
guests, lo que permite múltiples sistemas operativos se ejecuten en un solo
sistema host.

Los hipervisores de uso común en Linux son:

**Xen**: xen es un hipervisor de código abierto de tipo 1, lo que significa que no
depende de un sistema operativo subyacente para funcionar. Un hipervisor de este tipo
se concoce como un hipervisor de *bare-metal hypervisor*, ya que la computadora
puede arrancar directamente en el hipervisor.

**KVM**: Kernel Virtual Machine es un módulo de kernel de Linux para virtualización.
KMV es un hipervisor tipo 1 como del tipo 2, porque aunque necesita un sistema
operativo Linux génerico para funcionar, puede funcionar perfectamente como hipervisor
al integrarse con una instalación Linux en ejecución. Las máquinas virtuales
implementadas con KVM usan el demonio `libvirt` y las utilidades de software asociadas
para ser creadas y administradas.

**VirtualBox**: una aplicación de escritorio popular que facilita la creación y
administración de máquinas virtuales. Oracle VM VirtualBox es multiplataforma y
funciona en Linux, macOS y Windows. Como VirtualBox requiere un sistema operativo
subyacente para ejecutarse, es un hipervisor de tipo 2.

#### Tipos de máquinas virtuales
Hay tres tipos principales de máquinas virtuales: el *fully virtualized* guest
(un invitado toalmente virtualizado), el *paravirtualized* guest (paravirtualizado) y
el *hybrid* (híbrido).

**Totalmente virtualizado (Fully Virtualized)**: todas las instrucciones que se
esperan que ejecute un sistema operativo invitado deben poder ejecutarse dentro de
una instalación de sistema operativo totalmente virtualizado. La razón de esto es
que no se instalan controladores de software adicionales dentro de un huésped para
traducir las instrucciones a hardware simulado o real. Un invitado totalmente
virtualizado es aquel en el que el invitado (o HardwareVM) desconoce que es una
instancia de máquina virtual en ejecución. Para que este tipo de virtualización tenga
lugar en hardware basado en x86, las extensiones de CPU Intel VT-x o AMD-V deben
estar habilitadas en el sistema que tiene instalado el hipervisor. Esto se puede
hacer desde un menú de configuración de firmware BIOS o UEFI.

**Paravirtualizado (Paravirtualized)**: un invitado paravirtualizado (o PVM) es aquel
en el que el sistema operativo es consciente de que es una instancia de máquina
virtual en ejecución. Este tipo de invitados utilizará un kernel modificado y
controladores especiales (conocidos como "controladores invitados") que ayudaran al
sistema operativo invitado a utilizar los recursos de software y hardware del
hipervisor. El rendimiento de un huésped paravirtualizado es a menudo mejor que el
del huésped totalmente virtualizado debido a la ventaja que poporciona estos
controladores de software.

**Híbrido (Hybrid)**: la paravirtualización y virtualización se pueden combianr para
permitir que los sistemas operativos no modificados reciban un rendimiento de E/S casi
nativo mediante el uso de controladores paravirtualizados en sistemas operativos
completamente virtualizados. Los controladores paravirtualizados contiene controladores
de almacenamiento y dispositivos de red con disco mejorado y redimiento de E/Sd de red.

Las plataformas de virtualización a menudo proporcionan controladores invitados
empaquetados para sistemas operativos virtualizados. El KVM utiliza controladores del
proyecto *Virtio*, mientras que Oracle VM VirtualBox utiliza *Guest Extensions*
disponibles desde un archivo de iamgen DC-ROM ISO descargable.

#### Ejemplo de máquina virtual `libvirt`
Veremos un ejemplo de máquina virtual que es administrada por `libvirt` y usa el
hipervisor KVM. Una máquina virtual a menudo consiste en un grupo de archivos,
principalmente un archivo XML que define la máquina virtual (como su configuración
de hardware, conectividad de red, capacidades de visualización y más) y un archivo
de imagen de disco duro asociado que contiene la instalación del sistema operativo
y su software.

Primero, comencemos a examinar un archivo de configuración XML de ejemplo para una
máquina virtual y su entorno de red:
```sh
ls /etc/libvirt/qemu
drwxr-xr-x 3 root root 4096 Oct 29 17:48 networks
-rw------- 1 root root 5667 Jun 29 17:17 rhel8.0.xml
```

Tenga en cuenta que hay un directorio llamado `networks`. Este directorio contiene
archivos de definición (también usando XML) que crean configuraciones de red que las
máquinas virtuales pueden usar. Este hipervisor solo utiliza una red, por lo que solo
hay un archivo de definición que contiene una configuración para un segmento de red
virtual que utilizarán estos sistemas.
```sh
ls -l /etc/libvirt/qemu/networks/
total 8
drwxr-xr-x 2 root root 4096 Jun 29 17:15 autostart
-rw------- 1 root root
576 Jun 28 16:39 default.xml
$ sudo cat /etc/libvirt/qemu/networks/default.xml
<!--
WARNING: THIS IS AN AUTO-GENERATED FILE. CHANGES TO IT ARE LIKELY TO BE
OVERWRITTEN AND LOST. Changes to this xml configuration should be made using:
virsh net-edit default
or other application using the libvirt API.
-->
<network>
    <name>default</name>
    <uuid>55ab064f-62f8-49d3-8d25-8ef36a524344</uuid>
    <forward mode='nat'/>
    <bridge name='virbr0' stp='on' delay='0'/>
    <mac address='52:54:00:b8:e0:15'/>
    <ip address='192.168.122.1' netmask='255.255.255.0'>
        <dhcp>
            <range start='192.168.122.2' end='192.168.122.254'/>
        </dhcp>
    </ip>
</network>
```
Esta definición incluye una red privada de Clase C y un dispositivo de hardware
emulado para actuar como enrutador para esta red. También hay un rango de direcciones
IP para que el hipervisor las use con una implementación de servidor DHCP que puede
asignarse a las máquinaas virtuales que usan esta red. Esta configuración de red
también utiliza la traducción de direcciones de red (NAT) para reenviar paquetes a
otras redes, como a la LAN del hipervisor.

    sudo cat /etc/libvirt/qemu/rhel8.0.xml

```xml
WARNING: THIS IS AN AUTO-GENERATED FILE. CHANGES TO IT ARE LIKELY TO BE
OVERWRITTEN AND LOST. Changes to this xml configuration should be made using:
virsh edit rhel8.0
or other application using the libvirt API.
-->
<domain type='kvm'>
    <name>rhel8.0</name>
    <uuid>fadd8c5d-c5e1-410e-b425-30da7598d0f6</uuid>
    <metadata>
        <libosinfo:libosinfo
xmlns:libosinfo="http://libosinfo.org/xmlns/libvirt/domain/1.0">
        <libosinfo:os id="http://redhat.com/rhel/8.0"/>
        </libosinfo:libosinfo>
    </metadata>
    <memory unit='KiB'>4194304</memory>
    <currentMemory unit='KiB'>4194304</currentMemory>
    <vcpu placement='static'>2</vcpu>
    <os>
        <type arch='x86_64' machine='pc-q35-3.1'>hvm</type>
        <boot dev='hd'/>
    </os>
    <features>
        <acpi/>
        <apic/>
        <vmport state='off'/>
    </features>
    <cpu mode='host-model' check='partial'>
        <model fallback='allow'/>
    </cpu>
    <clock offset='utc'>
        <timer name='rtc' tickpolicy='catchup'/>
        <timer name='pit' tickpolicy='delay'/>
        <timer name='hpet' present='no'/>
    </clock>
    <on_poweroff>destroy</on_poweroff>
    <on_reboot>restart</on_reboot>
    <on_crash>destroy</on_crash>
    <pm>
        <suspend-to-mem enabled='no'/>
        <suspend-to-disk enabled='no'/>
    </pm>
    <devices>
        <emulator>/usr/bin/qemu-system-x86_64</emulator>
        <disk type='file' device='disk'>
            <driver name='qemu' type='qcow2'/>
            <source file='/var/lib/libvirt/images/rhel8'/>
            <target dev='vda' bus='virtio'/>
            <address type='pci' domain='0x0000' bus='0x04' slot='0x00' function='0x0'/>
        </disk>
        <controller type='usb' index='0' model='qemu-xhci' ports='15'>
            <address type='pci' domain='0x0000' bus='0x02' slot='0x00' function='0x0'/>
        </controller>
        <controller type='sata' index='0'>
            <address type='pci' domain='0x0000' bus='0x00' slot='0x1f' function='0x2'/>
        </controller>
        <controller type='pci' index='0' model='pcie-root'/>
        <controller type='pci' index='1' model='pcie-root-port'>
            <model name='pcie-root-port'/>
            <target chassis='1' port='0x10'/>
            <address type='pci' domain='0x0000' bus='0x00' slot='0x02' function='0x0'
multifunction='on'/>
        </controller>
        <controller type='pci' index='2' model='pcie-root-port'>
            <model name='pcie-root-port'/>
            <target chassis='2' port='0x11'/>
            <address type='pci' domain='0x0000' bus='0x00' slot='0x02' function='0x1'/>
        </controller>
        <controller type='pci' index='3' model='pcie-root-port'>
            <model name='pcie-root-port'/>
            <target chassis='3' port='0x12'/>
            <address type='pci' domain='0x0000' bus='0x00' slot='0x02' function='0x2'/>
        </controller>
        <controller type='pci' index='4' model='pcie-root-port'>
            <model name='pcie-root-port'/>
            <target chassis='4' port='0x13'/>
            <address type='pci' domain='0x0000' bus='0x00' slot='0x02' function='0x3'/>
        </controller>
        <controller type='pci' index='5' model='pcie-root-port'>
            <model name='pcie-root-port'/>
            <target chassis='5' port='0x14'/>
            <address type='pci' domain='0x0000' bus='0x00' slot='0x02' function='0x4'/>
        </controller>
        <controller type='pci' index='6' model='pcie-root-port'>
            <model name='pcie-root-port'/>
            <target chassis='6' port='0x15'/>
            <address type='pci' domain='0x0000' bus='0x00' slot='0x02' function='0x5'/>
        </controller>
        <controller type='pci' index='7' model='pcie-root-port'>
            <model name='pcie-root-port'/>
            <target chassis='7' port='0x16'/>
            <address type='pci' domain='0x0000' bus='0x00' slot='0x02' function='0x6'/>
        </controller>
        <controller type='virtio-serial' index='0'>
            <address type='pci' domain='0x0000' bus='0x03' slot='0x00' function='0x0'/>
        </controller>
        <interface type='network'>
            <mac address='52:54:00:50:a7:18'/>
            <source network='default'/>
            <model type='virtio'/>
            <address type='pci' domain='0x0000' bus='0x01' slot='0x00' function='0x0'/>
        </interface>
        <serial type='pty'>
            <target type='isa-serial' port='0'>
                <model name='isa-serial'/>
            </target>
        </serial>
        <console type='pty'>
            <target type='serial' port='0'/>
        </console>
        <channel type='unix'>
            <target type='virtio' name='org.qemu.guest_agent.0'/>
            <address type='virtio-serial' controller='0' bus='0' port='1'/>
        </channel>
        <channel type='spicevmc'>
            <target type='virtio' name='com.redhat.spice.0'/>
            <address type='virtio-serial' controller='0' bus='0' port='2'/>
        </channel>
        <input type='tablet' bus='usb'>
            <address type='usb' bus='0' port='1'/>
        </input>
        <input type='mouse' bus='ps2'/>
        <input type='keyboard' bus='ps2'/>
        <graphics type='spice' autoport='yes'>
            <listen type='address'/>
            <image compression='off'/>
        </graphics>
        <sound model='ich9'>
            <address type='pci' domain='0x0000' bus='0x00' slot='0x1b' function='0x0'/>
        </sound>
        <video>
            <model type='virtio' heads='1' primary='yes'/>
            <address type='pci' domain='0x0000' bus='0x00' slot='0x01' function='0x0'/>
        </video>
        <redirdev bus='usb' type='spicevmc'>
            <address type='usb' bus='0' port='2'/>
        </redirdev>
        <redirdev bus='usb' type='spicevmc'>
            <address type='usb' bus='0' port='3'/>
        </redirdev>
        <memballoon model='virtio'>
            <address type='pci' domain='0x0000' bus='0x05' slot='0x00' function='0x0'/>
        </memballoon>
        <rng model='virtio'>
            <backend model='random'>/dev/urandom</backend>
            <address type='pci' domain='0x0000' bus='0x06' slot='0x00' function='0x0'/>
        </rng>
    </devices>
</domain>
```
Este archivo define una serie de configuraciones de hardware que utiliza el sistema
invitado (guest), como la cantidad de RAM que le habrá asignado, el número de
núcleos de CPU del hipervisor al que tendrá acceso el invitado, el archivo de imagen
del disco duro que está asociado (bajo la etiqueta `disk`), sus capacidades de
visualización (a través del protocolo SPICE) y el acceso del invitado a dispositivos
USB, así como la entrada emulada de teclado y mouse.

#### Ejemplo de almacenamiento en disco de una máquina virtual
La imagen de esta máquina virtual reside en `/var/lib/libvirt/images/rhel8`. Aquí
está la imagen de disco en este hipervisor:
```sh
sudo ls -lh /var/lib/libvirt/images/rhel8
-rw------- 1 root root 5.5G Oct 25 15:57 /var/lib/libvirt/images/rhel8
```
El tamaño actual de esta imagen de disco ocupa solo 5,5 GB de espacio en el
hipervisor. Sin embargo, el sistema operativo dentro de este invitado ve un disco de
23,3 GB de tamaño, como lo demuestra la salida del siguiente comando desde la
máquina virtual en ejecución:
```sh
lsblk
NAME            MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
vda             252:0   0   23.3G   0   disk
├─vda1          252:1   0   1GB     0   part /boot
└─vda2          252:2   0   22.3G   0   part
    ├─rhel-root 253:0   0   20G     0   lvm /
    └─rhel-swap 253:1   0   2.3G    0   lvm [SWAP]   
```
Esto se debe al tipo de aprovisionamiento de disco utilizado para este invitado. Hay
varios tipos de imágenes de disco que una máquina virtual puede usar, pero los dos
tipos principales son:

**COW**: copy-on-write (también conocido como *thin-porvisioning* o *sparse images*)
es un método en el que se crea un archivo de disco con un límite de tamaño superior
predefinido. El tamaño de la imagen del disco solo aumenta a medida que se escriben
nuevos datos en el disco. Al igual que en el ejemplo anterior, sl sistema operativo
invitado ve el límite de disco predefinido de 23,3 GB, pero solo ha escrito 5,5 GB
de datos en el archivo de disco. El formato de disco utilizado para la máquina
virtual de ejemplo es `qcow2`, que es un archivo de imagen QEMU COW.

**RAW**: un tipo de disco *raw* o *full* es un archivo que tiene todo su espacio
preasignado. Por ejemplo, un archivo de imagen de disco sin formato de 10 GB consume
10 GB de espacio real en el hipervisor. Hay un beneficio de rendimiento para este
estilo de disco, ya que todo el espacio en disco necesario ya existe, por lo que el
hipervisor subyacente puede simplemente escribir datos en el disco sin el impacto de
rendimiento de monitorear la imagen del disco para asegurarse de que aún no ha
alcanzado su límite y extender el tamaño del archivo a medida que se escriben nuevos
datos.

Existen otras plataformas de administración de virtualización como Red Hat Enterprise
Virtualization o oVirt que pueden usar discos físicos paraactuar como ubicaciones de
almacenamiento de respaldo para el sistema operativo de una máquina virtual. Estos
sistemas pueden utilizar la red de área de almacenamiento (SAN) o dispositivos de
almacenamiento conectados a la red (NAS) para escribir sus datos, y el hipervisor
realiza un seguimiento de qué ubicaciones de almacenamiento pertenecen a qué máquinas
virtuales. Estos sistemas de almacenamiento pueden usar tecnologías como la
administración de volumen lógico (LVM) para aumentar o reducir el tamaño de
almacenamiento en disco de una máquina virtual según sea necesario, y para la creación
y administración de instantáneas de almacenamiento.

#### Trabajar con plantillas de máquinas virtuales
pg 173
