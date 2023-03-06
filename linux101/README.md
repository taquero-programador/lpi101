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
los que se deben ejecutar una acción espefícica. El término `action` define cómo `init`
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

pg 85
