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
pasar parámetros al núcleo, como qué partición contiene el sistema de archivos raíz o
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
los reinicios. Se debe generar un nuevo archivo de configuración para el gestor de
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
núcleo en la RAM. Luego, el núcleo se hará cargo de la CPU y comenzará a detectar y
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
en la mayoría de las distribuciones de Linux. El administrador de servicios es el
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

Esto ha sido reemplazado por `/media`, que ahora es el punto de montaje predeterminado
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
partición MBR, la ID de la partición es `0xEF`.

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

El parámetro `set root` define el disco y la partición donde se encuentra el sistema
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
Luego, configure la partición de arranque como se indicó anteriormente y use el
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
Las referencias contenidas en los programas vinculados dinámicamente se resuelve
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
*Advanced Package Tool* (APT) es un sistema de administración de paquetes, que incluye
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

En el resultado anterior, la identificación del repositorio se muestra en la primera
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
dnf info PACKAGENAME
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
asignarse a las máquinas virtuales que usan esta red. Esta configuración de red
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

#### Trabajando con plantillas de máquinas virtuales
Dado que las máquinas virtuales generalmente son solo archivos que se ejecutan en un
hipervisor, es fácil crear plantillas que se puedan personalizar para escenarios de
implementación particulares. A menudo, una máquina virtual tendrá una instalación
básica del sistema operativo y algunos ajustes de configuración de autenticación
preconfigurdos para facilitar futuros lanzamientos del sistema. Esto reduce la cantidad
de tiempo que lleva contruir un nuevo sistema al reducir la cantidad de trabajo que a
menudo se repite, como la instalación de paquetes base y la configuración regional.

Esta plantilla de máquina virtual podría copiarse luego a un nuevo sistema invitado
(guest). En este caso, se cambiaría el nombre del nuevo invitado, se generaría una
nueva dirección MAC para su interfaz de red y se podría realizar otras modificaciones
dependiendo de su uso previsto.

#### El D-Bus Machine ID
Muchas instalaciones de Linux utilizarán un número de identifiación de máquina
generado en el momento de la instalación, llamado *D-Bus Machine ID*. Sin embargo, si
una máquina virtual se clona para ser utilizada como plantilla para otras instalaciones
de máquinas virtuales, se necesitaría crear una nueva ID de máquina D-Bus para
garantizar que los recuros del sistema del hipervisor se dirijan al sitema invitado
apropoiadamente.

El siguiente comando se puede usar para validar que existe una ID de máquina D-Bus
para el sistema en ejecución:

    dbus-uuidgen --ensure

Si no se encuentran mensajes de error, existe un ID para el sistema. Para ver la ID
actual de la máquina D-Bus, ejecute lo siguiente:
```sh
dbus-uuidgen --get
17f2e0698e844e31b12ccd3f9aa4d94a
```
La cadena de texto que se muestra es el número de identificación actual. No hay dos
sistemas Linux que se ejecuten en un hipervisor que tengan la misma ID de máquina
D-Bus.

La ID de la máquina se encuentra en `/var/lib/dbus/machine-id` y está simbólicamente
vinculado a `/etc/machine-id`. Se desaconseja cambiar este número de identificación
en un sistema en ejecución, ya que es probable que ocurra inestabilidad y fallas
del sistema. Si dos máquinas virtuales tienen la misma ID de máquina D-Bus, siga el
procedimiento a continuación para generar una nueva:
```sh
sudo rm -f /etc/machine-id
sudo dbus-uuidgen --ensure=/etc/machine-id
```
En el caso de que `/var/lib/dbus/machine-id` no sea un enlace simbólico a
`/etc/machine-id`, entonces sería necesario eliminar `/var/lib/dbus/machine-id`.

#### Implementación de máquinas virtuales en la nube
Hay una multitud de proveedores de IaaS (*Infrastucture as a service*) disponibles que
ejecutan sistemas de hipervisor y que pueden implementar imágenes virtuales de
invitados para una organización. Prácticamente todos estos proveedores cuentan con
herramientas que permiten a un administrador contruir, implementar y configurar
máquinas virtuales personalizadas basadas en una variedad de distribuciones de Linux.
Muchas de estas compañias también tienen sistemas que permiten el desliegue y las
migraciones de máquinas virtuales creadas desde la organización de un cliente.

Al evaluar la implementación de un sistema linux en un entorno IaaS, hay algunos
elementos claves que un administrador debe tener en cuenta:

**Instancias de computación**: muchos proveedores de la nube cobrarán tasas de uso
basadas en "instancias de computación", o cuánto tiempo de CPU utilizará su
infraestructura basada en la nube. Una planificación cuidadosa de cuánto tiempo de
procesamiento requerirán realmente las aplicaciones ayudará a mantener manejables los
costos de una solución en la nube.

Las instancias de computación a menudo también se refieren a la cantidad de máquinas
virtuales que se aprovisionan en un entorno en la nube. Una vez más, la mayor
cantidad de instancias de sistemas que se ejecutan a la vez también influirá en la
cantidad de tiempo total de CPU que se le cobrará a una organización.

**Bloque de almacenamiento**: los proveedores en la nube también tienen varios niveles
de almacenamiento en bloque disponibles para que una organización los use. Algunas
ofertas están destinadas simplemente a ser un almacenamiento de red basado en la web
para archivos, y otras ofertas se relacionan con el almacenamiento externo  para una
máquina virtual aprovisionada en la nube para usar y alojar archivos.

EL costo de tales ofertas variará según la cantidad de almacenamiento utilizado y la
velocidad del almacenamiento dentro de los centros de datos del proveedor. El acceso
al almacenamiento más rápido generalmente costará más y, por el contrario, los datos
"en reposo" a menudo son muy económicos.

**Redes**: uno de los componentes principales para trabajar con un proveedor de
soluciones en la nube es cómo se configurará la red virtual. Muchos proveedores de
soluciones IasS tendrán alguna forma de utilidades basadas en la web que puedan
utilizarse para el diseño e implementación de diferentes rutas de red, subredes y
configuraciones de firewall. Algunos incluso proporcionarán soluciones de DNS para
que se puedan asignar FQDN de acceso público (nombres de dominio completo) a sus
sistemas orientados a Internet. Incluso hay soluciones "híbridas" disponibles que
pueden conectarse de una infraestructura de red existente en las instalaciones de la
empresa a una infraestructura basada en la nube a través de una VPN, uniendo las dos
infraestructuras.

#### Acceso seguro a los invitados (guest) en la nube
El método más frecuente para acceder a un invitado virtual remoto en una plataforma
en la nube es mediante el uso del software OpenSSH. Un sistema Linux que reside en
la nube tendría el servidor OpenSSH ejecutándose, mientras que un administrador usaría
un cliente OpenSSH con claves precompartidas para acceso remoto.

Un administrador ejecutaría el siguiente comando:
```sh
ssh-keygen
# or
ssh-keygen -t rsa -b 4096 -C "comentario" -f ~/.ssh/name_key
```
Y seguiría las instrucciones para crear un par de claves SSH públicas y privadas. La
clave privada permanece en el sistema local del administrador (almacenado en
`~/.ssh/`) y la clave pública se copia en el sistema remoto de la nube.

El administrador ejecutaría el siguiente comando:

    ssh-copy-id -i ~/.ssh/pubkey user@ip

Esto copiará la clave SSH pública del par de claves recién generadas en el servidor
remoto de la nube. La clave pública se registrará en el archivo `~/.ssh/authorized_keys`
del servidor de la nube y establecerá los permisos apropiados en el archivo.

Algunos proveedores de la nube generarán automáticamente un par de claves cuando se
aprovisione un nuevo sistema Linux. El administrador deberá descargar la clave
privada para el nuevo sistema desde el proveedor de la nube y almacenarla en su
sistema local. Tenga en cuenta que los permisos para las claves SSH deben ser `0600`
para una clave privada y `0644` para una clave pública.

#### Preconfigurar un sistema en la nube
Una herramienta útil que simplifica los desliegues de máquinas virtuales basadas en
la nube es la utilidad `cloud-init`. Este comando, junto con los archivos de
configuración asociados y la imagen de máquina virtual predefinida, es un método
independiente del proveedor para implementar un invitado Linux en una gran cantidad
de proveedores IaaS. Utilizando achivos de texto plano YAML 
(*YAML Ain't Markup Language*), un administrador puede preconfigurar configuraciones de
red, selecciones de paquetes de software, configuración de claves SSH, creación de
cuentas de usuario, configuraciones regionales, junto con una miríada de otras
opciones para construir rápidamente nuevos sistemas.

Durante el arranque inicial de un nuevo sistema, `cloud-init` leerá la configuración de
los archivos YAML y los aplicará. Este proceso solo necesita aplicarse a la
configuración inicial de un sistema y facilita la implementación de una flota de
nuevos sistemas en la plataforma de un proveedor en la nube.

La sintaxis del archivo YAML utilizada con `cloud-init` se llama *cloud-config*. Archivo
de ejemplo `cloud-config`:
```yaml
#cloud-config
timezone: Africa/Dar_es_Salaam
hostname: test-system
# Update the system when it first boots up
apt_update: true
apt_upgrade: true
# Install the Nginx web server
packages:
- nginx
```
Tenga en cuenta que la línea superior no hay espacio entre el símbolo hash (`#`) y el
término `cloud-confi`.

#### Contenedores
La tecnologías de contenedores es similar en algunos aspectos a una máquina virtual,
donde se obtiene un entorno aislado para implementar fácilmente una aplicación.
Mientras que con una máquina virtual se emula una computadora completa, un contendor
el software suficiente para ejecutar una aplicación. De esta manera, hay mucho menos
gastos generales.

Los contenedores permiten una mayor flexibilidad sobre la de una máquina virtual. Un
contenedor de aplicaciones se puede migrar de un host a otro, al igual que una máquina
virtual se puede migrar de un hipervisor a otro. Sin embargo, a veces una máquina
virtual necesitará apagarse antes de que pueda migrarse, mientras que con un
contenedor la aplicación siempre se está ejecutando mientras se migra. Los contenedores
también facilitan la implementación de nuevas versiones de aplicaciones en conjunto
con una versión existente. A medida que los usuarios cierran sus sesiones con
contenedores en ejecución, el software de orquestación de contenedores puede eliminar
automáticamente estos contenedores del sistema y reemplazarlos con la nueva versión,
lo que reduce el tiempo de inactividad.

Los controladores utilizan el mecanismo *control groups* (mejor conocido como cgroups)
dentro del kernel de Linux. El cgroup es una forma de particionar los recursos del
sistema, como la memoria, el tiempo de procesador y el ancho de banda del disco y
la red para una aplicación individual. Un administrador puede usar cgroup directamente
para establecer límites de recursos del sistema en una aplicación, o un grupo de
aplicaciones que podrían existir dentro de un solo cgroup. En escencia, esto es lo que
hace el software de contenedor para el administrador, además de proporcionar
herramientas que facilitan la administración y la implementación de cgroups.

## Uso de secuencias de texto, tuberías y redireccionamiento
#### Here Document y Here String
Otra forma de redirigir la entrada involucra los métodos *Here Document* y *Here String*.
La redirección de documentos Here permite escribir texto de varias líneas que se utilizará
como contenido redirigido. Dos símbolos menores que `<<` indican una redirección de
Here Document:
```sh
wc -c <<EOF
> How many characters
> > in this Here document?
> > EOF
> 43
```
A la derecha de los dos símbolos menor que `<<` se encuentra el término final `EOF`. El
modo de inserción finarlizará tan pronto como se ingrese una línea que contenga solo
el término final. Se puede usar cualquier otro término final, pero es importante no poner
caracteres en blanco entre el símbolo menor que que y el término final.

El método *Here String* es muy similar al método de *Here Document*, pero solo para una
línea:
```sh
wc -c <<<"How many characters in this Here string?"
41
```

## Crear, matar y supervisar procesos
Cada vez que invocamos un comandos, se inician uno o más procesos. Un administrador de
sistemas bien entrenado no solo necesita crear procesos, sino también poder realizar un
seguimiento de ellos y enviarles diferentes tipos de señales si es necesario.

#### Control de trabajos
Los trabajos (*jobs*) son procesos que se han iniciado de forma interactiva a través de
una terminal, enviados a un segundo plano y aún no han finaliazado la ejecución. Puede
conocer los trabajos activos (y su estado) en un sistema Linux ejecutando `jobs`:

    jobs

El comando `jobs` no produce ningún resultado, lo que significa que no hay trabajos
activos en este momento. Lanzar `sleep 60` y presionar `Ctrl + z`:
```sh
sleep 60
^Z
[1]+    Stopped     sleep 60
```
La ejecución del comando se ha detenido (o, mejor dicho, suspendido) y el símbolo del
sistema vuelve a estar disponible. Puede buscar trabajos por segunda vez y encontrará
el *suspendido*:
```sh
jobs
[1]  + suspended  sleep 60
```
Explicación del resultado:
- **`[i]`** : este número es el ID del trabajo y se puede utilizar, precedido por un símbolo de porcentaje (`%`), para cambiar el estado del trabajo mediante las utilidades `fg`, `bg`, y `kill`.
- **`+`** : el signo más indica el trabajo actual predeterminado (es decir, el último suspendido o enviado al segundo plano). El trabajo anterior está marcado por un signo menos (`-`). Cualquier otro trabajo anterior no está marcado.
- **`Stopped`** : descripción del estado del trabajo
- **`sleep 60`** : el comando o trabajo en ejecución

Con la opción `-l`, los trabajos también monstrará la ID del proceso (PID) justo antes
del estodo:
```sh
jobs -l
[1]+ 1114 Stopped sleep 60
```
Las opciones posibles restantes de trabajo son:
- **`-a`**: lista solo los procesos que han cambiado de estado desde la última notificación. El estado posible incluye, `Running`, `Stopped`, `Terminated` o `Done`.
- **`-p`**: lista los IDs deprocesos
- **`-r`**: lista solo los procesos en ejecución
- **`-s`**: lista solo los trabajos detenidos (o suspendidos).

#### Especificaciones de trabajo
El comando `jobs`, así como otras utilidades, como `fg`, `bg` y `kill` necesitan una
espeficiación de trabajo (`jobspec`) para actuar sobre un trabajo en particular. Como
acabamos de ver, esto puede ser, y normalmente es, el ID del trabajo precedido por `%`.
Sin embargo, otras especificaciones de trabajo también son posibles.

- **`%n`**: trabajo cuyo proceso de identificación es `n`:
```sh
jobs %1
[1]+    Stopped     sleep 60
```
- **`%str`**: trabajo cuya línea de comando comienza con `str`:
```sh
jobs %sl
[1]+    Stopped     sleep 60
```
- **`%?str`**: trabajo cuya línea contiene `str`:
```sh
jobs %?le
[1]+    Stopped     sleep 60
```
- **`%+ o %%`**: trabajo actual (el último que se inició en segundo plano o suspendido del primero trabajo):
```sh
jobs %+
[1]+    Stopped     sleep 60
```
- **`%-`**: trabajo anterior (el que era `%` + antes del predeterminado, el actual):
```sh
jobs %-
[1]+    Stopped     sleep 60
```

#### Estado de trabajo: suspensión, primer plano y segundo plano
Una vez que un trabajo está en segundo plano o ha sido suspendido, podemos hacer
cualquiera de estas tres cosas:
1. Llevarlo al primer plano con `fg`:
```sh
fg %1
sleep 60
```
`fg` mueve el trabajo especificado al primer plano y lo convierte en el trabajo actual.
Ahora podemos esperar hasta que termine, detenerlo nuevamente con `Ctrl + Z` o
terminalo con `Ctrl + C`.

2. Llevarlo a un segundo plano con `bg`:
```sh
bg %1
[1]+ sleep 60 &
```
Una vez en segundo plano, el trabajo se puede volver a poner en primer placon con `fg` o
matar. Tenga en cuenta que el signo `&` significa que el trabajo se ha enviado a segundo
plano. De hecho, también puede usar el signo y comenzar un proceso directamente en 
segundo plano:
```sh
sleep 100 &
[2] 970
```
Junto con el ID de trabajo del nuevo trabajo `[2]`, ahora también obtenemos su ID de
proceso (`970`). ahora ambos trabajos se ejecutan en segundo plano:
```sh
jobs
[1]- Running sleep 60 &
[2]+ Running sleep 100 &
```
Un poco más tarde, el primer trabajo finaliza la ejecución:
```sh
jobs
[1]- Running sleep 60 &
[2]+ Running sleep 100 &
```
Un poco más tarde, el primer trabajo finaliza la ejecución:
```sh
jobs
[1]- Done sleep 60
[2]+ Running sleep 100 &
```
3. Termine con una seña `SIGTERM` con `kill`:
```sh
kill %2
```
Para asegurarse de que el trabajo ha finalizado, ejecute `jobs` nuevamente:
```sh
jobs
[2]+    Terminated      sleep 100
```

#### Trabajos separados: `nohup`
Los trabajos que hemos visto en las secciones anteriores se adjuntan a la sesión del
usuario que los invocó. Eso significa que si la sesión se termina, los trabajos
desaparecen. Sin embargo, es posible separar los trabajos se las sesiones y hacer
que se ejecuten incluso después de cerrar la sesión. Esto se logran con el comando
`nohup`. La sintaxis es la siguiente:

    nohup COMMAND &

Separemos el trabajo en segundo plano `ping localhost` de la sesión actual:
```sh
nohup ping localhost &
[1] 1251
$ nohup: ignoring input and appending output to 'nohup.out'
^C
```
La salida muestra la ID de trabajo `[1]` y el pid (`1251`), seguido de un mensaje que
nos informa sobre el archivo `nohup.out`. Este es el archivo predeterminado donde se
guarda `stdout` y `stderr`. Ahora podemos presesionar `Ctrl + C` para liberar el símbolo
del sistema, cerrar la sesión, iniciar otro como `root` y usar `tail -f` para verificar
si el comando se está ejecutando y la salida se está escribiendo en el archivo
predeterminado:
```sh
exit
logout
$ tail -f /home/carol/nohup.out
64 bytes from localhost (::1): icmp_seq=3 ttl=64 time=0.070 ms
64 bytes from localhost (::1): icmp_seq=4 ttl=64 time=0.068 ms
64 bytes from localhost (::1): icmp_seq=5 ttl=64 time=0.070 ms
^C
```
>Se puede seleccionar cualquier ubicación para el archivo `nohup.out`
Si queremos matar el proceso, debemos especificar su PID:

    kill 1251

#### Monitorear procesos
Un proceso o tarea es una instancia de un programa en ejecución. Por lo tanto, se crean
nuevos procesos cada vez que escribe comandos en la terminal.

El comando `watch` ejecuta un comando periódicamente (2 segundo por defecto) y nos
permite mirar el cambio de salida del programa con el tiempo. Por ejemplo, podemos
monitorear cómo cambia el promedio de carga a medida que se ejecutan más procesos
escribiendo `watch uptime`:
```sh
Every
2.0s: uptime    debian: Tue Aug 20 23:31:27 2019
23:31:27 up 21 min, 1 user, load average: 0.00, 0.00, 0.00
```
El comando se ejecuta hasta que se interrumpe, por lo que deberíamos detenerlo con
`Ctrl + C`. Obtenemos dos líneas como salida: la primera corresponde a `watch` y nos
dice con qué frecuencia se ejecutará el comando (`Every 2.0s: uptime`), qué
comando/programa mirar (`uptime`) así como el comando nombre del host y fecha
(`debian: mar 20 de agosto 23:31:27 2019`). La segunda línea es el tiempo de actividad
e incluye la hora (`23:31:27`), cuánto tiempo ha estado activo el sistema (`up 21 min`),
el número de usuarios activos (`1 usuario`) y carga promedio del sistema o número de
procesos en ejecución o estado en espera durante los últimos 1, 5 y 15 minutos
(`promedio de carga: 0.00, 0.00, 0.00`).

Del mismo modo, puede verificar el uso de memoria a medida que se crean nuevos procesos
con `watch free`:
```sh
Every
2.0s: free
debian: Tue Aug 20 23:43:37 2019
23:43:37 up 24 min,
1 user,
load average: 0.00, 0.00, 0.00
total used free shared buff/cache available
Mem: 16274868 493984 14729396 35064 1051488 15462040
Swap: 16777212 0 16777212
```
Para cambiar el intercambio de actualización para `watch` use la opción `-n` o
`--interval` más el número de segundos como en:

    watch -n 5 free

Ahora el comando `free` se ejecutará cada 5 segundos.

#### Envío de señales a procesos: `kill`
Cada proceso tiene un identificador de proceso único o PID. Una forma de averiguar el
PID de un proceso es mediante el comndoo `pgrep` seguido del nombre del proceso:
```sh
pgrep sleep
1201
```
Similar a `pgrep`, el commando `pkill` mata un proceso basado en su nombre:
```sh
pkill sleep
[1]+    Terminated      sleep 60
```
Para matar varias instancias del mismo proceso, se puede usar el comando `killall`:
```sh
$ sleep 60 &
[1] 1246
$ sleep 70 &
[2] 1247
$ killall sleep
[1]- Terminated sleep 60
[2]+ Terminated sleep 70
```
Tanto `pkill` como `killall` funcionan de la misma manera que `kill` en que envía una
señal de terminación a procesos especificados. Si no se proporciona una señal, se
envía el valor predeterminado de `SIGTERM`. Sin embargo, `kill` solo toma un trabajo o
un ID de proceso como argumento.

Las señales se pueden especificar por:
- **Nombre**:
```sh
kill -SIGHUP 1247
```
- **Número**:
```sh
kill -1 1247
```
- **Opciones**:
```sh
kill -s SIGHUP 1247
```
Para que `kill` funcione de manera similar a `pkill` o `killall` podemos usar la
sustitución de comandos:

    kill -1 $(pgrep sleep)

#### `top` y `ps`
Cuando se trata de monitoreo de procesos, dos herramientas invaluables son `top` y `ps`.
Mientras que el primero produce resultados dinámicamente, el segundo lo hace
estáticamente. En caulquier caso, ambos son excelente utilidades para tener una visión
integral de todos los procesos en el sistema.

#### Interactuando con `top`

    top

`top` le permite al usuario cierta interacción. Por defecto, la salida se ordena por
porcentaje de tiempo de CPU utilizado por cada proceso en orden descendente. Este
comportamiento puede modificarse presionando las siguiente teclas desde `top`:
- **`M`**: ordena por el uso de memoria
- **`N`**: ordena por número de ID
- **`T`**: ordena por el tiempo de ejecución
- **`P`**: ordena por porcentaje de uso de CPU

Otras teclas interesantes para interactuar con `top` son:
- **`? o h`**: ayuda
- **`k`**: mata un proceso. `top` solicitará que se elimine el `PID` del proceso y que se envíe la señal (por defecto, `SIGTERM` o `15`).
- **`r`**: cambiar la prioridad de un proceso (`renice`). `top` le pedirá el valor `nice`. los valores posibles oscilan entre -20 y 19, pero solo el superusuario puede establecerlo en un valor negativo o inferior al actual.
- **`u`**: lista de procesos de un usuario en particular (de forma predeterminada se muestran los procesos de todos los usuarios).
- **`c`**: muestras las rutas absolutas de los programas y diferencia entre procesos de espacio de usuario y procesos de espacio de kernel (entre corchetes).
- **`V`**: vista de bosque/jerarquía de procesos
- **`t y m`**: guardar ajustes de configuración en `~/.toprc`.

#### Una explicación de la salida `top`
La salida `top` se divide en dos áreas: el área resumen y el área de tareas.

#### El área de resumen den `top`
El área de resumen se compone de cinco filas superiores y nos proporciona la siguiente
información:
- `top - 11:10:29 up 2:21, 1 user, load average: 0,11, 0,20, 0,14`
    - Hora actual (formato 24 horas): `11:20:29`
    - Tiempo de actividad (cantidad de tiempo que el equipo ha estado activo y funcionando): `up 2:21`.
    - Número de usuarios conectados y promedio de carga de la CPU durante los últimos 1, 5 y 15 minutos, respectivamente: `load average: 0,11, 0,20, 0,14`.
- `Tasks: 73 total, 1 running, 72 sleeping, 0 stoppped, 0 zombie` (información sobre procesos).
    - Número total de procesos de modo activo: `73 total`.
    - Ejecutándose (los ejecutados en el momento): `1 running`.
    - Durmiendo (aquellos que esperan reanudar la ejecución): `72 sleeping`.
    - Detenido (por una seña de control de trabajo): `0 stoppped`.
    - Zombie (aquellos que han completado la ejecución, pero todavía están esperando que su proceso padre los elimine de la tabla de procesos): `0 zombie`.
- `%Cpu(s): 0,0 us, 0,3 sy, 0,0 ni, 99,7 id, 0,0 wa, 0,0 hi, 0,0 si, 0,0 st` (porcentaje de tiempo de CPU empleado).
    - Procesos de usuario: `0,0 us`
    - Procesos de sistema/kernel: `0,4 sy`
    - Procesos establecidos en un valor *nice*, cuanto mejor sea el valor, menor será la prioridad: `0,0 ni`.
    - Nada - tiempo de inactividad de la CPU: `99,7 id`
    - Procesos en espera de operaciones de E/S: `0,0 wa`
    - Procesos que sirven interrupciones de hardware, periféricos que envían las señales del procesador que requieren atención: `0,0 hi`.
    - Procesos que sirven interrupciones de software: `0,0 si`
    - Los procesos que sirven las tareas de otras máquinas virtuales en un entorno virtual, por lo tanto, roban tiempo: `0,0 st`.
- `KiB Mem : 1020332 total, 909492 free, 38796 used, 72044 buff/cache` (información de memoria en kilobytes).
    - Monto total de memoria: `1020332 total`
    - Memoria sin utilizar: `909492 free`
    - Memoria en usi: `38796 used`
    - La memoria intermedia (buffer) y almacenada en caché para evitar el acceso excesivo al disco: `72044 buff/cache`.
- `KiB Swap: 1046524 total, 1046524 free, 0 used, 873264 avail mem` (información memoria swap en kilobytes).
    - La cantidad total de espacio de swap: `1046524 total`
    - Espacio swap no utilizado: `1046524 free`
    - Espacio en uso de swap: `0 used`
    - La cantidad de memoria de intercambio que se puede asignar a los procesos sin causar más intercambio: `873264 avail mem`.

#### El área de tareas en `top`: campos y columnas
Debajo del área de resumen, aparece el área de tareas, que incluye una serie de campos
y columnas de información sobre los procesos en ejecución:
- **`PID`**: identificación de proceso
- **`USER`**: usuario que emitió el comando que generó el proceso
- **`PR`**: prioridad del proceso en el kernel
- **`NI`**: valor nice del proceso. Los valores más bajos tienen mayor prioridad que los más altos.
- **`VIRT`**: Cantidad total de memoria utilizada por el proceso (incluido la swap).
- **`RES`**: memoria RAM utilizada por el proceso
- **`SHR`**: memoria compartida del proceso con otros procesos
- **`S`**: estado del proceso. Los valores incluyen: `S` (suspensión interrumpible - esperando que termine un evento), `R` (ejecutable - ya sea una ejecución o en la cola que se ejecutará) o `Z` (procesos secundarios terminados en zombies cuyas estructuras de datos aún no se han eliminado de la tabla de procesos).
- **`%CPU`**: porcentaje del CPU utilizado por el proceso
- **`%MEM`**: porcentaje de RAM utilizada por el proceso, es decir, el valor `RES` expresado como porcentaje.
- **`TIME+`**: tiempo total de actividad del proceso
- **`COMMAND`**: nombre del comando/programa que generó el proceso

#### Visualización de procesos estáticos: `ps`
Como se dijo anteriormente, `ps` muestra una instantánea de los procesos. Para ver todos
los procesos con una terminal `tty`, escriba `ps a`:
```sh
ps a
PID TTY STAT TIME COMMAND
386 tty1 Ss+ 0:00 /sbin/agetty --noclear tty1 linux
424 tty7 Ssl+ 0:00 /usr/lib/xorg/Xorg :0 -seat seat0 (...)
655 pts/0 Ss 0:00 -bash
1186 pts/0 R+ 0:00 ps a
(...)
```

#### Explicación de la sintaxis y salida de la opción `ps`
Con respecto a las opciones, `ps` puede aceptar tres estilos diferentes: BSD, UNIX y GNU.

**BSD**: las opciones no siguen ningún guión inicial:
```sh
ps p 811
PID TTY STAT TIME COMMAND
811 pts/0 S 0:00 -su
```

**UNIX**: las opciones siguen un gión inicial:
```sh
ps -p 811
PID TTY TIME CMD
811 pts/0   00:00:00 bash
```

**GNU**: las opciones van seguidas de guiones dobles iniciales:
```sh
ps --pid 811
PID TTY TIME CMD
811 pts/0 00:00:00 bash
```
En los tres casos, `ps` informa sobre el proceso cuyo `PID` es `811`, en este caso `bash`.

Del mismo modo, puede usar `ps` para buscar los procesos iniciados por un usuario en
particular:
- `ps U carol` (BSD)
- `ps -u carol` (UNIX)
- `ps --user carol` (GNU)

Ver los procesos iniciados por `carol`:
```sh
ps U carol
PID TTY STAT TIME COMMAND
811 pts/0 S 0:00 -su
898 pts/0 R+ 0:00 ps U carol
```
Podemos obtener lo mejor de `ps` combinando algunas de sus opciones. Un comando muy útil
(que produce una salida similar a la de `top`) es `ps aux` (estilo BSD). En este caso, se
muestran los procesos de todos los shells (no solo el actual). El significado de los
interruptores es el siguiente:
- **`a`**: mostrara procesos que están conectados a una `tty` o terminal.
- **`u`**: mostrara formasto orientado al usuario
- **`x`**: mostrara procesos que no están conetados a una `tty` o terminal.

```sh
ps aux
USER    PID %CPU %MEM   VSZ RSS TTY     STAT START  TIME COMMAND
root    1   0.0  0.1  204504 6780 ?     Ss 14:04 0:00 /sbin/init
root    2   0.0  0.0    0   0     ?     S  14:04 0:00 [kthreadd]
root    3   0.0  0.0    0   0     ?     S  14:04 0:00 [ksoftirqd/0]
root    5   0.0  0.0    0   0     ?     S< 14:04 0:00 [kworker/0:0H]
root    7   0.0  0.0    0   0     ?     S  14:04 0:00 [rcu_sched]
root    8   0.0  0.0    0   0     ?     S   14:04 0:00 [rcu_bh]
root    9   0.0  0.0    0   0     ?     S 14:04 0:00 [migration/0]
```

- **`USER`**: dueño del proceso
- **`PID`**: identificador del proceso
- **`%CPU`**: porcentaje de CPU utilizado
- **`%MEM`**: porcentaje de memoria física utilizado
- **`VZS`**: memoria virtual de procesos en KiB
- **`RSS`**: memoria física no intercambiada utilizada por el proceso en KiB
- **`TT`**: terminal (`tty`) que controla el proceso
- **`STAT`**: código que representa el estado del proceso. Además de `S`, `R` y `Z`, otros valores posibles incluyen: `D` ( suspensión ininterrumpida, generalmente esperando E/S), `T` (detenido, normalmente por una señal de control). Algunos modificadores adicionales incluyen: `<` (alta prioridad, no agradable por otros procesos), `N` (baja prioridad, agradable para otros procesos) o `+` (en el grupo de procesos en primer plano).
- **`STARTED`**: hora a la que comenzó el proceso
- **`TIME`**: tiempo de CPU acumulado
- **`COMMAND`**: comando que inició el proceso

#### Características de los multiplexores terminales
En electrónica, un multiplexor (o mux) es un dispositivo que permite contectar
múltiples entradas a una sola salida. Por lo tanto, un multiplexor terminal no da la
capacidad de cambiar entre diferentes entradan según sea necesario. Aunque no son
exactamente lo mismo, `screen` y `tmux` comparten un serie de características comunes:
- Cualquier invocación exitosa dará como resultado al menos una sesión que, a su vez, incluirá al menos una ventana. Las ventanas contienen ventanas.
- Las ventanas se pueden dividir en regiones o paneles, lo que ayuda a la productividad cuando se trabaja con varios programas simultáneamente.
- Facilidad de control: para ajecutar la mayoría de los comandos, utilizan una combinación de teclas, el llamado *comando prefijo* o *comando clave*, seguido de otro carácter.
- Las sesiones se pueden separar de su terminal actual (es decir, los programas se envían en segundo plano y continúan ejecutándose). Esto garantiza la ejecución completa de los programas sin importar si cerramos accidentalmente un terminal, experimantamos un congelamiento ocasional del terminal o incluso una pérdida de conexión remota.
- Conexiones de socket
- Modo de copia
- Son altamente personalizables

#### GNU Screen
En los primeros diás de Unix (1970-1980), las computadoras consistían básicamente en
terminales conectados a una computadora central. Eso fue todo, sin múltiples ventanas
o pestañas. Y esa fue la razón detrás de la creación de GNU Screen en 1987: emular
múltiples pantallas VT100 independientes en un solo terminal físico.

#### Ventanas
La pantalla GNU se invoca simplemente scribiendo `screnn` en la terminal. Primero verá
un mensaje de bienvenida:
```sh
GNU Screen version 4.05.00 (GNU) 10-Dec-16
Copyright (c) 2010 Juergen Weigert, Sadrul Habib Chowdhury
Copyright (c) 2008, 2009 Juergen Weigert, Michael Schroeder, Micah Cowan, Sadrul
Habib Chowdhury
Copyright (c) 1993-2002, 2003, 2005, 2006, 2007 Juergen Weigert, Michael Schroeder
Copyright (c) 1987 Oliver Laumann
(...)
```
Presione la barra espaciadora o Enter para cerrar el mensaje y se le mostrará un
símbolo del sistema:

    $

Puede parecer que no ha pasado nada, pero el hecho es que `screen` ya ha creado y
gestionado su primera sesión y ventana. El prefijo de comando de la pantalla es
`Ctrl + a`. Para ver todas la ventanas en la parte inferior de la pantalla de terminal
escriba `Ctrl + w-a`:

    0*$ bash

¡Ahí está, nuestra única ventana hasta ahora! Sin embargo, tenga en cuenta que el
conteo inicia en 0. Para crear otra ventana, escriba `Ctrl + a-c`. Verá un mensaje.
Comencemos con `ps` en esa nueva ventana:
```sh
ps
PID TTY
TIME CMD
974 pts/2 00:00:00 bash
981 pts/2 00:00:00 ps
```
y escriba `Ctrl + a` nuevamente:

    0-$ bash 1*$ bash

Ahí tenemos nuestras dos ventanas (observe el asterisco que indica la que se está
mostrando en este momento). Sin embargo, como comenzamos con bash, ambas reciben el
mismo nombre. Ya que invocamos `ps` nuestra ventana actual, cambiaremos el nombre con el
mismo nombre. Para eso, debe escribir `Ctrl + a-A` y escribir el nombre de la nueva
ventana (`ps`) cuando se le solicite:

    Set window's title to: ps

Ahora, creemos otra ventana pero proporcione un nombre desde el principio:
`yetanotherwindow`. Esto se hace invocando `screen` con el modificador `-t`:

    screen -t yetanotherwindow

Puede moverse entre ventanas de diferentes formas:
- Usando `Ctrl + a-n` (ir a la ventana siguiente) y `Ctrl + a-p` (ir a la ventana anterior).
- Usando `Ctrl + a-número` (vaya a la ventana "número").
- Usando `Ctrl + a-"` para ver una lista de todas las ventanas. Puede moverse hacia arriba y abajo con las teclas de flecha y seleccione la que desee con Enter.

Para eliminar una ventan, simplemente finalice el programa que se está ejecutando en
ella (una vez que se elimine la última ventan, `screen` terminará). Alternativamente,
use `Ctrl + a-k` mientras está en la ventana que desea eliminar; se le pedira
confirmación:
```sh
Really kill this window [y/n]
Window 0 (bash) killed.
```

#### Regiones
`screen` puede dividir la pantalla en un terminal en varias regiones para acomodar las
ventanas. Estas ventanas pueden ser horizontales (`Ctrl + a-S`) o verticales (`Ctrl +a-I`).

lo único que mostrará la nueva región es simplemente -- en pa parte inferior, lo que
significa que está vacío.

Para moverse a la nueva región, escriba `Ctrl + a-Tab`. Ahora puede agregar una nueva
ventana mediante cualquiera de los métodos, por ejemplo: `Ctrl + a-2`. Ahora el --
debería haberse convertido en `2 yetanotherwindow`:

Los aspectos importantes a tener en cuenta al trabajar con regiones son:
- Puede moverse entre regiones escribiendo `Ctrl + a-Tab`.
- Puede terminar todas las regiones excepto la actual con `Ctrl + a-Q`.
- Puede terminal la región actual con `Ctrl + a-X`.
- Terminar una región no termina su ventana asociada.

#### Sesiones
Hasta ahora hemos jugado con alguna ventanas y regiones, pero todas pertenecen a la
misma y única sesión. Es hora de empezar a jugar con sesiones. Para ver una lista de
todas las sesiones, teclee `screen -list` o `screen -ls`:
```sh
screen -list
There is a screen on:
    1037.pts-0.debian       (08/24/19 13:53:35)     (Attached)
1 Socket in /run/screen/S-carol.
```
Esa es nuestra única sesión hasta ahora:
- **PID**: `1037`
- **Name**: `pts-0.debian` (indicando el terminal -en nuestro caso un psuedo terminal esclavo -y el nombre del host).
- **Status**: `Attached`

Creemos una nueva sesión dándole un nombre más descriptivo:

    screen -S "second session"

La patanlla de la terminal se borrará y se le dará un nuevo mensaje. Puede comprobar
las sesiones una vez más:

    screen -ls

Para cerrar una sesión, salga de todas sus ventanas o simplemente escriba el comando
`screen -S SESSION-PID -X quit` (también puede proporcionar el nombre de la sesión).
Deshagámonos de nuestra primera sesión:

    screen -S 1037 -X quit

Se le enviará de nuevo al indicador de su terminal fuera de la `screen`. Pero recuerde, nuestra segunda sesión todavía está viva. Sin embargo, dado que matamos su sesión
principal, se le asigna una nueva etiqueta: `Detached`.

#### Desvincular sesiones
Por varias razones, es posible que desee desconectar una sesión de pantalla de su
terminal:
- Para permitir que su computadora en el trabajo haga su trabajo y se conecte de forma remota más tarde desde casa.
- Para compartir una sesión con otros usuarios.

Desconecte una sesión con la combinación de teclas `Ctrl + a-d`. Volverá a su terminal.

Para vincular nuevamente a la sesión, use el comando `screen -r SESSION-PID`.
Alternativamente, puede usar el `SESSION-NAME`. Si solo hay una sesión desvinculada,    ninguna es obligatoria:

    screen -r

Opciones importantes para volver a adjuntar la sesión:
- **`-d -r`**: vuelva a conectar una sesión y, si es necesario, desconéctela primero.
- **`-d -R`**: igual que `-d -r` pero `screen` incluso creará la sesión primero si no existe.
- **`-d -RR`**: igual que `-d -R`. Sin embargo, utilice la primera sesión si hay más de una disponible.
- **`-D -r`**: vuelve a conectar una sesión. Si es necesario, desconecte y cierre la sesión de forma remota primero.
- **`-D -R`**: si está ejecutando una sesión, vuelva a conectarla.
- **`-D -RR`**: lo mismo que `-D -RR` solo que más fuerte.
- **`-d -m`**: inicie `screen` en modo independiente. Esto crea una nueva sesión, pero no se vincula a ella. Esto es útil para los scripts de inicio del sistema.
- **`-D -m`**: igual que `-d -m`, pero no bifurca un nuevo proceso. El comando sale si la sesión termina.

#### Copiar y pegar: modo Scrollback
GNU Screen presenta un modo de copia o *scrollback*. Una vez ingresado, puede moverse el
cursor en la ventana actual y por el contenido de su historial usando las teclas de
flechas. Puede marcar texto y copiarlo en ventanas. Los pasos a seguir son:
1. Ingresar al modo copia/scrollback: `Ctrl + a-[`.

2. Vaya al principio del texto que desea copiar usando las teclas de flecha.

3. Marque el comienzo del fragmento de texto que desea copiar: Espacio.

4. Vaya al final del fragmento de texto que desea copiar usando las teclas de flecha.

5. Marque el final del fragmento de texto que desea cpiar: Espacio.

6. Vaya a la ventana de su selección y pegue el fragmento de texto: `Ctrl + a-]`.

#### Personalización de Screen
El archivo de configuración de todo el sistema para screen es `/etc/screenrc`.
Alternativamente, se puede usar un `~/.screenrc` a nivel de usuario. El archivo incluye
cuatro secciones principales de configuración:

**`SCREEN SETTINGS`**: puede definir la configuración general especificando la directiva
seguida de un espacio y el valor como en `defscrollback 1024`.

**`SCREEN KEYBINGINGS`**: esta sección es bastante interesante ya que permite redefinir
combinaciones de teclas que quizás interfieran con su uso diarior del terminal. Utilice
la palabra clave `bind` seguida de un espaco, el carácter que se utilizará después del
prefijo del comando, otro espacio y el comando en: `bind l kill` (este configuración
cambiará la forma predeterminada de matar una ventana a `Ctrl + a-l`).

**`TERMINAL SETTINGS`**: esta sección incluye configuraciones relacionadas con el tamaño
de la ventana de la terminal y los búferes, entre otros. para habilitar el modo sin
bloqueo para manejar mejor las conexiones ssh inestables, por ejemplo, se utiliza la
siguiente configuración: `defnonblock 5`.

**`STARTUP SCREENS`**: puede incluir comandos para que varios programas se ejecuten en
el inicio de la `pantalla`; por ejemplo: `screen -t top top` (`screen` abrirá una
ventana llamada `top` con `top` adentro).

#### tmux
`tmux` fue lanzado en 2007. Aunque es muy similar a `screen`, incluye algunas
diferencias notables:
- Modelo cliente-servidor: el servidor suministra una serie de sesiones, cada una de las cuales puede tener varias ventanas vinculadas que, a su vez, pueden ser compartidas por varios clientes.
- Selección interactiva de sesiones, ventanas y clientes a través de menús.
- La misma ventana se puede vincular a varias sesiones.
- Disponibilidad de diseños de teclas vim y Emacs.
- Soporte para terminales UTF-8 y 256 colores.

#### Ventanas
`tmux` se puede invocar simplemente escribiendo `tmux` en el símbolo del sistema. Se le
mostrará un indicador de shell y una barra de estado en la parte inferior de la vetana:
```sh
[0] 0:bash*         "debian" 18:53
27-Aug-19
```
Aparte del `hostname`, la hora y la fecha, la barra de estado proporciona la siguiente
información:

- **`Session name`**: `[0]`
- **`Window number`**: `0:`
- **`Window name`**: `bash*`. Por defecto, este es el nombre del programa que se ejecuta dentro de la ventana y, diferencia de `screen`, `tmux` lo actualizará automáticamente para reflejar el programa en ejecución actual. Note el asterisco que indica que esta es la ventana visible actual.

Puede asignar nombre se sesión y ventana al invocar `tmux`:

    tmux new -s LPI - "window zero"

La barra de estado cambiará en consecuencia:
```sh
[LPI] 0:Window zero*        "debian" 19:01
27-Aug-19
```
El prefijo de comand de tmux es `Ctrl + b`. Para crear una nueva ventana, simplemente
escriba `Ctrl + b-c`; se le llevara a un nuevo prompt y la barra de estado replejará
la nueva ventana.

Como Bash es el shell subyacente, la nueva ventana recibe ese nombre de forma
predeterminada. Inicie `top` y vea cómo cambia el nombre a `top`.

En cualquier caso, puede cambiar el nombre de una ventan con `Ctrl + b-,`. Cuando le
solicite, proporcione el nuevo nombre y presione Enter:

    (rename-window) Window one

Puede mostrar todas las ventanas para su selección con `Ctrl + b-w` (use las teclas de
flechas para moverse hacia arriba y abajo y `enter` para seleccionar).

De manera similar a `screen`, podemos saltar de una ventana a otra con:
- **`Ctrl + b-n`**: ir a la siguiente ventana
- **`Ctrl + b-p`**: ir a la ventana anterior
- **`Ctrl + b-número`**: ir a la ventan número

Para deshacerse de una ventan, use `Ctrl + b-&`. Se le pedirá confirmación.

Otros comandos de ventana interasantes incluyen:
- **`Ctrl + b-f`**: buscar ventana por nombre
- **`Ctrl + b-.`**: cambiar el número de indice de la ventana

#### Paneles
La función de división de ventanas de `screen` también está presente en `tmux`. Sin
embargo, las divisiones resultantes no se llaman regiones, sino paneles. La diferencia
más importante entre regiones y paneles es que estos últimos psuedo-terminales
completas vinculdas a una ventan. Esto significa que matar un panel también matará
su pseudo-terminal y cualquier programa asociado que se ejecute dentro.

Para dividir una ventana horizontalmente, usamos `Ctrl + b-"`.

Para dividir una ventan verticalmente, usamos `Ctrl + b-%`.

Para destruir el panel actual (junto con su psuedo-terminal que se ejecuta dentro de
él, junto con cualquier programa asociado), use `Ctr + b-x`. Se le pedirá confirmación
en la barra de estado.

Comandos de panel importantes:
- **`Ctrl + b-↑, ↓, ←, →`**: moverse entre paneles
- **`Ctrl + b-;`**: pasar al último panel activo
- **`Ctrl + b-Ctrl + arrow key`**: cambiar el tamaño de panel.
- **`Ctrl + b-Alt + arrow key`**: cambiar el tamaño del panel en cinco líneas.
- **`Ctrl + b-{`**: intercambiar paneles (actual a anterior).
- **`Ctrl + b-}`**: intercambiar paneles (actual a siguiente).
- **`Ctrl + b-z`**: panel de acercar/alejar
- **`Ctrl + b-t`**: `tmux` muestra un reloj elegante (deténgalo presionando `q`).
- **`Ctrl + b-!`**: convertir el panel en ventana

#### Sesiones
Para listar sesiones en `tmux` puede usar `Ctrl + b-s` o usar el comando `tmux ls`.

Solo hay una sesión (`LPI`) que incluye dos ventanas. Creemos una nueva sesión desde
nuetra sesión actual. Esto se puede lograr usando `Ctrl + b`, teclee `:new` en el
indicador, luego presione Enter. Se le enviará a la nueva sesión. Por defecto, 
`tmux` nombró la sesión `2`. Para cambiar el nombre, use `Ctrl + b-$`. Cuando se le
solicite, ingrese el nuevo nombre y presione Enter.

Puede cambiar de sesión con `Ctrl + b-s` (use la teclas de flechas y `enter`).

Para cerrar una sesión, puede usar el comando `tmux kill-session -t session-name`.

#### Desvincular sesiones
Al matar a `Second session` estamos fuera de `tmux`. Sin embargo, todavía tenemos una
sesión activa. Pidale a `tmux` una lista de sesiones y seguramente la encontrara allí:

    tmux ls

Sin embargo, esta sesión está separada de su terminal. Podemos vincularla con
`tmux a -t session-name`. Cuando solo hay una sesión, la especificación del nombre es
opcional.

Comandos importantes para vincular/desvincular sesiones:
- **`Ctrl + b-D`**: seleccione qué cliente Desvincular
- **`Ctrl + b-r`**: actualizar la terminal del cliente

#### Copiar y pegar: modo Scrollback
`tmux` también presenta el modo scrollback básicamente de la misma manera que `screen`.
La única diferencia en cuanto a los comandos es usar `Ctrl + Espacio` para marcar el
comienzo de la selección y `Alt + w` para copiar el texto seleccionado.

#### Personalización de tmux
Los archivos de configuración para `tmux` se encuentran normalmente en `/etc/tmux.con` y
`~/.tmux.conf`. Cuando se inicia, `tmux` utiliza estos archivos si existen. También
existe la posibilidad de iniciar `tmux` con el parámetro `-f` para proporcionar un
archivo de configuración alternativo. Puede encontrar un ejemplo de archivo de
configuración `tmux` ubicado en `/usr/share/doc/tmux/example_tmux.conf`. El nivel de    personalización que se puede logar es realmente alto. Algunas de las cosas que puede
hacer incluyen:
- Cambiar la tecla de prefijo
```sh
# Change the prefix key to C-a
set -g prefix C-a
unbind C-b
bind C-a send-prefix
```
- Establecer combinaciones de teclas adicionales para ventanas superiores a 9
```sh
# Some extra key bindings to select higher numbered windows
bind F1 selectw -t:10
bind F2 selectw -t:11
bind F3 selectw -t:12
```

## Modificar la prioridad de ejecución de los procesos
Los sistemas operativos capaces de ejecutar más de un proceso al mismo tiempo se
denominan sistemas multitarea o multiprocesamiento. Si bien la verdadera simultaniedad
solo ocurre cuando hay más de una unidad de procesamiento disponible, incluso los
sistemas de un solo procesador puede imitar la simueltaniedad al cambiar entre procesos
muy rápidamente. Esta técnica también se emplea en sistemas con muchas CPU equivalentes,
o sistemas multiprocesador simétrico (SMP), dado que el número de procesos concurrentes
potenciales superan con creces el número de unidades procesadores disponibles.

De hecho, solo un solo proceso a la vez puede controlar la CPU. Sin embargo, la
mayoría de las actividades del proceso son llamadas el sistema, es decir, el proceso
en ejecución transfiere el control de la CPU al proceso de un sistema operativo para
que realice la operación solicitada. Las llamadas al sistema están a cargo de toda la
comunicación entre dispositivos, como asignación de memoria, lectura y escritura en
sistemas de archivos, impresión de texto en la pantalla, interacción del usuario,
transferencias de red, etc. La tranferencia del control de CPU duraten una llamada al
sistema permite, que el sistema operativo decida si devolver el control de la CPU al
proceso anterior o pasarlo a otro proceso. Como las CPU modernas pueden ejecutar
instrucciones mucho más rápido de lo que la mayoría de hardware externo puede
comunicarse entre sí, un nuevo proceso de control puede hacer mucho trabajo de la CPU
mientras que las respuestas de hardware solicitadas anterriormente aún no están
disponibles. Para garantizar el máximo aprovechamiento de la CPU, los sistema operativos
de multiprocesamiento mantiene una cola dinámica de procesos activos en espera de un
intervalo de tiempo de la CPU.

Aunque permite mejorar significativamente la utilización del tiempo de la CPU,
depender únicamente de las llamadas del sistema para cambiar entre procesos no es
suficiente para logar un rendimiento multitarea satisfactorio. Un proceso que no
realiza llamadas sl sistema podría controlar la CPU de forma indefinida. Esta es la
razón por lo que los sistemas modernos también son preferentes, es decir, un proceso
en ejecución se puede volver a poner en la cola para que un proceso más importante
pueda controlar la CPU, incluso si el proceso en ejecución no ha realizado una llamada
al sistema.

#### El planificador de Linux
linux, como sistema operativo de multiprocesamiento preventivo, implementa un
planificador que organiza la cola de procesos. Más precisamente, el planificador
también decide qué subproceso en cola se ejecutará (un proceso puede ramificar muchos
subprocesos independientes), pero procesos y subprocesos son términos intercambiales
en este contexto. Cada proceso tiene dos predicados que intervienen en su programación:
la política de programación y la prioridad de programación.

Hay dos tipos principales de políticas de programación: políticas en tiempo real y
políticas normales. Los procesos bajo una política de tiempo real se programan
directamente por sus valores de prioridad. Si un proceso más importante está listo
para ejecutarse, se sustituye por un proceso en ejecución menos importante y el
proceso de mayor prioridad toma el control de la CPU. Un proceso de menor prioridad
obtendrá el control de la CPU solo si los procesos de mayor prioridad están inactivos
o esperando las respuestas de hardware.

Cualquier proceso en tiempo real tiene mayor prioridad que un proceso normal. Como
sistema operativo de propósito general, Linux ejecuta solo algunos procesos en tiempo
real. La mayoría de los procesos, incluidos los programas de usuario y del sistema, se
ejecutan según las políticas de programación normales. Los procesos normales suelen
tener el mismo valor de prioridad, pero las políticas normales pueden definir reglas de
prioridad de ejecución utilizando otro predicado de proceso: el valor *nice*. Para
eviar confuciones con las prioridads dinámicas derivadas de valores agradables, las
prioridades de programación generalmente se denomina prioridades de programación
esáticas.

El planificador de Linux se puede configurar de muchas formas diferentes y existen
formas aún más complejas de establecer prioridades, pero estos conceptos generales
siempre se aplican. Al inspeccionar y ajustar la programación del proceso, es
importante tener en cuenta que solo se verán afectados los procesos bajo la política
de programación normal.

#### Prioridades de lectura
Linux reserva prioridades estáticas que van de 0 a 99 para procesos en tiempo real y
los procesos normales se asignan a prioridades estáticas que van de 100 a 139, lo
que significa que hay 39 niveles de prioridad diferentes para procesos normales. Los
valores más bajos significan una prioridad más alta. La prioridad estática de un
proceso activo se puede encontrar en el archivo `sched`, ubicado en su directorio
respectivo dentro del sistema de archivos `/proc`:
```sh
grep ^prio /proc/1/sched
prio    :   120
```
Como se muestra en el ejemplo, la línea que comienza con `prio` da el valor de prioridad
del proceso (el proceso PID 1 es el proceso init o systemd, el primer proceso que
el kernel inicia durante la inicialización del sistema). La prioridad estándar para
los procesos normales es 120, de modo que se puede reducir a 100 o aumentar a 139. Las
prioridades de todos los procesos en ejecución se pueden verificar con el comando
`ps -Al` o `ps -el`:

    ps -el

La columan `PRI` indica la prioridad estática asignada por el kernel. Sin embargo,
tenga en cuenta que el valor de prioridad mostrador por `ps` difiere el obtenido en el
ejemplo anterior. Debido a razones históricas, las prioridades mostradas por `ps`
varían de -40 a 99 por defecto, por la que la prioridad real se obtiene agregando
40 (en particular, 80 + 40 = 120).

También es posible monitorear continuamente los procesos que actualmente administra
el kernel de Linux con el programa `top`. Al igual que con `ps`, `top` también muestra 
el valor de prioridad de forma diferente. Para que sea más fácil indentificar los
procesos en tiempo real, `top` resta 100 del valor de prioridad, por lo que todas las
prioridades en tiempo real son negativas, con un número negativo o *rt* 
indentificandolas. Por lo tanto, las prioridades normales mostradas por `top` van de
0 a 39.

#### Niceness de procesos
Todos los procesos normales comienzan con un valor nice predeterminado de 0 (prioridad
120). El nombre *nice* proviene de la idea de que los procesos "más agradables"
permiten que otros procesos se ejecuten antes que ellos en una cola de ejecución
particular. Los números nice van desde -20 (menos agradables, alta prioridad) a 19
(más agradables, baja prioridad). Linux también permite la capacidad de asignar
diferentes valores *nice* a subprocesos dentro del mismo proceso. La columna `NI` en la
salida de `ps` indica el número *nice*.

Solo el usuario root puede disminuir el valor nice de un proceso por debajo de cero.Es posible iniciar un proceso con una prioridad no estándar con el comando `nice`. Por
ejemplo, `nice` cambia el valor a 10, pero se puede especificar con la opción `-n`:

    nice -n 15 tar czf home_backup.tar.gz /home

En este ejemplo, el comando `tar` se ejecutara con un valor nice de 15. El comando
`renice` puede usarse para cambiar la prioridad de un proceso en ejecución. La opción
`-p` indica el número PID del proceso objetivo. Por ejemplo:
```sh
renice -10 -p 2164
2164 (process ID) old priority 0, new priority -10
```
Las opciones `-g` y `-u` se utilizan para modificar todos los procesos de un grupo o
usuario específico, respectivamente. Con `renice +5 -g users`, el valor nice de los
procesos propiedad de los usuarios del grupo *users* se elevará en cinco.

Además de `renice`, la prioridad de los procesos se pueden modificar con otros
programas, como el programa `top`. En la pantalla principal superior, el valor nice de
un proceso se puede modificar presionando `r` y luego el número PID del proceso.

Aparece el mensaje `PID to renice [default pid = 1]` con el primer proceso listado
seleccionado por defecto. Para cambiar la prioridad de otro proceso, escriba su PID
y presione Enter. Luego, aparecerá el mensaje `Renice PID 1 to value` (con el
número PID solicitado) y se puede asignar un nuevo valor nice.

## Realizar búsquedas de archivos de textos usando expresiones regulares
Los argoritmos de búsqueda de cadenas son ampliamente utilizados por varias tareas de   procesamiento de datos, tanto que los sistemas operativos similares a Unix tiene su
propia implementación ubicua: *Expresiones regulares (Regular expression)*, a menudo
abreviadas como *REs*. Las expresiones regulas consisten en secuencias de caracteres
que forman un patrón genérico que se utilizar para localizar y, a veces, modificar
una secuecia correspondiente en una cadena de caracteres más grande. Las expresiones
regulares amplían enormemente la capacidad de:
- Escribir reglas de análisis para solicitudes en servidor HTTP, nginx en particular.
- Escribir scripts que conviertan conjuntos de datos basados en texto a otro formato.
- Buscar ocurrencias de interés en entradas de diario o documentos. 
- Filtrar docuentos de marcado, manteniendo el contenido semático.

Las expresiones regulares más simples contienen al menos un átomo. Un átomo, llamado
así porque es el elemento básico de una expresión regular, es solo un carácter que
puede tener o no un significado especial. la mayoría de los caracteres ordianarios
son inequívocos, conservan su significado literal, mientras que otros tienen un 
siguiendo especial:
- **`. (punto)`**: átomo coincide con cualquier carácter
- **`^ (signo de intercalación)`**: átomo coincide con el comienzo de una línea.
- **`$ (signo de dólar)`**: átomo coincide con el final de una línea.

Por ejemplo, la expresión regular `bc`, compuesta por los átomos literales b y c, se
pueden encontrar en la cadena `abcd`, pero no en la cadena `a1cd`. Por otro lado, la
expresión regular `.c` puede encontrarse en ambas cadenas de caracteres `abcd` y `a1cd`,
ya que el punto `.` coincide con cualquier carácter.

Los átomos del signo de intercalación y dólar se utilizan cuando solo son de interés
la concidencias al principio o al final de la cadeana. Po eso también se le llama
*anchors* (anclas). Por ejemplo, `cd` se puede encontrar en `abcd`, pero `^cd` no. De
manera simiilar, `ab` se puede encontrar en `abcd`, pero `ab$` no. El signo de
intercalación `^` es un carácter literal excepto cuando está al principio y `$` es un
carácter literal excepto cuando está al final de la expresión regular.

#### Expresión de corchetes
Hay otro tipo de átomo llamado *bracket expression* (expresión de corchetes). Aunque
no es un solo carácter, los corchetes `[]` (incluido su contenido) se consideran un
solo átomo. Una expresión entre corchetes suele ser solo una lista de caracteres
literales encerrados por `[]`, haciendo que el átomo coincida con cualquier carácter de
la lista. Por ejemplo, la expresión `[1b]` se puede encontrar en ambas cadenas `abcd` y
`a1cd`. Para especificar los caracteres a lo que no debe corresponder el átomo, la lista
debe comenzar cpm `^`, como en `[^1b]`. También es posible especificar rangos de 
caracteres en expresiones entre corchetes. Por ejemplo, `[0-9]` concide con los dígitos
del 0 al 9 y `[a-z]` coincide con cualquier letra minúscula. Los rangos deben usarse
con precaución, ya que pueden no ser consistentes en distintas configuraciones
regionales.

Las listas de expresiones entre corchetes también aceptan clases en lugar de solo
caracteres y rangos individuales. Las clases de caracteres tradicionale son:
- **`[:ascii:]`**: representa un carácter que encaja en el juego de caracteres ASCII.
- **`[:blank:]`**: representa un carácter en blanco, es decir, un espacio o una tabulación.
- **`[:cntrl:]`**: representa un carácter de control.
- **`[:digit:]`**: representa un dígito (0 a 9).
- **`[:graph:]`**: representa cualquier caŕacter imprimible excepto el espacio.
- **`[:lower:]`**: representa un carácter en minúscula.
- **`[:print:]`**: representa cualquier carácter imprimible, incluido el espacio.
- **`[:punct:]`**: representa cualquier carácter imprimible que no sea un espacio ni un carácter alfanumérico.
- **`[:space:]`**: representa caracteres de espacio en blanco: espacio, avance de formulario (`\f`), nueva línea (`\n`), retorno de carro (`\r`), tabulación horizontal (`\t`) y tabulación veritcal (`\v`).
- **`[:upper:]`**: representa una letra en mayúscula.
- **`[:xdigit:]`**: representa dígitos hexadecimales (de 0 a F).

Las clases de caracteres se pueden combinar con caracteres y rangos individuales,
pero no se pueden como punto final de un rango. Además, las clases de caracteres pueden
usarse solo en expresiones de corchetes, no como un átomo independiente fuera de los
corchetes.

#### Cuantificadores
El ancance de un átomo, ya sea de un átomo de un solo carácter o un átomo de corchete,
se puede ajustar utilizando un cantificador de átomos. Los cuantificadores de átomos
definen secuencias de átomos, es decir, las coincidencias ocurren cuando se encuentra
una repetición contigua para el átomo de la cadena. Las subcadena correspondiente a la
coincidencia se llama pieza. No obstante, los cuantificadores y otras características
de las expresiones regulares se tratan de manera diferente según el estándar que se
utilice.

Según lo define POSIX, hay dos tipos de expresiones regulares: expresiones regulares
"básicas" y expresiones regulares "extendidas". la mayoría de los programas
relacionados con el texto en cualquier distribución convencional de Linux admite ambas
formas, por lo que es importante conocer sus diferencias para evitar problemas de
compatibilidad y elegir la implementación más adecuada para la tarea prevista.

El cuantificador `*` tiene la misma función tanto en los RE básicos como en los
extendidos (el átomo aparece cero o más veces) y es un carácter literal si aparece al
principio de la expresión regular o si está precedido por una barra invertida `\`.
El cuantificador de signo más `+` seleccionará piezas que contengan una o más
coincidencias de átomos de secuencia. Con el cuantificador de signo de interrogación
`?`, se producira una coincidencia si el átomo correspondiente aparece una vez o si no
aparece en absoluto. Si está precedido por una barra invertida `\`, se obvia su
significado especial. Las expresiones regulares básicas también adminten cuantificadores
`+` y `?`, pero deben ser precedidas de una barra invertida. A diferencia de las
expresiones regulares extenidadas, `+` y `?` por sí mismo son caracteres literales en
expresiones regulares básicas.

#### Límites
Un *bound* (límite) es un cuantificador de átomos que, como su nombre lo indica, permite
al usuario especificar límites cuantitativos precisos para un átomo. En las expresiones
regulares extendidas, un límite puede aparecer de tres formas:
- **`{i}`**: el átomo debe aparecer exactamente `i` veces (`i` un número entero). Por ejemplo, `[[:blank:]]{2}` coincide con exactamente dos caracteres en blanco.
- **`{i,}`**: el átomo debe aparecer al menos `i` veces (`i` un número entero). Por ejemplo, `[[:blank:]]{2,}` coincide con cualquier secuencia de dos o más caracteres en blanco.
- **`{i,j}`**: el átomo debe aparecer al menos `i` veces y como máximo `j` veces (`i` y `j` números enteros, `j` mayor que `i`). Por ejemplo, `xyz{2,4}` coincide con la cadena `xy` seguida entre dos y cuatro caracteres `z`.

En cualquier caso, si una subcadena coincide con una expresión regular y una subcadena
más larga que comienza en el mismo punto también coincide, se considerará la subcadena
más larga.

Las expresiones regulares básicas también admiten límites, pero los delimitadores deben
estar precedidos por `\:\{ y \}`. Por sí mismo, `{ y }` se interpretan como caracteres
literales. Un `\{` seguido de un carácter que no sea un dígito es un carácter literal,
no el comienzo de un límite.

#### Ramas y referencias posteriores
Las expresiones regulares básicas también difieren de las expresiones regulares
extendidas en otro aspecto importante: una expresión regular extendida se puede dividir
en ramas, cada una de las cuales es una expresión regular independiente. Las ramas
están separadas por `|` y la expresión regular combinada coincidirá con cualquier cosa
que corresponda a cualquiera de las ramas. Por ejemplo, `he|him` coincidirá si la
subcadena `he` o `him` se encuentran en la cadena que se está examinando. Las
expresiones regulares básicas interpretan `|` como un carácter literal. Sin embargo,
la mayoría de los programas que soportan expresiones regulares básicas permitirán
ramificaciones con `\|`.

Una expresión regular extendida encerrada entre `()` se puede usar en una referencia
posterior. Por ejemplo, `([[:digit]])\1` coincidirá con cualquier expresión regular
que se repita al menos una vez, porque el `\1` en la expresión es la referencia
posterior a la pieza que coincide con la primera subexpresión entre paréntesis. Si
existe más de una subexpresión entre paréntesis en la expresión regular, se puede
hacer referencia a ellas con `\2`, `\3`, etc.

Para REs básicas, las subexpresiones deben estar delimitadas por `\( y )`, con `( y )`
por sí mismos caracteres ordinarios. El índice de referencia inversa se usa del
mismo modo que en las expresiones regulares extendidas.

#### Búsqueda con expresiones regulares
El beneficio inmediato que ofrece las expresiones regulares es mejorar las búsquedas
en sistemas de archivos y documentos de texto. La opción `-regex` del comando `find`
permiten probar cada ruta en una jerarquía de directorios contra una expresión regular.
Por ejemplo:

    find $HOME -regex '.*/\..*' -size +100M

Busca archivos de más de 100 megabytes, pero solo en rutas dentro del directorio de
inicio del usuario que contiene una coincidencia con `.*/\..*`, es decir, un `/.`
rodeado por cualquier otro número de caracteres. En otras palabras, solo se
enumerarán los archivos ocultos o los archivos dentro de los directorios ocultos,
independientemente de las posiciones de `/.` en la ruta correspondiente. Para
expresiones regulares que no distinguen entre mayúsculas y minúsculas, se usará en
su lugar de la opción `-iregex`:
```sh
find /usr/share/fonts -regextype posix-extended -iregex
'.*(dejavu|liberation).*sans.*(italic|oblique).*'
/usr/share/fonts/dejavu/DejaVuSansCondensed-BoldOblique.ttf
/usr/share/fonts/dejavu/DejaVuSansCondensed-Oblique.ttf
/usr/share/fonts/dejavu/DejaVuSans-BoldOblique.ttf
/usr/share/fonts/dejavu/DejaVuSans-Oblique.ttf
/usr/share/fonts/dejavu/DejaVuSansMono-BoldOblique.ttf
/usr/share/fonts/dejavu/DejaVuSansMono-Oblique.ttf
/usr/share/fonts/liberation/LiberationSans-BoldItalic.ttf
/usr/share/fonts/liberation/LiberationSans-Italic.ttf
```
En este ejemplo, la expresión regular contien ramas (escritad en modo extendido) para
listar solo archivos de fuentes específicos bajo la jerarquía de directorios
`/usr/share/fonts`. Las expresiones regulares extendidas no son compatibles de forma
predeterminada, pero `find` permite habilitarlas con `-regextype posix-extended` o
`-regextype egrep`. El estándar RE predeterminado para `find` es *findutils-default*,
que es prácticamente un clon básico de expresión regular.

A menudos es necesario pasar la salida de un programa al comando `less` cuando no
cabe en la pantalla. El comando `less` divide su entrada en páginas, una pantalla
completa a la vez, lo que permite al usuario navegar fácilmente por el texto hacia
arriba y abajo. Además, `less` también permite al usuario realizar búsquedas basadas
en expresiones regulares. Esta característica es notablemente importante porque
`less` es el paginador predeterminado que se usa para muchas tareas diarias, como
inspeccionar entradas de diarior o consultar páginas de manul. Al leer una página de
manual, por ejemplo, al presionar la tecla `/` se abrirá un mensaje de búsqueda.
Este es un escenario típico en el que las expresiones regulares son útiles, ya que
las opciones de comando se enumaran justo después de un margen de página. Sin embargo,
la misma opción puede aparecer muchas veces en el texto, lo que hace que las
búsquedas literales sean inviables. Independientemente de eso, al escribir
`^[[:blank:]]-o` o de manera más simple: `^-o` (en el indicador de búsqueda y presionar
Enter se saltará inmediatamente a la sección `-o`, permitiendo consultar la descripción
de una opción de manera más rápida).

#### El buscador de patrones: `grep`
Uno de los usos más comunes de `grep` es facilitar la inspección de archivos largos,
utilizando la expresión regular con filtro aplicado a cada linea. Se puede usar para
mostrar solo las líneas que comienzan con un término determinado. Por ejemplo,
`grep` se puede usar para inspeccionar un archivo de configuración con módulos del
kernel, mostrando solo las líneas de opciones:
```sh
grep '^options' /etc/modprobe.d/alsa-base.conf
options snd-pcsp index=-2
options snd-usb-audio index=-2
options bt87x index=-2
options cx88_alsa index=-2
options snd-atiixp-modem index=-2
options snd-intel8x0m index=-2
options snd-via82xx-modem index=-2
```
El carácter de barra vertical `|` se puede usar para redirigir la salida de un comando
directamente a la entrada de `grep`. El siguiente ejemplo usa una expresión entre
corchetes para seleccionar las líneas de la salida de `fdisk -l`, comenzando con
`Disk /dev/sda` o `Disk /dev/sda`:
```sh
fdisk -l | grep '^Disk /dev/sd[ab]'
Disk /dev/sda: 320.1 GB, 320072933376 bytes, 625142448 sectors
Disk /dev/sdb: 7998 MB, 7998537728 bytes, 15622144 sectors
```
La mera selección de línas con coincidencias puede no ser apropiada para una tarea
en particular, requiriendo ajustes en el comportamiento de `grep` a través de sus
opciones. Por ejemplo, la opción `-c` o `--count` le dice a `grep` que muestre cuántas
líneas tenían coincidencias:
```sh
fdisk -l | grep '^Disk /dev/sd[ab]' -c
2
```
La opción se puede colocar antes o después de la expresión regular. Otrad opciones
importantes de grep son:
- **`-c o --count`**: en lugar los resultados de la búsqueda, solo muestra el recuento total e cuántas veces se produce una coincidencia en un archivo determinado.
- **`i o --ingore-case`**: hace que la búsqueda no distinga entre mayúscula y minúsculas.
- **`-f FILE o --file=FILE`**: indica un archivo que contenga la expresión regular a utilizar.
- **`-n o --line-number`**: muestra el número de la línea.
- **`-v o --invert-match`**: selecciona todas las línas, excepto las que contengan coincidencias.
- **`-H o --with-filename`**: imprime también el nombre del archivo que contiene la línea.
- **`-z o --null-data`**: en lugar de que `grep` trate los datos de flujo de entrada y salida como líneas separadas (usando newline por defecto), toma la entrada o salida como una secuencia de líneas. Cuando se combina la salida del comando `find` usando la opción `-print0` con el comando `grep`, la opción `-z` o `--null-data` deben usarse para procesar el flujo de la misma manera.

Aunque se activa de forma predeterminada cuando se porporcionan varias rutas de archivo
como entrada, la opción `-H` no está activada para archivos individuales. Eso puede ser
crítico en situaciones especiales, como cuando `grep` es llamado directamente por   `find`, por ejemplo:
```sh
find /usr/share/doc -type f -exec grep -i '3d modeling' "{}" \; | cut -c -100
artistic aspects of 3D modeling. Thus this might be the application you are
This major approach of 3D modeling has not been supported
oce is a C++ 3D modeling library. It can be used to develop CAD/CAM softwares, for
instance [FreeCad]
```
En este ejemplo, `find` lista todos los archivos en `/usr/share/doc` y luego pasa cada
uno a `grep`, que a su vez realiza una búsqueda que no distingue entre mayúsculas y
minúsculas de `3d modeling` dentro del archivo. La tuberia está ahí solo para limitar
la longitud de salida a 100 columnas. Sin embargo, tenga en cuenta que no hay forma
de saber de que archivo provienen las líneas. Este problema se resuelve agregando
`-H` a `grep`:
```sh
find /usr/share/doc -type f -exec grep -i -H '3d modeling' "{}" \; | cut -c -100
/usr/share/doc/openscad/README.md:artistic aspects of 3D modeling. Thus this might
be the applicatio
/usr/share/doc/opencsg/doc/publications.html:This major approach of 3D modeling has
not been support
```
Ahora es posible identificar los archivos donde se encontró cada coincidencia. Para
que la lista sea aún más informativo, se puede agregar líneas iniciales y finales
a las líneas con coincidencias:
```sh
find /usr/share/doc -type f -exec grep -i -H -1 '3d modeling' "{}" \; | cut -c -100
/usr/share/doc/openscad/README.md-application Blender), OpenSCAD focuses on the CAD
aspects rather t
/usr/share/doc/openscad/README.md:artistic aspects of 3D modeling. Thus this might
be the applicatio
/usr/share/doc/openscad/README.md-looking for when you are planning to create 3D
models of machine p
/usr/share/doc/opencsg/doc/publications.html-3D graphics library for Constructive
Solid Geometry (CS
/usr/share/doc/opencsg/doc/publications.html:This major approach of 3D modeling has
not been support
/usr/share/doc/opencsg/doc/publications.html-by real-time computer graphics until
recently.)
```
La opción `-1` le indica a `grep` que incluya una lína antes y una línea después cuando
encuentre una línea con una coincidencia. Estas líneas adicionales se denominan
líneas de contexto y se identifican en la salida con un signo menos después del
nombre del archivo. Se puede obtener el mismo resultado con `-C` o `--context=1` y se
pueden indicar otras cantidades de líneas de contexto.

Hay dos programas complementarios a grep: `egrep` y `fgrep`. El programa `egrep` es
equivalente al comando `grep -E`, que incorpora características adicionales además de
las expresiones regulares básicas. Por ejemplo, con `egrep` es posible usar
características extendidas de expresión regular, como ramificaciones:
```sh
find /usr/share/doc -type f -exec egrep -i -H -1 '3d (modeling|printing)' "{}" \;
| cut -c -100
/usr/share/doc/openscad/README.md-application Blender), OpenSCAD focuses on the CAD
aspects rather t
/usr/share/doc/openscad/README.md:artistic aspects of 3D modeling. Thus this might
be the applicatio
/usr/share/doc/openscad/README.md-looking for when you are planning to create 3D
models of machine p
/usr/share/doc/openscad/RELEASE_NOTES.md-* Support for using 3D-Mouse / Joystick /
Gamepad input dev
/usr/share/doc/openscad/RELEASE_NOTES.md:* 3D Printing support: Purchase from a
print service partne
/usr/share/doc/openscad/RELEASE_NOTES.md-* New export file formats: SVG, 3MF, AMF
/usr/share/doc/opencsg/doc/publications.html-3D graphics library for Constructive
Solid Geometry (CS
/usr/share/doc/opencsg/doc/publications.html:This major approach of 3D modeling has
not been support
/usr/share/doc/opencsg/doc/publications.html-by real-time computer graphics until
recently.)
```
En este ejemplo, `3D modeling` o `3D printing` coincidirán con la expresión, no
dinstingue entre mayúsculas y minúsculas. Para mostrar solo las partes de un flujo
de texto que coinciden con la expresión utilizada por `egrep`, use la opción `-o`.

El programa `fgrep` es equivalente a `grep -F`, es decir, no analiza expresiones
regulares. Es útitl en búsquedas simples donde el objetivo es hacer coincidir una
expresión literal. Por lo tanto, los caracteres especiales como el signo de dólar y
el punto se tomarán literalmente y no por su significado en una expresión regular.

#### El editor de trasmisiones: `sed`
El propósito del programa `sed` es modificar datos basados en texto de una manera no
interactiva. Significa que toda le edición se realiza mediante instrucciones
predefinidas, no escribiendo arbitrariamente directamente en un texto que muestra en
pantalla. En términos modernos, `sed` puede entenderse como un analizador de plantillas:
dando un texto como entrada, coloca contenido personalizado en posiciones predefinidas
o cuando encuentra una coincidencia para una expresión regular.

Sed, como su nombre lo indica, es muy adecuado para texto transmitido a través de
tuberías. Su sintaxis básica es `sed -f SCRIPT` cuando las instrucciones de edición
se almacenan en el archivo `SCRIPT` o `sed -e COMMANDS` para ejecutar `COMMANDS`
directamente desde la línea de comandos. Si no hay `-f` ni `-e`, `sed` utiliza el
primer parámetro que no es una opción como archivo de script. También es posible
usar un archivo como entrada simplemente dando su ruta como argumento a `sed`.

Las instrucciones `sed` se componen de un solo carácter, posiblemente precedidas por
una dirección o seguidas de una o más opciones, y se aplican a cada línea a la vez.
Las direcciones pueden ser un número de una sola línea, una expresión regular o un
rango de líneas. Por ejemplo, la primera línea de una secuencia de texto se puede
eliminar con `1d`, donde `1` especifica la línea donde se aplicará el comando de    eliminación `d`.
```sh
factor `seq 12`
1:
2: 2
3: 3
4: 2 2
5: 5
6: 2 3
7: 7
8: 2 2 2
9: 3 3
10: 2 5
```
La eliminación de la primera línea con `sed` se logra mediante `1d`:
```sh
factor `seq 12` | sed 1d
2: 2
3: 3
4: 2 2
5: 5
6: 2 3
7: 7
8: 2 2 2
9: 3 3
10: 2 5
11: 11
12: 2 2 3
```
Se puede especificar un rango de líneas separadas por coma:
```sh
factor `seq 12` | sed 1,7d
8: 2 2 2
9: 3 3
10: 2 5
11: 11
12: 2 2 3
```
Se puede utilizar más de una instrucción en la misma ejecución, separadas por punto
y coma. En este caso, sin embargo, es importante encerrarlos entre paréntesis para
que el punto y coma no sea interpretado por el shell:
```sh
factor `seq 12` | sed "1,7d;11d"
8: 2 2 2
9: 3 3
10: 2 5
12: 2 2 3
```
En este ejemplo, se ejecutaron dos instrucciones de eliminación, primero en las líneas
que van de 1 al 7 y luego en la línea 11. Una dirección también puede ser una
expresión regular, por lo que solo las líneas con coincidencias se verán afectadas por
la instrucción:
```sh
factor `seq 12` | sed "1d;/:.*2.*/d"
3: 3
5: 5
7: 7
9: 3 3
11: 11
```
La expresión regular `:.*2.*` coinciden con cualquier aparición del número 2 en
cualquier lugar después de dos puntos, lo que provoca la eliminación de las líneas
correspondientes a los números con 2 como factor. Con `sed`, cualquier cosa colocada
entre barras (`/`) se consideran una expresión regular y. pordefecto, se admiten todos
los REs básicos. Por ejemplo, `sed -e "/^#/d" /etc/services` muestra el contenido del
archivo `/etc/service` sin las líneas que comienzan con `#`.

La instrucción de eliminación `d` es solo una de las muchas instrucciones de edición
proporcionadas por `sed`. En lugar de eliminar una línea, `sed` puede reemplazarla con
un texto dado:
```sh
factor `seq 12` | sed "1d;/:.*2.*/c REMOVED"
REMOVED
3: 3
REMOVED
5: 5
REMOVED
7: 7
REMOVED
9: 3 3
REMOVED
11: 11
REMOVED
```
La instrucción `c REMOVED` simplemente reemplaza una lína con el texto `REMOVED`. En
el caso del ejemplo, cada línea con una subcadena que coincida con la expresión
regular `:.*2.*` se ve afectada por la instrucción `c REMOVED`. La instrucción `a TEXT`
copia el texto indicado por `TEXT` en una nueva línea después de la línea con una
coincidencia. La instrucción `r FILES` hace los mismo, pero copia el contenido del
archivo indicado por `FILE`. La instrucción `w` hace lo contrario de `r`, es decir, la
línea se agregará al archivo indicado.

Con mucho, la instrucción `sed` más utilizada es `s/FIND/REPLACE/`, que se utiliza
para reemplazar una coincidencia de la expresión regular `FIND` con el texto indicado
por `REPLACE`. Por ejemplo, la instrucción `s/hda/sda` reemplazara una subcadena que
coincide con el literal RE `hda` con `sda`. Solo se reemplazara la primera coincidencia
que se encuentra en la línea, a menos que la opción `g` se coloque después de la
instrucción, como en `s/hda/sda/g`.

Un estudio de caso más realista ayudará a ilustrar las características de `sed`.
Suponga que una clínica médica desea enviar mensajes de texto a sus clientes,
recondándoles sus citas programadas para el día siguiente. Un escenario de
implementación típico se basa en un servicio de mensajería instantánea profesional,
que proporciona una API para acceder al sistema responsable de entregar los mensajes.
Estos mensajes generalmente se originan en el mismo sistema que ejecuta la aplicación
que controla las citas del cliente, activados a una específica del día o algún
otro evento. En esta situación hipotética, la aplicación podría generar un archivo
llamado `citas.csv` que contengan los datos tabulados con todas las citas para el día
siguiente, luego usando por `sed` para procesar los mensajes de texto de un archivo de
platilla llamado `template.txt`. Los archivos CSV son una forma estándar de exportar
datos de consultas de base de datos, por lo que las citas de muestras se pueden
proporcionar de la siguiente manera:
```sh
cat appointments.csv
"NAME","TIME","PHONE"
"Carol","11am","55557777"
"Dave","2pm","33334444"
```
La primera línea contiene las etiquetas de cada columna, que se utilizarán para hacer
coincidir las etiquetas dentro del archivo de plantilla de muestra:
```sh
cat template.txt
Hey <NAME>, don't forget your appointment tomorrow at <TIME>.
```
Los signos de menos de `<` y de mayor que `>` se colocaron alrededor de las etiquetas
solo para ayudar a indetificarlas como etiquetas. El siguiente script de Bash
analiza todas las citas en una cola usando `template.txt` como plantilla de mensaje:
```sh
#! /bin/bash
TEMPLATE=`cat template.txt`
TAGS=(`sed -ne '1s/^"//;1s/","/\n/g;1s/"$//p' appointments.csv`)
mapfile -t -s 1 ROWS < appointments.csv
for (( r = 0; r < ${#ROWS[*]}; r++  ))
    do
    MSG=$TEMPLATE
    VALS=(`sed -e 's/^"//;s/","/\n/g;s/"$//' <<<${ROWS[$r]}`)
    for (( c = 0; c < ${#TAGS[*]}; c++  ))
    do
        MSG=`sed -e "s/<${TAGS[$c]}>/${VALS[$c]}/g" <<<"$MSG"`
    done
    echo curl --data message=\"$MSG\" --data phone=\"${VALS[2]}\"
https://mysmsprovider/api
done
```
Un script de producción real también controlaría la autenticación, la verificación de
errores y el registro. Las primeras instrucciones ejecutadas por `sed` se aplican solo
a la primera línea -the address `1` in `1s/^"//;1s/","/\n/g;1s/"$//p`- para eliminar
las comillas iniciales y finales -`1s/^"//"` y `1s/"$//"`- y reemplazara los separadores
de campo con un carácter de nueva línea: `1s/","/\n/g`. Solo se necesita la primera
línea para cargar los nombres de las columnas, por lo que las líneas que no coincidan
se suprimirán con la opción `-n`, lo que requiere que la marca `p` se coloque después
del último comando `sed` para imprimir la línea coincidente. Luego, las etiquetas
se almacenan en la variable `TAG` como una matriz Bash. Otra variable de tipo matriz
es creada por el comando `mapfile` para almacenar las líneas que contienen las citas
en la variable de matriz `ROWS`.

Se emplea un bucle `for` para procesar cada línea de cita que se encuentra en `ROWS`.
Luego, las comillas y los separadores en la cita - la cita está en la variable
`${ROWS[$r]}` usada como un here string - se reemplazara por `sed`, de manera similar
a los comandos usados para cargar las etiquetas. Los valores separados para la cita
se almacenan en la variable de matriz `VALS`, donde los subíndices de matriz 0, 1 y 2
corresponden a los valores `NAME`, `TIME` y `PHONE`.

Finalmente, un bucle anidado `for` recorre la matriz `TAGS` y reemplaza cada etiqueta
que se encuentra en la plantilla con su valor correspondiente en `VALS`. La variable
`MSG` contiene una copia de la plantilla renderizada, actualizada por el comando de
sustitución `s/<${TAGS[$c]}>/${VALS[$c]}/g` en cada iteración de bucle a través de
`TAGS`.

Esto da como resultado un mensaje como: `"Hey Carol, don’t forget your appointment tomorrow at 11am."`. El mensaje se puede enviar como un parámetro a través de una
solicitud HTTP con `curl`, como un mensaje de correo o cualquier otro método similar.

#### Combinando `grep` y `sed`
Los comandos `grep` y `sed` pueden usarse juntos cuando se requieren procedimientos de
minería de texto más complejos. Como administrador del sistema, es posible que desee
inspeccionar todos los intentos de inicio de sesión en un servidor, por ejemplo. El
archivo `/var/log/wtmp` registra todos los inicios y cierres de seesión, mientras que
el archivo `/var/log/btmp` registra los intentos fallidos de inicio de sesión. Están
escritos en formato binario, que se pueden leer con los comandos `last` y `lastb`,
respectivamente.

La salida de `lastb` muestra no solo el nombre de usuario utilizado en el intento de
inicio de sesión incorrecto, sino tambien su dirección IP:
```sh
lastb -d -a -n 10 --time-format notime
user
ssh:notty (00:00) 81.161.63.251
nrostagn ssh:notty (00:00) vmd60532.contaboserver.net
pi ssh:notty (00:00) 132.red-88-20-39.staticip.rima-tde.net
pi ssh:notty (00:00) 132.red-88-20-39.staticip.rima-tde.net
pi ssh:notty (00:00) 46.6.11.56
pi ssh:notty (00:00) 46.6.11.56
nps ssh:notty (00:00) vmd60532.contaboserver.net
narmadan ssh:notty (00:00) vmd60532.contaboserver.net
nominati ssh:notty (00:00) vmd60532.contaboserver.net
nominati ssh:notty (00:00) vmd60532.contaboserver.net
```
La opción `-d` traduce el número IP al nombre de host correspondiente. El nombre de host
puede proporcionar pistas sobre el ISP o el servicio de alojamiento utilizado para
realizar estos intentos de inicio de sesión incorrectos. La opción `-a` coloca
el nombre de host en la última columna, lo que facilita el filtrado aún por aplicar.
La opción `--time-format notime` suprime la hora en que se produjo el intento de inicio
de sesión. El comando `lastb` puede tardar algún tiempo en completarse si hubo 
demasiados intentos de inicio de sesión incorrectos, por lo que la salida se limitó
a diez con la opción `-n 10`.

No todas las direcciones IP remotas tienen un nombre de host asociado, por lo que el
DNS inverso no se aplica a ellas y se pueden descartat. Aunque se podría escribir
una expresión regular para que coincida con el formato esperado para un nombre de
host al final de la línea, probablemente sea más sencillo escribir una expresión
regular para que coincida con una letra del alfabeto o con un solo dígito al final de
la línea. El siguiente ejemplo muestra cómo el comanndo `grep` toma el listado en
su entrada estándar y elimina las líneas sin nombre de host:
```sh
lastb -d -a --time-format notime | grep -v '[0-9]$' | head -n 10
nvidia
ssh:notty (00:00) vmd60532.contaboserver.net
n_tonson ssh:notty (00:00) vmd60532.contaboserver.net
nrostagn ssh:notty (00:00) vmd60532.contaboserver.net
pi ssh:notty (00:00) 132.red-88-20-39.staticip.rima-tde.net
pi ssh:notty (00:00) 132.red-88-20-39.staticip.rima-tde.net
nps ssh:notty (00:00) vmd60532.contaboserver.net
narmadan ssh:notty (00:00) vmd60532.contaboserver.net
nominati ssh:notty (00:00) vmd60532.contaboserver.net
nominati ssh:notty (00:00) vmd60532.contaboserver.net
nominati ssh:notty (00:00) vmd60532.contaboserver.net
```
El comando `grep` con la opción `-v`, muestra solo las líneas que no coincidan con la
expresión regular dada. Una expresión regular que coincida con cualquier línea que
termine con un número (es decir, `[0-9]$`) capturará solo las entradas sin un nombre
de host. Por lo tanto, `grep -v [0-9]$` monstrará solo las líneas que terminen con un
nombre de host.

La salida se puede filtrar aún más, manteniendo solo el nombre de dominio y eliminando
las otras partes de cada línea. El comando `sed` puede hacerlo con un comando se
sustitución para reemplazar todas las líneas con una referencia al nomre de dominio
con ella:
```sh
lastb -d -a --time-format notime | grep -v '[0-9]$' | sed -e 's/.* \(.*\)$/\1/' | head -n 10
vmd60532.contaboserver.net
vmd60532.contaboserver.net
vmd60532.contaboserver.net
132.red-88-20-39.staticip.rima-tde.net
132.red-88-20-39.staticip.rima-tde.net
vmd60532.contaboserver.net
vmd60532.contaboserver.net
vmd60532.contaboserver.net
vmd60532.contaboserver.net
vmd60532.contaboserver.net
```
El parámetro de escape en `.* \(.*\)$` indica a `sed` que recuerde esa parte de la
línea, es decir, la parte entre el último carácter de espacio y el final de la línea.
En el ejemplo, se hace referencia a esta parte con `\1` y se usa para reemplazar la
línea completa.

Está claro que la mayoría de los host remotos intentan iniciar sesión más de una vez,
por lo que el mismo nombre de dominio se repite. Para suprimir las entradas repetidas,
primero se deben ordenar (con el comando `sort`) y luego pasar el comando `uniq`:
```sh
lastb -d -a --time-format notime | grep -v '[0-9]$' | sed -e 's/.* \(.*\)$/\1/' | sort | uniq | head -n 10
116-25-254-113-on-nets.com
132.red-88-20-39.staticip.rima-tde.net
145-40-33-205.power-speed.at
tor.laquadrature.net
tor.momx.site
ua-83-226-233-154.bbcust.telenor.se
vmd38161.contaboserver.net
vmd60532.contaboserver.net
vmi488063.contaboserver.net
```
Esto muestra cómo se pueden combinar diferentes comandos para producir el resultado
deseado. La lista de nombres de host se pueden utilizar para escribir reglas de
bloqueo de firewall o para tomar otras medidas para hacer cumplir la seguirdad del
servidor.

## Dispositivo, sistemas de archivos Linux y el estándar de jerarquía de archivos
En cualquier sistema sistema opetaivo, es necesario particionar un disco antes de
poder usarlo. Una partición es un subconjunto lógio del disco físico y la información
sobre las particiones se almacena en una tabla de particiones. Esta tabla incluye
información sobre el primer y último sector de la partición y su tipo, y más detalles
sobre cada partición.

Por lo general, un sistema operativo considera cada partición como un "disco" separado,
incluso si reside en el mismo medio físico. En los sistemas Windwos se les asigna letras
como `C:` (historicamente el disco principal), `D:` y así sucesivamente. En Linux, cada
partición se asigna a un directorio en `/dev`, como `/dev/sda1` o `/dev/sda2`.

#### Comprensión de MBR y GPT
Hay dos formas principales de almacenar información de paritción en discos duros.
El primero es MBR (Master Boot Record) y el segundo es GPT (GUID Partition Table).

**MBR**: este es un remanente de los primeros días de MS_DOS (más específicamente, 
PC_DOS 20.0 de 1983) y durante décadas fue el esquema de particiones estádar en las PC.
La tablla de particiones se almacena en el primer sector del disco, llamado
*Boot Sector*, junto con un cargador de arranque, que en los sistemas Linux suele ser el
GRUB. Pero MBR tiene una serie de limitaciones que dificultan su uso en sistemas
modernos, como la imposibilidad de utilizar discos de más de 2 TB de tamaño y el
límite de solo 4 particiones primarias por disco.

**GUID**: un sistema de particiones que aborad muchas de las limitaciones de MBR. No
existe un límite práctico en el tamaño del disco, y el número máximo de particiones
está limitado solo por propio sistema operativo. Se encuetra más comúnmente en máquinas
más modernas que usa UEFI en lugar del antiguo BIOS de PC.

Durante las tareas de administración del sistema es muy posible que encuentre amboses   quemas en uso, por lo que es importante saber como usar las herramientas asociadas a
cada uno para crear, eliminar y modificar particiones.

#### Gestión de particiones MBR con FSIDK
La utilidad estándar para administrar particiones MBR en Linux es `fdisk`. Esta es una
utilidad interactiva basada en menús. Para usarlo, teclee `fdisk` seguido del nombre del
dispositivo correspondiente al disco que deea editar. Por ejemplo, el comando:

    fdisk /dev/sda

Editaría la tabla de particiones del primer dispositivo conectado a SATA (`sda`) en
el sistema. Tenga en cuenta que debe especificar el dispositivo correspondiente al
disco físico, no una de sus particiones (como `/dev/sda1`).

Cuando se invoca, `fdisk` monstrará un saludo, luego una advertencia y esperará sus
comandos.
```sh
fdisk /dev/sda
Welcome to fdisk (util-linux 2.33.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.
Command (m for help):
```
La advertencia es importante. Puede crear, editar o elimnar particiones a voluntad,
pero no se escribirá nada en el disco a menos que utilice el comando write (`w`). Por
lo tanto, puede "practicar" sin riesgo de perder datos, siempre que se mantenga alejado
de la tecla `w`. Para salir de `fdisk` sin guardar cambios, use el comando `q`.

#### Impresión de la tabla de particiones actual
El comando `p` se usa para imprimir la tabla de particiones actual. La salida sería algo
como esto:
```sh
Command (m for help): p
Disk /dev/sda: 111.8 GiB, 120034123776 bytes, 234441648 sectors
Disk model: CT120BX500SSD1
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x97f8fef5
Device
Boot
Start
/dev/sda1
/dev/sda2
End
Sectors
Size Id Type
4096 226048942 226044847 107.8G 83 Linux
226048944 234437550
8388607
4G 82 Linux swap / Solaris
```
Aquí esta el significado de cada columna:
- **`Device`**: el dispositivo asignado a la partición
- **`Boot`**: muestra si la partición es de arranque o no
- **`Start`**: el sector donde comienza la partición
- **`End`**: el sector donde termina la partición
- **`Sectors`**:  el número total de sectores en la partición. Multiplíquelo por el tamaño del sector para obtener el tamaño de la particón en bytes.
- **`Size`**: el tamaño de la partición en formato legible para humanos. En el ejemplo anterior, los valores están en gigabytes.
- **`Id`**: el valor númerico que representa el tipo de partición.
- **`Type`**: la descripción del tipo de partición

#### Particiones primarias vs extendidas
En un disco MBR, puede tener 2 tipos principale de particiones, primarias y extendidas.
Como dijimos antes, solo puede tener 4 particiones primarias en el disco, y si desea
que el disco sea "de arranque", la primera partición debe ser primaria.

Una forma de ecitar esta limitación es crear una partición extendida que actúe como
contenedor de particiones lógicas. Podría tener, por ejemplo, una partición primaria,
una partición extendida que ocupa el resto del espacio del disco y cinco particiones
lógicas dentro de ella.

Para un sistema operativo como Linux, las particiones primarias y extendidas se tratan
exactamente de la misma manera, por lo qe no hay "ventajas" de usar una sobre la otra.

#### Crear una partición
Para crear una partición, use el comando `n`. De forma predeterminada, las particiones
se crearán al comienzo del espacio no asignado en el disco. Se le pedirá el tipo de
partición (primaria o extendida), primer sector y último sector.

Para el primer sector, normalmente puede aceptar el valor predeterminado sugerido por
`fdisk`, a menos que necesite una partición para comenzar en un sector específico. En
lugar de especificar el último sector, puede especificar un tamaño seguido de las letras
`K`, `M`, `G` o `P`. Por lo tanto, si desea crear una partición de 1 GB, puede
especificar `+1G` como el último sector y `fdisk` dimensionará la partición en
consecuencia. Vea este ejemplo para la creación de un partición primaria:
```sh
Command (m for help): n
Partition type
    p primary (0 primary, 0 extended, 4 free)
    e extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-3903577, default 2048): 2048
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-3903577, default 3903577): +1G
```
#### Comprobación de espacio no asignado
Si no sabe cuánto espacio libre hay en el disco, puede usar el comando `f` para mostar
el espacio no asignado, así:
```sh
Command (m for help): F
Unpartitioned space /dev/sdd: 881 MiB, 923841536 bytes, 1804378 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
Start
End Sectors Size
2099200 3903577 1804378 881M
```
#### Eliminar particiones
Para eliminar una partición, use el comando `d`. `fdisk` le pedirá el número de la
partición que desea eliminar, a menos qye haya solo una partición en el disco. En este
caso, esta partición será seleccionada y eliminada inmediatamente.

Tenga en cuenta que si elimina un partición extendida, también se eliminarán todas las
particiones lógicas que contiene.

#### ¡Cuidado con la brecha!
Tengas en cuenta que al crear una nueva partición con `fdisk`, el tamaño máximo se
limitará a la cantidad máxima de espacio contiguo no asignado en el disco. Digamos, por
ejemplo, que tiene el siguiente mapa de particiones:
```sh
Device  Boot    Start   End Sectors Size Id Type
/dev/sdd1 2048 1050623 1048576 512M 83 Linux
/dev/sdd2 1050624 2099199 1048576 512M 83 Linux
/dev/sdd3 2099200 3147775 1048576 512M 83 Linux
```
Luego borre la partición 2 y compruebe si hay espacio libre:
```sh
Command (m for help): F
Unpartitioned space /dev/sdd: 881 MiB, 923841536 bytes, 1804378 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes

Start   End Sectors Size
1050624 2099199 1048576 512M
3147776 3903577 755802 269M
```
Sumando el tamaño del espacio no asignado, en teoría tenemos 881 MB disponible. Pero
visualice lo que sucede cuanto intentamos crear una partición de 700 MB:
```sh
Command (m for help): n
Partition type
    p primary (2 primary, 0 extended, 2 free)
    e extended (container for logical partitions)
Select (default p): p
Partition number (2,4, default 2): 2
First sector (1050624-3903577, default 1050624):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (1050624-2099199, default 2099199):
+700M
Value out of range.
```
Eso sucede porque el espacio contiguo no asignado más grande en el disco es el bloque
de 512 MB que pertenecía a la partición 2. Su nueva partición no puede "pasar" la
partición 2 para usar parte del espacio no asignao después de ella.

#### Cambiar el tipo de partición
En ocasiones, es posible que deba cambiar el tipo de partición, especialmente cuando
se trata de disco que utilizarán en otro sistemas operativo o plataformas. Esto de hace
con el comando `t`, seguido del número de la partición que desea cambiar.

El tipo de patición debe ser especificado por su código hexadecimal correspondiente, y
puede ver una lista de todos los códigos válidos usando el comando `l`.

No confunga el tipo de partición con el sistema de archivos que se utiliza en ella.
Aunque al principio existía una relación entre ellos, hoy no se puede asumir que esto
sea cierto. Una partición de Linux, por ejemplo, puede contener cualquier sistema de
sistema de archivos nativo de Linux, como ext4 o ReiserFS.

#### Administrar particiones GUID con GDISK
La utilidad `gdisk` es el equivalente a `fdisk` cuando se trata de discos particionados
GPT. De hecho, la interfaz está modelada a partir de `fdisk`, con un indicador
interactivo y los mismos (o muy similares) comandos.

#### Impresión de la tabla de particiones actual
El comando `p` se usa para imprimir la tabla de particiones actual. La salida sería algo
como esto:
```sh
Command (? for help): p
Disk /dev/sdb: 3903578 sectors, 1.9 GiB
Model: DataTraveler 2.0
Sector size (logical/physical): 512/512 bytes
Disk identifier (GUID): AB41B5AA-A217-4D1E-8200-E062C54285BE
Partition table holds up to 128 entries
Main partition table begins at sector 2 and ends at sector 33
First usable sector is 34, last usable sector is 3903544
Partitions will be aligned on 2048-sector boundaries
Total free space is 1282071 sectors (626.0 MiB)
Number  Start (sector)  End (sector)    Size    Code Name
1 2048 2099199 1024.0 MiB 8300 Linux filesystem
2 2623488 3147775 256.0 MiB 8300 Linux filesystem
```
Desde el princio, notamos algunas cosas diferentes:
- Cada disco tiene un identificado de disco (GUID) único. Este es un número hexadecimal de 128 bits, asignado al azar cuando se crea la tabla de particiones. Dado que hay 3.4 x 10^38 valores posibles para este número, las posibilidades de que 2 discos aleatorios tengan el mismo GUID son bastante escasas. El GUID se puede usar para identificar qué sistemas de archivos montar en el momento del arranque (y dónde), eliminando la necesidad de usar la ruta del dispositivo para hacerlo (como `/dev/sda`).
- ¿Ve la frade `Partition table holds up to 128 entries`? Así es, se puede tener hasta 128 particiones en un disco GPT: Debido a esto, no hay necesidad de paticiones primarias y extendidas.
- El espacio libre aparece en la última línea, por lo que no es necesario un equivalente del comando `F` de `fdisk`.

#### Crear una patición
El comando para crear una partición es `n` al igual que en `fdisk`. La principal
diferencia es que además del número de partición y el primer y último sector, también
puede especificar el tipo de partición durante la creación. Las particiones GPT
admiten más tipos que MBR. puede consultar una lista de todos los tipos admitidos
mediante el comando `l`.

#### Eliminar particiones
Para eliminar una partición, escriba `d` y el número de partición. A diferencia de
`fdisk`, la primera partición no se seleccionará automáticamente si es la única en el
disco.

En los discos GPT, las particiones se pueden reordenar u "ordenar" fácilemente para
evitar huecos en secuencia de numeración. Para hacer esto, simplemente use el comando
`s.` Por ejemplo, imagine un disco con la siguiente tabla departiciones:

#### ¿Brecha? ¿Qué brecha?
A diferencia de los disco MBR, al crear una partición en discos GTP, el tamaño no está
limitado por la cantidad máxima de espacio contiguo no asignado. Puede utilzar el
último bit de un sector libre, sin importart dónde se encuentre en el disco.

#### Opciones de recuperación
Los dicos GPT almacenan copias de seguridad del encabezado GPT y las tabla de
particiones, lo que facilita la recuperación de discos en caso de que estos datos se
hayan dañado. `gdisk` proporciona funciones para ayudar en esas tareas de recuperación,
a las que se accede con el comando `r`.

Puede recontruir un encabezado GPT principal corrupto o una tabla de particiones con
`b` y `c`, respectivamente, o usar el encabezado princpal y la tabla para reconstruir
una copia de seguiridad con `d` y `e`. Tambien puede convertir un MBR a GPT con `f` y
hacer lo contrario con `g`, entre otras opciones. Escriba `?` en el menú de recuperación
para obtener una lista de todos los comandos de recuperación disponibles y descripciones
sobre lo que hacen.

#### Creación de sistemas de archivos
Particionar el disco es solo el primer paso para poder usar un disco. Después de eso,
deberá formatear la partición con un sistema de archivos antes de usarla para
almacenar datos.

Un sistema de archivos controla cómo se almacena los datos y cómo se accede a ellos en
el disco. Linux admite muchos sistema de archivos, algunos nativos, como la familia
ext (Extended Filesystem), mientras que otros porvienen de otros sistemas operativos
como Fat de MS-DOS, NTFS de Windows NT, HFS y HFS+ de Mac OS, etc.

La herramienta estándar utilizada para crear un sistema de archivos en Linux es
`mkfs`, quien viene en muchos "sabores" según el sistema de archivos con el que necesita
trabajar.

#### Creación de un sistema de archivos ext2/ext3/ext4
El sistema de archivos extendido (ext) fue el primer sistema de archivos para Linux,
y a través de los años fue reemplazado por nuevas versiones llamadas ext2, ext3 y ext4,
que actualmente es el sistema de archivos predeterminado para muchas distribuciones de
Linux.

Las utilidades `mkfs.ext2`, `mkfs.ext3` y `mkfs.ext4` se utilizan para crear sistemas de
archivos ext2, ext3 y ext4 respectivamente. De hecho, todas estas utilidades existen
solo como enlaces simbólicos a otra utilidad llamada `mke2fs`. `mke2fs` modifica sus
valores predeterminados de acuerdo con el nombre por el que se le llama. Como tal,
todos tiene el mismo comportamiento y parámetros de línea de comando.

La forma de uso más sencilla es:

    mkfs.ext2 TARGET

Donde `TARGET` es el nombre de la partición donde se debe crear el sistema de archivos.
Por ejemplo, para crear un sistema de archivos ext3 en `/dev/sda1` el comando sería:

    mkfs.ext3 /dev/sda1

En lugar de usar el comando correspondiente al sistema de archivos que desea crear,
puede pasar el parámetro `-t` a `mke2fs` seguido del nombre del sistema de archivos.
Por ejemplo, los siguientes comandos son equivalentes y crearán un sistema de archivos
en `/dev/sda1`:
```sh
mkfs.ext4 /dev/sda1
mke2fs -t ext4 /dev/sda1
```

#### Parámetros de línea de comandos
`mke2fs` adminte una amplia gama de opciones y parámetros de línea de comandos. Estos
son algunos de los más importantes. Todos ellos también se aplican a `mkfs.ext2`,
`mkfs.ext3` y `mkfs.ext4`:
- **`-b SIZE`**:  establece el tamaño de los bloques de datos en el dispositivo en `SIZE`, que puede ser de 1024, 2048o 4096 bytes por bloque.
- **`-c`**: comprueba el dispositivo de destino en busca de bloques defectuosos antes de crear el sistema de archivos. Puede ejecutar una comprobación completa, per mucho más lenta, pasando esre parámetro dos veces, como en `mkfs.ext4 -c -c TARGET`.
- **`-d DIRECTORY`**: copia el contenido del directorio especificafo en la raíz del nuevo sistema de archivos. Útil si necesita "rellenar previamente" el disco con un conjunto predefinido de archivos.
- **`-F`**: esta opción forzará mke2fs a crear un sistema de archivos, incluso si las otras opciones pasadas a él o al objetivo son peligrosas o no tienen ningún sentido. Si se especifica dos veces, incluso se puede usar para crear un sistema de archivos en un dispositivo que está montado o en uso, lo cual es muy, muy malo.
- **`-L VOLUME_LABEL`**: establecerá la etiqueta de volumen en la especificada en `VOLUME_LABEL`. Esta etiqueta debe tener un máximo de 16 caracteres.
- **`-n`**: esta es una opción realmente útil que simula la creación del sistema de archivos y muestra lo que se haría si se ejecuta sin la opción `n`. Piense en ello como un modo de "prueba". Es bueno comprobar las cosas antes de realizar cambios en el disco.
- **`-q`**: modo silencioso. `mke2fs` se ejecutará normalmente, pero no producirá ninguna salida en la terminal. üitl cuando se ejecuta `mke2fs` desde un script.
- **`-U ID`**: esto establecerá un UUID de una partición en el valor especificado como ID. Los UUID son números de 128 bits de notación hexadecimal que sirven para identificar de forma única una partición en el sistema operativo. Este número se especifica como una cadena de 32 dígitos en el formato 8-4-4-4-12, es decir, 8 dígitos, guión, 4 dígitos, guión, 4 dígitos, guión, 4 dígitos, guión, 12 dígitos, como `D249E380-7719-45A1-813C-35186883987E`. En lugar de un ID, también
    puede especificar parámetros como `clear` para borrar el UUID del sistema de archivos, `random`, para usar un UUID generado aleatoriamente o `time` para crear un UUID basado en el tiempo.
- **`-V`**: modo detallado, imprime mucha más informacion durante el funcionamiento de lo habitual. ütil para fines de depuración.

#### Creación de un sistema de archivos XFS
XFS es un sistema de archivos de alto redimiento desarrollado originalmente por Silicon
Graphic en 1993 para su sistema operativo IRIX. Debido a sus característica de
rendimiento y confiabilidad, se usa comúnmente para servidores y otros entorn que
requiern un ancho de banda alto (o garantizado) del sistema de archivos.

Las herramientas para administrar sistemas de archivos XFS son parte del paquete
`xfsprogs`. Es posible que este paquete deba instalarse manualmente, ya que no se
incluye de forma predeterminada en algunas sitribuciones de Linux. Otros, como Red
Hat Enterprise Linux 7, usan XFS como sistema de archivos predeterminado.

Los sistema de archivos XFS se dividen en al menos 2 partes, una sección de registrodonde    se mantiene un registro de todas las operaciones del sistema de
archivos (comúnmente llamado Journal) y la sección de datos. La secci´on de registro
puede estar ubicada dentro de la sección de datos (el comportamiento predeterminado),
o incluso en un disco separado por completo, para un mejor rendimiento y confiabilidad.

El comando más básico para crear un sistema de archivos XFS es `mkfs.xfs TARGE`,
donde `TARGET` es la partición en la que se desea que se cree el sistema de archivos.
Por ejemplo: `mkfs.xfs /dev/sda1`.

Como en `mke2fs`, `mkfs.xfs` adminte una serie de opciones de línea de comandos. Estos
son algunas de las más comues:
- **`-b size=VALUE`**: establece el tamaño del bloque en el sistema de archivos, en butes, al especificado en `VALUE`. El valor predeterminado es 4096 bytes (4 KiB), el mínimo es 512 y el máximo es 65536 (64 KiB).
- **`-m crc=VALUE`**: los parámetros que comienzan con `-m` son opciones de etadatos. Este habilita (si `VALUES` es `1`) o deshabilita (si `VALUE` es `0`) el uso de comprobaciones CRC32c para verificar la integridad de todos los metadatos en el disco. Esto permite una mejor detección de erroes y recuperación de fallas relacionadas con problemas de hardware, por lo que está habilitado de forma predetermnada. El impacto el en rendimiento de esta comprobación debería ser mínimo, por
    lo que normalmente no hay razón para desactivarlo.
- **`-m uuid=VALUE`**: establece el UUID de la ártición al especificado como VALUE. Recuerde que los UUDI son números de 32 caracteres (128 bits) en base hexadecimal, especificados en grupos de 8, 4, 4, 4 y 12 dígitos separados por guines, como `1E83E3A3-3AE9-4AAC-BF7E-29DFFECD36C0`.
- **`-f`**: forzar la creación de un sistema de archivos en el dispositivo de destino incluso si se detecta un sistema de archivos en él.
- **`-l logdev=DEVICE`**: esto colocará la sección de registro del sistema de archivos en el dispositivo especificado, en lugar de dentro de la sección de datos.
- **`-l size=VALUE`**: esto establecerá el tamaño de la sección de registro en el especificado en `VALUE`. El tamaño se puede especificar en bytes y se pueede utilizar un sufijo como `m` o `g`. `-l size=10m`, por ejemplo, limitará la sección de registro a 10 Megabytes.
- **`-q`**: modo silencioso. En este modo, `mkfs.xfs` no imprimirá los parámetros del sistema de archivos que se está creando.
- **`-L LABEL`**: establece la etiqueta del sistema de archivos, que puede tener un máximo de 12 caracteres.
- **`-N`**: similar al parámetro `-n` de `mke2fs`, hará que `mkfs.xfs` imprima todos los parámetros para la creación del sistema de archivos, sin crearlo realmente.

#### Creación de un sistema de archivos FAT o VFAT
El sistema de archivos FAT se originó a partir de MS-DOS y, a lo largo de los años,
ha recibido muchas revisiones que culminarian con el formato FAT32 lanzado en 1996 con
Windows 95 OSR2.

VFAT es una extensión del formato FAT16 que admite nombre de archivo largos (hasta
255 caracteres). Ambos sistemas de archivos son manejados por la misma utilidad,
`mkfs.fat`. `mkfs.vfat` es un alias.

El sistema de archivos FAT tiene importantes inconvenientes que restringen su uso en
discos grandes. FAT16, por ejemplo, adminte volúmenes de 4 GB como máximo y un tamaño
de archivos máximo de 2 GB. FAT32 aumenta el tamañp del volumen hasta 2 PB y el tamaño
máximo del archivo hasta 4 GB. Debido a esot, los sistemas de archivos FAT se utilizan
hoy con más frecuencia en unidades flash pequeñas o tarjetas de memoria, o en
dispositivos y sistemas operativos heredados que no adminten sistemas de archivos más
avanzados.

El comando más básico para la creación de un sistema de archivos FAT es `mkfs.fat TAR`,
donde `TARGET` es la partición den la que desea que se cree el sistema de archivos. Por
ejemplo: `mkfs.fat /dev/sdc1`.

Como otras utilidades, `mkfs.fat` soporta una serie de opciones de línea de comandos.
A continuación se muestra los más importantes. Se puede leer una lista completa y una
descripción de cada opción en el manual de la utilidad, `man mkfs.fat`.
- **`-c`**: comprueba el dispositivo de destino en busca de los bloques defectuosos antes de crear el sistema de archivos.
- **`-C FILENAME BLOCK_COUNT`**: creará el archivo especificado en `FILENAME` y luego creará un sistema de archivos FAT dentro de él, creadno efectivamente una "imagen de disco" vacía, que luego puede escribirse en un dispositivo usando una utilidad como `dd` o montarse como un loopback dispositivo. Al usar esta opción, el número de bloque en el sistema de archivos (`BLOCK_COUNT`) debe especificarse después del nomre del dispositivo.
- **`-F SIZE`**: selecciona el tamaño de la FAT (File Allocation Table), entre 12, 16 o 32, es decir, entre FAT12, FAT16 o FAT32. si no se especifica, `mkfs.fat` seleccionará la opción apropiada según el tamaño del sistema de archivos.
- **`-n NAME`**: establece la etiqueta de volumen, o el nombre, para el sistema de archivos. Puede tener hasta 11 caracteres y el valor predeterminado es sin nombre.
- **`-v`**: modo detallado. Imprime muchas más información de la habitual, útil para depurar.

#### Creación de un sistema de archivos exFAT
exFAT es un sistema de archivos creado por Microsoft en 2006 que aborda una de las
limitaciones más importantes de FAT3: el tamaño del archivo y del disco. En exFAT, el   tamaño máximo de archivo es de 16 exabytes (desde 4 GB en FAT32) y el tamaño máximo del
disco es de 128 petabytes.

Como es compatible con los tres principales sistemas operativo (Windows, Linux y mac
OS), es una buena opción donde se necesita interoperabilidad, como en unidades flash
de gran capacidad, tarjetas de memoria y discos externos. De hcho, es el sistema de
archivos predeterminado, según lo define la SD Association, para tarjetas de memoria
SDXC de más de 32 GB.

La utilidad predeterminada para crear sistemas de archivos exFAT es `mkfs.exfat`, es
es un enlace a `mkexfatfs`. El comando más básico es `mkfs.exfar TARGET`, donde `TARGET`
es la patición en la que desea que se cree el sistema de archivos. Por ejemplo:
`mkfs.exfat /dev/sdb2`.

Al contrario de las otras utilidades, `mkgs.exfat` tiene muy pocas opciones de línea
de comandos. Son:
- **`-i VOL_ID`**: establece el ID del volumen en el valor especificado en `VOL_ID`. este es un número hexadecimal de 32 bits. Si no esa definido, es establece una ID basada en la hora actual.
- **`-n NAME`**: establece la etiqueta de volumen o el nombre. Puede tener hasta 15 caracteres y el valor predeterminado es sin nombre.
- **`-p SECTOR`**: especifica el primer sector de la primera partición del disco. Este es un valor opcional y el predeterminado es cero.
- **`-s SECTORS`**: define el número de sectores físicos por el grupo de asignación. Debe ser una potencia de dos, como 1, 2, 4, 8, etc.

#### Familiarización con el sistema de archivos Btrfs
Btrfs (oficialmente el B-Tree Filesystem, pronunciado como "Butter FS", "Better FS"
o incluso "Butterfuss") es un sistema de archivos que ha estado en desarrollo desde 2007
específicamente para Linux por el Oracle Corporation y otras empresas, incluidas
Fujitsu, Red Gat, Intel y SUSE, entre otras.

Hyas muchas características que hacen que Btrfs sea atractivo en sistemas modernos donde
son comunes grandes cantidades de almacenamiento. Entre estos se encuetran el soporte
para múltiples dispositivos (incluyendo la cración de bandas, la duplicación y la
creación de bandas+duplicación, como en una configuración RAID), compresión
transparente, optimizaciones de SSD, copias de seguridad incrementales, instantáneas, desfragmentación en línea, comprobación duera de línea, compatibilidad de
subvolúmenes (con cuotas), deduplicación y mucho más.

Como es un sistema de archivos copy-on-write, es muy resistente a los bloqueos. Y además
de eso, Btrfs es fácil de usar y está bien soportado por muchas distribuciones de
Linux. Y algunos de ellos, como SUSE, lo utilizan como sistema de archivos
predeterminado.

#### Creación de un sistemas de archivos Btrfs
La utilidad `mkfs.btrfs` se utiliza para crear un sistema de archivos Btrfs. Usar el
comando sin ninguna opción crear un sistema de archivos Btrfs en un dispositivo dado,
así:

    mkfs.btrfs /dev/sdb1

Puede usar la `-L` para establecer una etiqueta (o nombre) para su sistema de archivos.
Las etiquetas Btrfs pueden tener hasta 256 caracteres, excepto para las nuevas líneas:

    mkfs.btrfs /dev/sdb1 -L "New Disk"

Tenga en cuenta este aspecto pecular de Btrfs: puede pasar múltiples dispositivos al
comando `mkfs.btrfs`. Pasar más de un dispositivo abarcará el sistema de archivos sobre
todos los dipositivos, lo que es similar a una configuración RAID o LVM. Para
especificar cómo se distribuirán los metadatos en la matriz de discos, use el parámetros `-m`. Los parámetros válidos son `raid0`, `raid1`, `raid5`, `raid6`,
`raid10`, `single` y `dump`.

Por ejemplo, para crear un sistema de archivos que abarque `/dev/sda1` y `/dev/sdc1`,
concatenando las dos particiones en una gran partición, user:

    mkfs.btrfs -d single -m single /dev/sdb /dev/sdc

#### Gestión de subvolúmenes
Los subvolúmenes son como sistemas de archivos dentro de sistemas de archivos. Piense
en ellos como un directorio que se puede montar (y tratar como) un sistema de archivos
separado. Los subvolúmenes facilitan la organnización y la administración del sistema,
ya que cada uno de ellos puede tener cuotas o reglas instantáneas independientes.

Suponga que tiene un sistema de archivos Btrfs montado `/mnt/disk`, y desea crear un
subvolumen dentro de él para almacenar sus copias de seguirdad. Llamémoslo `BKP`:

    btrfs subvolume create /mnt/disk/BKP

A continuación, enumeramos el contenido del sistema de archivos `/mnt/disk`. Verá
que tenemos un nuevo directorio, llamado así por nuestro subvolumen.
```sh
ls -lh /mnt/disk/
total 0
drwxr-xr-x 1 root
root
0 jul 13 17:35 BKP
drwxrwxr-x 1 carol carol 988 jul 13 17:30 Images
```
Podemos comprobar que el subvolumen está activo, con el comando:
```sh
btrfs subvolume show /mnt/disk/BKP/
Name: BKP
UUID: e90a1afe-69fa-da4f-9764-3384f66fa32e
Parent UUID: -
Received UUID: -
Creation time: 2019-07-13 17:35:40 -0300
Subvolume ID: 260
Generation: 23
Gen at creation: 22
Parent ID: 5
Top level ID: 5
Flags: -
Snapshot(s):
```
Puede montar el subvolumen en `/mnt/BKP` pasando el parámetro `-t btrfs -o subvol=NAME`
al comando `mount`:

    mount -t btrfs -o subvol=BKP /dev/sdb1 /mnt/bkp

#### Trabajando con instantáneas
Las instantáneas son como subvolúmenes, pero rellenadas previamente con el contenido
del volumen desde el que se tomó la instantánea.

Cuando se crea, una instantánea y el volumen original tiene exactamente el mismo
contenid. Pero a partir de ese momento, divergirán. Los cambios realizados en el
volumen original (como archivos agregados, renombrados o eliminados) no se reflejarán
en la instantánea y viceversa.

Tenga en cuenta que una instantánea no duplica los archivos e inicialmente casi no
ocupa espacio en el disco. Simplemente duplica el árbol del sistema de archivos,
mientras apunta a los datos originales.

El comando para crear una instantánea es el mismo que se usa para crear un subvolumen,
simplemente agregue el parámetro `snapshot` después de `btrfs subvolume`. El siguiente
comando creará una instantánea del sistema de archivo Btrfs montado en `/mnt/disk` en
`/mnt/disk/snap`:

    btrfs subvolume snpashot /mnt/disk /mnt/disk/snap

Ahora, imagine que tiene el siguiente contenido en `/mnt/disk`:
```sh
ls -lh
total 2,8M
-rw-rw-r-- 1 carol carol 109K jul 10 16:22 Galaxy_Note_10.png
-rw-rw-r-- 1 carol carol 484K jul 5 15:01 geminoid2.jpg
-rw-rw-r-- 1 carol carol 429K jul 5 14:52 geminoid.jpg
-rw-rw-r-- 1 carol carol 467K jul 2 11:48 LG-G8S-ThinQ-Mirror-White.jpg
-rw-rw-r-- 1 carol carol 654K jul 2 11:39 LG-G8S-ThinQ-Range.jpg
-rw-rw-r-- 1 carol carol 2 15:43 Mimoji_Comparativo.jpg
94K jul
-rw-rw-r-- 1 carol carol 112K jul 10 16:20 Note10Plus.jpg
drwx------ 1 carol carol
366 jul 13 17:56 snap
-rw-rw-r-- 1 carol carol 118K jul 11 16:36 Twitter_Down_20190711.jpg
-rw-rw-r-- 1 carol carol 324K jul
2 15:22 Xiaomi_Mimoji.png
```
Observe el directorio de instantáneas, que contiene la instantánea. Ahora elimines
algunos archivos y verifiquemos el contenido del directorio:
```sh
$ rm LG-G8S-ThinQ-*
$ ls -lh
total 1,7M
-rw-rw-r-- 1 carol carol 109K jul 10 16:22 Galaxy_Note_10.png
-rw-rw-r-- 1 carol carol 484K jul 5 15:01 geminoid2.jpg
-rw-rw-r-- 1 carol carol 429K jul 5 14:52 geminoid.jpg
-rw-rw-r-- 1 carol carol 2 15:43 Mimoji_Comparativo.jpg
94K jul
-rw-rw-r-- 1 carol carol 112K jul 10 16:20 Note10Plus.jpg
drwx------ 1 carol carol
366 jul 13 17:56 snap
-rw-rw-r-- 1 carol carol 118K jul 11 16:36 Twitter_Down_20190711.jpg
-rw-rw-r-- 1 carol carol 324K jul
2 15:22 Xiaomi_Mimoji.png
```
Sin embargo, si revias dentro del directorio snap, los archivos que eliminó todavía
están allí y se pueden restauran si es necesario.
```sh
ls -lh snap/
total 2,8M
-rw-rw-r-- 1 carol carol 109K jul 10 16:22 Galaxy_Note_10.png
-rw-rw-r-- 1 carol carol 484K jul 5 15:01 geminoid2.jpg
-rw-rw-r-- 1 carol carol 429K jul 5 14:52 geminoid.jpg
-rw-rw-r-- 1 carol carol 467K jul 2 11:48 LG-G8S-ThinQ-Mirror-White.jpg
-rw-rw-r-- 1 carol carol 654K jul 2 11:39 LG-G8S-ThinQ-Range.jpg
-rw-rw-r-- 1 carol carol 2 15:43 Mimoji_Comparativo.jpg
94K jul
-rw-rw-r-- 1 carol carol 112K jul 10 16:20 Note10Plus.jpg
-rw-rw-r-- 1 carol carol 118K jul 11 16:36 Twitter_Down_20190711.jpg
-rw-rw-r-- 1 carol carol 324K jul
2 15:22 Xiaomi_Mimoji.png
```
También es posible crear instantáneas de solo lectura. Funcionan exactamente como
instantáneas en las que se puede escribir, con la diferencia de que el contenido de la
instantánea no se puede cambiar, se "congela" en el tiempo. Simplemente agregue el
parámetro `-r` al crear la instantánea:

    btrfs subvolume snapshot -r /mnt/disk /mnt/disk/snap

#### Algunas palabras sobre compresión
Btrfs adminte la compresión de archivos transparente, con tres algoritmos diferentes
disponibles para el usuario. Esto se hace automáticamente por el archivo, siempre que
el sistema de archivo esté montado con la opción `-o compress`. Los algoritmos son lo
suficientemente inteligentes como para detectar archivos que no se pueden comprimir y
no intentará comprimirlos, ahorrando recursos del sistema. Entonces, en un solo
directorio puede tener archivos comprimidos y descomprimidos juntos. El algoritmo decompresión predeterminado es ZLIB, pero están disponibles LZO (más rápido, pero
relación de compresión) o ZSTD (más rápido que ZLIB, compresión comparable), con
múltiples niveles de compresión.

#### Administrar particiones con GNU Parted
GNU Parted es un editor de particiones muy poderoso que se puede usar para crear,
eliminar, mover, redimensionar, rescatar y copiar particiones. Puedefuncionar con
discos GPT y MBR y cubir casi todas la nnecesidades de administración de discos.

Hay muchas interfaces gráficas que facilitan mucho el trabajo con `parted`, como
GParted para entornos de escritorio basados en GNOME y el KDE Partition Manager para
escritorios KDE. Sin embargo, debe aprender a usar `parted` en la línea de comandos,
ya que en una configuración de servidor nunca puede contar con un entorno de
escritorio gráfico disponible.

Las forma más sencilla de comenzar a usar partes es escribiendo `parted DEVICE`,
donde `DEVICE` es el dispositivo que desea administrar (`parted /dev/sdb`). El programa
inicia una interfaz de línea de comandos interactiva como `fdisk` y `gdisk` con
un mensaje para que ingrese los comandos:
```sh
parted /dev/sdb
GNU Parted 3.2
Using /dev/sdb
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted)
```
#### Seleccionar discos
Par cambiar a un disco diferente al especificado en la línea de comando, puede usar el
comando `select`, seguido del nombre del dispositivo:
```sh
(parted) select /dev/sdb
Using /dev/sdb
```

#### Obtener información
El comando `print` se puede utilizar para obtener más información sobre una partición
específica o incluso todos los dispositivos de bloque (discos) conectados a su sistema.

Para obtener información sobre la partición seleccionada actualmente, `print`.

Puede obtener una lista de todos los dispositivos de bloque conectados a su sistema
usando `print devices`.

Para obtener información sobre todos los dispositivos conectados a la vez, puede usar
`print all`. Si desea saber cuánto espacio libre hay en cada uno de ellos, puede usar
`print free`.

#### Crear una tabla de particiones en un disco vacío
Para crear una tabla de particiones en un disco vacío, use el comando `mklabel`,
seguido del tipo de tabla de articiones que desea usar.

Hay muchos tipos de tablas de particiones admitidas, pero los tipos principales que
debe concer son `msdos` que se usa aquí par referirse a una tabla de particiones MBR,
y `gpt` para referirse a una tabla de particiones GPT. Para crear una tabla de
particiones MBR, teclee:

    mklabel msdos

Y para crear una tabla de particiones GPT, el comando es:

    mklabel gpt

#### Crear una partición
Para crear una partición se usa el comando `mkpart`, usando la sintaxis
`mkparte PARTYPE FSTYPE START END`, donde:
- **`PARTYPE`**: es el tipo de partición, que puede ser primaria, lógica o extenida en caso de que se utilice una tabla de particiones MBR.
- **`FSTYPE`**: especifica qué sistema de archivos se utilizará en esta partición. Tenga en cuenta que `parted` no creará el sistema de archivos. Simplemente establece una marca en la partición que le dice al sistema de archivos qué tipo de datos esperar de ella.
- **`START`**: especifica el punto exacto en el dispositivo donde comienza la partición. Puede utilizar diferentes unidades para especificar este punto. `2s` se puede usar para referirse al segundo sector del disco, mientras que `1m` se refiere al comienzo del primer megabyte del disco. Otras unidades son `B` (bytes) y `%` (porcentaje del disco).
- **`END`**: especifica el final de la partición. Tenga en cuenta que este no es el tamaño dela particiónm es el punto del disco donde termina. Por ejemplo, si especifica `100 m`, la partición finalizará en 100 MB después del inicio del disco. Puede utilizar las misma unidades que en el parámetro `START`.

Entonces, el comando:

    mkpart primary ext4 1m 100m

Crear una partición primari de tipo `ext4`, comenzando en el primer megabyte del disco y
terminando después del megabyte 100.

#### Eliminar una partición
Para eliminar una partición, use el comando `rm` seguido del número de particón, que
puede mostrar usando el comando `print`. Entonces, `rm 2` eliminaría la segunda
partición en el disco seleccionado actualmente.

#### Recuperando particiones
`partes` puede recuperar una partición eliminada. Considere que tiene la siguiente
estructura de partición:
```sh
Number
Start End Size File system Name
1 1049kB 99.6MB 98.6MB ext4 primary
2 99.6MB 200MB 100MB ext4 primary
3 200MB 300MB 99.6MB ext4 primary
```
Por accidente, eliminó la partición 2 usando `rm 2`. Para recuperarlo, puede utilizar
el comando `rescue`, con la sintaxis `rescue START END`, donde `START` es la ubicación
aproximada donde comenzí la partición y `END` la ubicación aproximada donde terminó.

`parted` escaneará el disco en busca de particiones y ofrecerá restaurar las que
encuentre. En el ejemplo anterior, la partición 2 coemnzó en 99,6 MB y terminó en
200 MB. entonces puede usar el siguiente comenado para recuperar la partición:
```sh
(parted) rescue 90m 210m
Information: A ext4 primary partition was found at 99.6MB -> 200MB.
Do you want to add it to the partition table?
Yes/No/Cancel? y
```
Esto recuperará la partición y su contenido. Tenga en cuenta que `rescue` solo puede
recuperar particiones que tengan un sistema de archivos instalado. No se detectan
particiones vacías.

#### Cambiar el tamaño de las particiones ext2/3/4
`parted` se puede usar para cambiar el tamaño de las particiones y hacerlas más grandes
o más pequeñas. Sin embargo, hay algunas advertencias:
- Durante el cambio de tamaño, la partición debe estar sin usar y sin montar.
- Necesita suficiente espacio libre después de la partición para hacerla crecer al tamaño que desee.

El comando es `resizepart`, seguido del número de partición y dónde debería terminar.
Por ejemplo, si tiene la siguiente tabla de particiones:
```sh
Number
Start End Size File system Name
1 1049kB 99.6MB 98.6MB ext4 primary
2 99.6MB 200MB 100MB ext4 3 200MB 300MB 99.6MB ext4
```
Intentar hacer crecer la partición `1` usando `resizepart` generaría un mensaje de
error, porque con el nuevo tamaño, la partición `1` se superpondría con la partición `2`.
Sin embargo, la partición `3` se puede cambiar de tamaño ya que hay espacio libre
después de ella, lo que se puede verificar con el comando `print free`.

Entonces puede usar el siguiente comando para cambiar el tamaño de la partición
3 a 350 MB:

    resizepart 3 350m

Recuerde que el nuevo punto final se especifica contando desde el inicio del disco.
Entonces, debido a que la partición `3` terminó en 300 MB, ahora debe terminar en 350 MB.

Pero cambiar el tamaño de la partición es solo una parte de la tarea. También necesita
cambiar el tamaño del sistema de archivos que reside en ella. Para sistemas de
archivos ext2/3/4 esto se hace con el comando `resize2fs`. En el caso del ejemplo
anterior, la paritción 3 todavía muestra el tamaño "antiguo" cuando se monta:

    df -h /dev/sdb3

Para ajustar el tamaño, se puede usar el comando `resize2fs DEVICE SIZE`, donde
`DEVICE` corresponde a la partición que desea cambiar de tamaño y `SIZE` es el nuevo
tamaño. Si omite el parámetro de tamaño utilizará todo el espacio disponible de la
partición. Antes de cambiar el tamaño, se recomienda desmontar la partición.

En el ejemplo anterior:
```sh
sudo resize2fs /dev/sdb3

df -h /dev/sdb3
```
Para encoger una partición, el proceso debe realizarse en orden inverso. Primero cambie
el tamaño del sistema de archivos al nuevo tamaño más pequeño, luego cambie el tamaño
de la partición usando `parted`.

En nuestro ejemplo:
```sh
resize2fs /dev/sdb3 88m

parted /dev/sdb3
```

#### Creación de particiones de intercambio
En Linux, el sistema puede intercambiar páginas de memoria de RAM a disco según sea
necesario, almacenándolas en un espacio separado generlamente implementado como una
partición separada en un disco, llamada partición de intercambio o simplemente
intercambio. Esta partición debe ser de un tipo especifico y configurarse con una
utilidad adecuada (`mkswap`) antes de que pueda usarse.

Para crear la partición de intercambio usando `fdisk` o `gdisk`, simplemente como si
estuviera creando una partición normal. la única diferencia es que necesitará cambiar
el tipo de paritción a Linux swap.
- En `fdisk` use el comando `t`. Seleccione la partición que deasea utilizar y cambie su tipo a `82`.
- En `gdisk` el comando para cambiar el ripo de patición tambien es `t`, pero el código es `8200`. Escriba los cambios en el disco y salga con `w`.

Si está usando `parted`, la partición debe identificarse como una partición de
intercambio durante la creación, simplemente use `linux-swap` como tipo de sistema de
archivos. Por ejemplo, el comando para crear una partición de intercambio de 500 a
partir de 300 MB en el disco es:

    mkpart primary linux-swap 301m 800m

Una vez que la partición está creada e identificada, simplemente user `mkswap` seguido
del dispositivo que representa la partición que desea usar, como:

    mkswap /dev/sda2

Para habilitar el intercambio en esta partición, user `swapon` seguido del nombre del
dispositivo:

    swapon /dev/sda2

Del mismo modo, `swapoff`, seguido del nombre del dispositivo, desactivará el
intercambio en ese dipositivo.

Linux también admite el uso de swap files en lugar de particiones. Simplemente cree un
archivo vacío del tamaño que desee usando `dd` y luego user `mkswap` y `swapon` con este
archivo como destino.

Los siguientes comandos crearán un archivo de 1 GB llamado `myswap` en el directorio
actual, lleno de ceros, y luego lo configurarán y habilitarán como un archivo de
intercambio.

Cree el archivo de intercambio:

    sudo dd if=/dev/zero of=myswap bs=1M count=1024

`if=` es el archivo de entrada, el origen de los datos que se escribirán en el archivo.
En este caso es el dispositivo `/dev/zero`, que proporciona tantos caracteres `NULL`
como se soliciten. `of=` es el archivo de salida, el archivo que se creará. `bs=`
es el tamaño de los bloques de datos, aquí especificado en Megabytes, y `count=`
es la cantidad de bloques que se escribirán en la salida, 1024 bloques de 1 MB cada
uno equivalente a 1 GB.
```sh
mkswap myswap

swapon myswap
```
Usando los comandos anteriores, este archivo de intercambio se usará solo durante la
sesión actual del sistema. Si se reinicia la máquina, el archivo seguirá estando
disponible, pero no se cargará automáticamente. Puede automzatizar eso agregando
el nuevo archivo de intercambio a `/etc/fstab`.

## Mantener la integridad de los sistemas de archivos
Los sistemas de archivos modernos de Linux utilizan journalig. Esto significa que cada
operación se refleja en un registro interno (el journal) antes de ejecutarse. Si
la operación se interrumpe debido a un error del sistema, se puede recontruir
revisando el diario, evitando la corrumpción del sistema de archivo y la pérdida de
datos.

Esto reduce en gran medida la necesida de verificaciones manuales del sistema de
archivos, pero es posible que aún sean necesarias. Conocer las herramientas utilizadas
para esto puede representar la diferencia entre cenar en casa o con tu familia o
pasar la noche en la sala de servidores del trabajo.

#### Comprobación del uso del disco
Hay dos comando que se pueden usar para verificar cuáto espacio se está usando y cuánto
queda en un sistema de archivos. El primero es `du`, que significa "uso de disco".

`du` es de natulareza recursiva. En su forma más básica, el comando simplemente
monstará cuántos bloques de 1 kilobyte están siendo utilizados por el directorio actual
y todos sus subdirectorios:

    du

Esto no es muy útil, por lo que podemos solicitar una salida legible por humanos
agregando el parámetro `-h`:

    du -h

Por defecto, `du` solo muestra el recuent de uso de los directorios. Para mostrar un
recuento individual de todos los archivos en el directorio, user el parámetro `-a`:

    du -ah

El comportamiento predeterminado es mostrar el uso de cada subdirectorio, luego el
uso total del directorio actual, incluyendo subdirectorios.

En el ejemplo anterior, podemos ver que el subdirectorio `Temp` ocupa 4.8 MB y el
directorio actual incluyendo `Temp`, ocupan 6.0 MB. Pero ¿cuánto espacio ocupan los
archivos en el directorio actual, excluyendo los subdirectorios? Para eso tenemos el
parámetro `-S`:
```sh
du -Sh
4.8M ./Temp
1.3M .
```
Si desea mantener esta distinción entre el espacio usado por los archivos en el
directorio actual y el espacio usado por los subdirectorios, pero también quiere un
gran total al final, puede agregar el parámetro `-c`:
```sh
du -Shc
4.8M ./Temp
1.3M .
6.0M total
```
Puede controlar qué tan profundo debe ir la salida de `du` con el parámetro `-d N`,
donde `N` describe los niveles. Por ejemplo, si usa el parámetro `-d 1`, mostrará el
directorio actual y sus subdirectorios, pero no los subdirectorios de esos.

Vea la diferencia a continuación. Sin `-d`:
```sh
du -h
216K ./somedir/anotherdir
224K ./somedir
232K .
```
Y limitando la profundidad a un nivel con `-d 1`:
```sh
du -h -d1
224K ./somedir
232K .
```
Tenga en cuenta que incluso si no se muestra `anotherdir`, su tamaño se siguie teniendo
en cuenta.

Es posible que desee excluir algunos tipos de archivos del recuento, lo que puede hacer
con `--exclude="PATTERN"`, donde `PATTERN` es el patrón con el que desea comparar.
Considere este directorio:
```sh
du -ah
124K ./ASM68K.EXE
2.0M ./Contra.bin
36K ./fixheadr.exe
4.0K ./README.txt
2.1M ./Contra_NEW.bin
4.0K ./Built.bat
8.0K ./Contra_Main.asm
4.2M .
```
Ahora, usaremos `--exclude` para filtar todos los archivos con la extension `.bin`:
```sh
du -ah --exclude="*.bin"
124K
./ASM68K.EXE
36K ./fixheadr.exe
4.0K ./README.txt
4.0K ./Built.bat
8.0K ./Contra_Main.asm
180K .
```
Tenga en cuenta que el total ya no refleja el tamaño de los archivos excluidos.

#### Comprobación de espacio libre
`du` funciona a nivel de archivos. Hay otro comando que puede monstrarle el uso del
disco y la cantidad de espacio disponible a nivel del sistema de archivos. Este
comando es `df`.

El comando `df` proporcionará una lista de todos los sistemas de archivos disponibles
(ya montados) en un sistema, incluido su tamaño total, cuánto espacio se ha utilizado,
cuánto espacio está disponible, el procentaje de uso y dónde está montado:

    df

Sin embargo, mostrarar el tamaño en bloques de 1 KB no es muy fácil de usar. Como
en `du`, puede agregar los árámetros `-h` para obtener un resultado más legible por
humanos:

    df -h

También puede usar el parámetro `-i` para mostrar indos usados/disponible en lugar de
bloques:

    df -i

Un parámetro útil es `-T`, que también imprimirá el tipo de cada sistema de archivos:

    df -hT

Conociendo el tipo de sistemas de archivos, puede filtrar la salida. Puede monstrar
solo sistemas de archivos de un tipo dado con `-t TYPE`, o excluir sistemas de archivos
de un tipo dado con `-x TYPE`, como en los ejemplos siguientes.

Excluyendo los sistemas de archivos `tmpfs`:

    df -hx tmpfs

Mostrando solo los sistemas de archivos `ext4`:

    df -ht ext4

También puede personalizar la salida de `df`, seleccionando lo que debe mostrarse y en
qué orden, usando el parámetro `--output=` seguido de una lista de campos separados
por comas que desee monstrar. Algunos de los campos disponibles son:
- **`source`**: el dispositivo correspondiente al sistema de archivos.
- **`fstype`**: el tipo de sistema de archivos.
- **`size`**: el tamaño total del sistema de archivos.
- **`used`**: cuánto espacio se está utilizando.
- **`avail`**: cuánto espacio hay disponible.
- **`pcent`**: el porcentaje de uso.
- **`target`**: dónde está montado el sistema de archivos (punto de montaje).

Si desea una salida que muestre el destino, fuente, el tipo y el uso, puede usar:

    df -h --output=target,source,fstype

`df` también se puede usar para verificar la información de inodos, pasando los
siguientes campos a `--output=`:
- **`itotal`**: el número total de inodos en el sistema de archivos.
- **`iused`**: el número de inodos usados en el sistema de archivos.
- **`iavail`**: el número de inodos disponibles en el sistema de archivos.
- **`ipcent`**: el porcentaje de inodos usados en el sistema de archivos.

Por ejemplo:

    df --output=source,fstype,itotal,iused,ipcent

#### Mantenimiento de los sistemas de archivos ext2/3/4
Para comprobar un sistema de archivos en busca de errores, Linux proporciona la
utilidad `fsck` (filesystem check). En su forma más básica, puede invovarlo con `fsck`
seguido de la ubicación del sistema de archivos que desee verificar:

    fsch /dev/sdb1

> NUNCA ejecute `fssch` en un sistema de archivos montado. Si lo hace de todos modos, es posible que pierda los datos.

`fsck` por sí mismo no verificará el sistemas de archivos, simplemente llamará a la
utilidad aporpiada para el tipo de sistema de archivos para hacerlo. En el ejemplo
anterior, dado que no se especificó un tipo de sistema de archivos, `fsck` asumió
un sistema de archivos ext2/3/4 por defecto y se llamó `e2fsck`.

Para especificar un sistema de archivos, user la opción `-t`, seguida del nombre del
sistema de archivos, como en `fsck -t vfat /dev/sdc`. Alternativamente, puede llamar
directamente a una utilidad específica del sistema de archivos, como `fsck.msdos`
para sistema de archivos FAT.

`fsck` puede tomar argumentos de línea de comandos. Estos son algunos de los más
comunes:
- **`-A`**: esto comprobará todos los sistemas de archivos listado en `/etc/fstab`.
- **`-C`**: muestra una barra de porgreso al comprobar un sistema de archivos. Actualmente solo funciona en sistemas de archivos ext2/3/4.
- **`-N`**: esto imprimirá lo que ser haría y sadrá, sin realmente verificar el sistema de archivos.
- **`-R`**: cuando se usa junto con `-A`, esto omitirá la verificación del sistema de archivos raíz.
- **`-V`**: modo detalaldo, imprime más información de lo habitual durante el funcionamiento. Esto es útil para depurar.

La utilidad específica para los sistemas de archivos ext2/3/4 es `e2fsck`, también
llamado `fsck.ext2`, `fsck.ext3` y `fsck.ext4`. De forma predeterminada, se ejecuta en
modo interactivo: cuando se encuentra un error en el sistema de archivos, se detiene y
le preguna al usuario qué hacer. El usuario debe escribir `y` para solucionar el
problema, `n` para dejarlo sin arreglar o `a` para solucionar el problema actual y
todos los posteriores.

Por supueesto, setrase frente a una terminal esperando a que `e2fsck` pregunte qué
hacer no es un uso productivo de su tiempo, especialmente si se trata de un sistema
de archivos grande. Entonces, hay opciones que hacen que `e2fsck` se ejecuten en modo
no interactivo:
- **`-p`**: esto intentara coregir automáticamente cualquier error encontrado. Si se encuentra uun error que requiere intervención del administrador del sistema, `e2fsck` proporcionará una descripción del problema y saldrá
- **`-y`**: esto responderá `y` (sí) a todas las preguntas.
- **`-n`**: lo contrario de `-y`. Además de responder `n` a todas las preguntas, esto hará que el sistema de archivo se monte como de solo lectura, por lo que no se puede modificar.
- **`-f`**: obliga a `e2fsck` a comprobar un sistema de archivos incluso si está marcando como "clean", es decir, se ha desmontado correctamente.

#### Ajustes de un sistema de archivos ext
Los sistemas de archivos ext2/3/4 tienen una serie de parámetros que el administrador
del sistema puede ajustar o afinar para adaptarse mejor a las necesidades del sistema.
La utilidad utilizada para mostrar o modificar estos parámetros se llamada `tune2fs`.

Para ver los parámetros actuales para cualquier sistema de archivos dado, user el
parámetro `-l` seguido del dispositivo que representa la partición. El siguiente
ejemplo muestra el resultado de este comando en la primera partición del primer disco
(`/dev/sda1`) de una máquina:

    tune2fs -l /dev/sda1

Los sistemas de archivso ext tienen conteos de montaje. El recuento aumenta en 1 cada
vez que se monta el sistema de archivos, y cuando se alcanza un valor de umbral (el
recuento máximo de montaje), el sistema se comprobará automáticamente con `e2fsck`
en el próximo arranque.

El número máximo de montajes se puede establecer con el parámetro `-c N`, donde `-N`
es el número de veces que se puede montar el sistema de archivos sin comprobarlo.
El parámetro `-C N` establece el número de veces que se ha montado el sistema en el
valor de `N`. Tenga en cuenta que los parámetros de la línea de comandos dinstingue
entre mayúsculas y minúsculas, por lo que `-c` es diferente de `-C`.

También es posible un intervalo de timpo entre comprobaciones, con el parámetro `-i`,
seguido de un número y las letras `d` para días, `m` para meses e `y` para años. Por
ejemplo, `-i 10d` comprobaría el sistema de archivo en el próximo reinicio cada 10 días.
Utilice cero como valo para
desactivar esta función.

`-L` se puede usar para establecer una etiqueta para el sistema de archivos. Esta
etiqueta puede tener hasta 16 caracteres. El parámetro `-U` establece el UUID para el
sistema de archivos, que es un número hexadecimal de 128 bits. En el ejemplo anterior,
el UUID es `6e2c12e3-472d-4bac-a257-c49ac07f3761`. Tanto la etiqueta como el UUID pueden
usarse en lugar del nombre del dispositivo (como `/dev/sda1`) para mostrar el sistema
de archivos.

La opción `-e BEHAVIOUR` define el comportamiento del kernel cuando se encuentra un
error en el sistema de arhivos. Hay tres posibles comportamientos:
- **`continue`**: continuará la ejecución normalmente.
- **`remount-re`**: volverá a montar el sistema de archivos como de solo lectura.
- **`panic`**: causará kernel panic.

El comportamiento predeterminado es `continue`. `remount-ro` podría ser útil en
aplicaciones sensibles a los datos, ya que detendrá inmediatamente las escrituras en
el disco, evitando más errores potenciales.

Los sistemas de archivos ext3 son básicamente sistemas de archivos ext2 con un journal.
Usando `tune2fs` puede agreagar un journal en sus sistema de archivos ext2,
convirtiéndolo así en ext3. El procedimiento es simple, simplemente pase el parámetro
`-j` a `tune2fs`, seguido del dispositivo que contienen el sistema de archivos:

    tune2fs -j /dev/sda1

Luego, cuando monte el sistema de archivos convertido, no olvide establecer el tipo
en `ext3` para que se pueda usar el journal.

Cuando se trata de sistemas de archivos registrados por journal, el parámetro `-J` le
permite usar parámetros adicionales para establecer algunas opciones de journal,
como `-J size=` para establecer el tamaño del journal (en megabytes), `-J location=`
para especificar dónde el journal debe almacenarse e incluso poner el journal en un
dispositivo externo con `-j device=`.

Puede especificar varios parámetros a la vez separándolos con una coma. Por ejemplo:
`-J size=10,location=100M,device=/dev/sdb1` creará un journal de 10 MB en la posición
de 100 MB en el dispositivo `/dev/sdb1`.

#### Mantenimiento de sistemas de archivos XFS
Para los sistemas de archivos XFS, el equivalente de `fsck` es `xfs_repair`. Si
sospecha que algo anda mal con el sistema de archivos, lo primero que debe hacer es
escanearlo en busca de daños.

Esto se puede hacer pasando el parámetros `-n` a `xfs_repair`, seguido del dispositivo
que contiene el sistema de archivos. El parámetros `-n` significa "no modificar": se
comprobará el sistema de archivos, se informarán los errores pero no se realizarán
reparaciones:

    xfs_repair -n /dev/sdb1

Si se encuentran errores, puede proceder a hacer las preparaciones sin el parámetro
`-n`, así: `xfs_repair /dev/sdb1`.

`xfs_repair` acepta varias opciones de la línea de comandos. Entre ellos:
- **`-l LOGDEV y -r RTDEV`**: estos son necesarios si el sistema de archivos tiene un riesgo externo y secciones en tiempo real. En este caso, reemplace `LOGDEV` y `RTDEV` con los dispositivos correspondientes.
- **`-m N`**: se utiliza para limitar el uso de memoria de `xfs_repair` a `N`, algo que puede ser útil en la configuración del servidor. Según la página del manual, de forma predeterminada, `xfs_repair` escalará su uso de memoria según sea necesario, hasta el 75% de la RAM física del sistema.
- **`-d`**: el modo "dangerous" permitirá la reparación del sistema de archivos que estén montados como de solo lectura.
- **`-v`**: modo detalladoo. Cada vez que se usa este parámetro, la verbosidad aumenta.

Tenga en cuenta que `xfs_repair` no puede reparar sistemas de archivos con un registro
"sucio". Puede "poner a cero" un registro corrupto con el parámetro `-L`, pero tenga
en cuenta que este es un último recurso ya que puede provocar la corrupción delsiste    ma de archivos y la pérdida de datos.

Para depurar un sistema de archivos XFS, se puede usar la utilidad `xfs_db`, como
en `xfs_db /dev/sdb1`. Esto se usa principalmente para inspeccionar varios elementos y
parámetros del sistema de archivos.

Esta utilidad tiene un indicador interactivo, como `parted`, con muchos comandos
internos. También hay disponible un sistema de ayuda: teclee `help` para ver una lista
de todos los comandos, y `help` seguido del nombre del comando para ver más información
sobre el comando.

Otra utilidad interesante es `xfs_fsr`, que se puede utilizar para reorganizar un
sistema de archivos XFS. Cuando se ejecuta sin ningún argumento adicional, se ejecutará
durante dos horas e intentará desfragmentar todos los sistemas de archivos XFS
monstados y escribibles enumerados en el archivo `/etc/mtab`. Es posible que deba
instalar esta utilidad utilizando el administrador de paquetes para su distribución
de Linux, ya que es probale que no sea parte de una instalación predeterminada.

## Controlar el montaje y desmontaje de los sistemas de archivos
#### Montaje y desmontaje de sistemas de archivos
El comando para montar manualmente un sistema de archivos se llama `mount` y su
sintaxis es:

    mount -t TYPE DEVICE MOUNTPOINT

Donde:
- **`TYPE`**: el tipo de sistema de archivos que se está montando.
- **`DEVICE`**: el nombre de la partición que contiene el sistema de archivos (`/dev/sda1`).
- **`MOUNTPOINT`**: dónde se montará el sistema de archivos. No es necesario que el directorio en el que esté montado esté vacío, aunque debe existir. Sin embargo, cualquier archivo que contenga será inaccesible por su nombre mientras el sistema de archivos esté montado.

Por ejemplo, para montar una unidad flash USB que contenga un sistema de archivos exFAT
ubicado en `/dev/sdb1` en un directorio llamado `flash` en su directorio de inicio,
puede usar:

    mount -t exfat /dev/sdb1 ~/flash

Después del montaje, se podrá acceder al contenido del sistema de archivos en el
directorio `~/flash`:

    ls -lh ~/flash

#### Listado del sistema de archivos montados
Si teclea simplemente `mount`, obtendrá una lista de todos los sistemas de archivos
actualmente montados en su sistema. Esta lista puede ser bastante grande porque,
además de los discos conectados a su sistemas, también contiene varios sistemas de
archivos en tiempo de ejecución en la memoria que sirven para propósitos. Para filtrar
la salida, puede usar el parámetro `-t` para listar solo los sistemas de archivos del
tipo correspondiente, como se muestra a continuación:
```sh
mount -t ext4
/dev/sda1 on / type ext4 (rw,relatime,errors=remount-ro)
```
Puede especificar varios sistemas de archivos a la vez separándolos con una coma:
```sh
mount -t ext4,fuseblk
/dev/sda1 on / type ext4 (rw,noatime,errors=remount-ro)
/dev/sdb1 on /home/carol/flash type fuseblk
(rw,nosuid,nodev,relatime,user_id=0,group_id=0,default_permissions,allow_other,blksize=4096) [DT_8GB]
```
La salida de los ejemplo anteriores se puede describir en el formato:

    SOURCE on TARGET type TYPE OPTIONS

Donde `SOURCE` es la partición que contiene el sistema de archivos, `TARGET` es el
directorio donde está montado, `TYPE` es el tipo del sistemas de archivos y `OPTIONS`
son las opciones pasadas al comando `mount` en el momento del montaje.

#### Parámetros adicionales de la línea de comandos
Hay muchos parámetros de la línea de comandos que se pueden usar con `mount.` Algunas
de las más utilzadas son:
- **`-a`**: esto mostrará todos los sistemas de archivos listados en el archivo `/etc/fstab`.
- **`-o o --options`**: esto pasará una lista de opciones de montaje separadas por comas al comando de montaje, que puede cambiar cómo se montará el sistema de archivos.
- **`-r o -ro`**: esto montará el sistema de archivos como de solo lectura.
- **`-w o -rw`**: esto hará que el sistema de archivos de montaje sea escribible.

Para desmontar un sistemas de archivos, use el comando `umount`, seguido del nombre del
dispositivo o el punto de montaje. Teniendo en cuenta el ejemplo anterior, los
comandos siguientes son intercambiables:
```sh
umount /dev/sdb1
umount ~/flash
```
Algunos de los parámetros de la línea de comandos para `umount` son:
- **`-a`**: esto desmontara todos los sistemas de archivos listados en `/etc/fstab`.
- **`-f`**: esto forzará el demonstaje de un sistema de archivos. Esto puede resultar úitl si ha montado un sistema de archivos remoto que se ha vuelto inalcanzable.
- **`-r`**: si el sistema de archivos no se puede desmontar, esto intentará convertirlo en solo lectura.

#### Tratamiento de archivos abiertos
Al desmontar un sistema de archivos, puede econtrar un mensaje de error que indica que el `target is busy`. Esto sucederá si hay archivos abiertos en el sistema de archivos.
Sin embargo, puede que no sea obvio de inmediato dónde se encuentra un archivo 
abierto o qué está accediendo al sistema de archivos.

En tales casos, puede usar el comando `lsof`, seguido del nombre del dispositivo que
contiene el sistema de archivos, para ver una lista de los procesos que acceden a él
y qué archivos están abiertos. Por ejemplo:
```sh
umount /dev/sdb1
umount: /media/carol/External_Drive: target is busy.

lsof /dev/sdb1
COMMAND PID USER FD TYPE DEVICE SIZE/OFF NODE NAME
evince 3135 carol 16r REG 8,17 21881768 5159
media/carol/External_Drive/Docuements/E-Books/MagPi40.pdf
```
`COMMAND` es el nombre del ejecutable que abrió el archivo y `PID` es el número de
proceso. `NAME` es el nombre del archivo que está abierto. En el ejemplo anterior, el
archivo `MagPi40.pdf` es abierto por el programa `evince`. Si cerramos el programa,
podremos demontrar el sistema de archivos.

#### ¿Dónde montar?
Puede montar un sistema de archivo en cualaquier lufar que desee. Sin embargo, hay
algunas buenas prácticas que debe seguir para facilitar la administración del sistema.

Tradicionalmente, `/mnt` era el directorio en el que se montarían todos los dispositivos
externo y una serie de puntos de anclaje preconfigurado para dispositivos comunes,
como unidades de CR-ROM (`/mnt/cdrom`) y disquetes (`/mnt/floppy`) existían en él.

Esto ha sido reemplazado por `/media`, que ahora es el punto de montaje predeterminado
para cualquier medio extraible por el usuario conectados al sistema.

En la mayoría de las distribuciones modernas de Linux y entornos de escritorio, los
dispositivos extraíbles se  montar automáticamente en `/media/USER/LABEL` cuando
se conecta al sistema, donde `USER` es el nombre de usuario y `LABEL` es la etiqueta del
dispositivo. Por ejemmplo, una unidad flash USB con la etiqueta `FlashDrive`
conectada por el usuario `john` se montaría en `/media/john/FlashDrive`.

Dicho esto, siempre que necesite montar manualmente un sistema de archivos, es buena
práctica montarlo en `/mnt`.

#### Montaje de sistemas de archivo en el arranque
El archivo `/etc/fstab` contiene descripciones sobre los sistemas de archivos que se
pueden montar. Este s un archivo de texto, donde cada línea describe un sistema de
archivos que se va a montar, con seis campos por línea en el siguiente orden:

    FILESYSTEM MOUNTPOINT TYPE OPTIONS DUMP PASS

Donde:
- **`FILESYSTEM`**: el dispositivo que contiene el sistema de archivos que se va a montar. En lugar del dispositivo, puede especificar el UUID o etiqueta de la partición.
- **`MOUNTPOINT`**: dónde se montará el sistema de archivos.
- **`TYPE`**: el tipo de sistema de archivos.
- **`OPTIONS`**: opciones de montaje que se pasarán a `mount`.
- **`DUMP`**: indica si cualquier sistema de archivos ext2/3/4 debe considerarse para la copia de seguridad mediante el comando `dump`. Por lo general, es cero, lo que significa que debe ignorarse.
- **`PASS`**: cuando es distinto de cero, define el orden en el que se comprobarán los sistemas de archivos durante el arranque. Normalmente es cero.

Por ejemplo, la primera partición en el primer disco de una máquina podría describerse
como:

    /dev/sda1 / ext4 noatime/errors

Las opciones de montaje en `OPTIONS` son una lista de parámetros separados por comas,
que pueden ser genéricos o específicos del sistema de archivos. Entre los genéricos
tenemos:
- **`atime y noatime`**: por defecto, cada vez que se lee un archivo, se actualiza la información de tiempo de acceso. Deshabilitar esto (con `noatime`) puede acelerar la E/S del disco. No confundan esto con la hora de modificación, que se actualiza cada vez que se escribe un archivo.
- **`auto y noauto`**: si el sistema de archivos puede (o no) montarse automáticamente con `mount -a`.
- **`defaults`**: esto pasará las opciones `rw`, `suid`, `dev`, `exec`, `auto`, `nouser` y `async` a `mount`.
- **`dev y nodev`**: si deben interpretarse los dipositivos de caracteres o bloques en el sistema de archios montado.
- **`exec y noexec`**: permitir o denegar el permiso para ejecutar binarios en el sistema de archivos.
- **`user y nouser`**: permite (o no) a un usuario normal montar el sistema de archivos.
- **`group`**: permite a un usuario montar el sistema de archivos si el usuario pertenece al mismo grupo que posee el dispositivo que lo contiene.
- **`owner`**: permite a un usuario montar un sistema de archivos si el usuario posee el dipositivo que lo contiene.
- **`suid y nosuid`**: permite, o no, que los bits SETUID y SETGID surtan efecto.
- **`ro y rw`**: monsta un sistema de archivos como de solo lectura o de escritura.
- **`remount`**: esto intentará volver a montar un sistema de archivos ya montado. Esto no se usa en `/etc/fstab`, sino como un parámetro para `mount -o`. Por ejemplo, para volver a montar la partición `/dev/sdb1` ya contada como de solo lectura, puede usar el comando `mount -o remount,ro /dev/sdb1`. Al vovler a montar, no es necesario especificar el tipo de sistema de archivos, solo el nombre del dispositivo o el punto de mnotaje.
- **`sync y async`**: realizar todas las operaciones E/S en el sistema de archivos de forma sincrónica o asicrónica. `async` suele ser predeterminado. La página del manual de `mount` advierte que el uso de `sync` en medios con un número limitado de ciclos de escritura (como unidades flash o tarjetas de memoria) puede acortar la vida útil del dipositivo.

#### Uso de UUDI y etiquetas
Especificar el nombre del dispositivo que contiene el sistema de archivos a montar
puede presentar algunos problemas. A veces, el mismo nombre del dispositivo puede
asignarse a otro dispositivo dependiendo de cuándo o dónde se conectó a su sistema.
Por ejemplo, una unidade flash USB en `/dev/sdb1` puede asignarse a `/dev/sdc1` si se
conecta a otro puerto, o después de otra unidad flash.

Una forma de evitrar esto es especificar la etiqueta o UUID del volumen. Ambos se
especifican cuando se crea el sistema de archivos y no cambiarán, a menos que el
sistema de archivos se destruya o se asigne manualmente una nueva etiquetaa o UUID.

El comando `lsblk` se puede utlizar para consultar información sobre un sistema de
archivos y averiguar la etiqueta y el UUID asociados a él. Para hacer esto, user el
parámetro `-f`, seguido del nombre del dispositivo:

    lsblk -f /dev/sda1

Aquí está el significado de cada columna:
- **`NAME`**: nombre del dispositivo que contiene el sistema de archivos.
- **`FSTYPE`**: tipo de sistema de archivos.
- **`LABEL`**: etiqueta del sistema de archivos.
- **`UUID`**: identificador único universal (UUID) asignao al sistema de archivos.
- **`FSAVAIL`**: cuánto espacio hay disponible en el sistema de archivos.
- **`FSUSE%`**: porcentaje de uso del sistema de archivos.
- **`MOUNTPOINT`**: dónde está montado el sistema de archivos.

En `/etc/fstab` un dispositivo se puede especificar por su UUID con la opción `UUID=`,
seguido del UUID, o con `LABEL=`, seguido de la etiqueta. Entonces, en lugar de:

    /dev/sda1 / ext4 noatime/errors

Usarías:

    UUID=6e2c12e3-472d-4bac-a257-c49ac07f3761 / ext4 noatime,errors

O, si tienen un disco con la etiqueta `homedisk`:

    LABEL=homedisk /home ext4 defaults

La misma sintaxis se puede utilizar con el comando `mount`. En lugar del nombre del
dispositivo, pase el UUID o la etiqueta. Por ejemplo, para montar un disco NTFS externo
con el UUID `56C11DCC5D2E1334` en `/mnt/external`, el comando sería:

    mount -t ntfs UUID=56C11DCC5D2E1334 /mnt/external

#### Montaje de discos con Systemd
Systemd es el init del sistema, el primer proces que se ejecuta en muchas
distribuciones de Linux. Es responsable de generar otros procesos, iniciar servicios
y arrancar el sistema. Entre muchas otras tareas, systemd también se puede utilizar para
gestionar el montaje (y montaje automático) de sistemas de archivos.

Para utilizar esta función de systemd, debe crear un archivo de configuración
llamado *mount init*. Cada volumen que se va a montar tiene su propia unidad de montaje
y es necesario colocarlo en `/etc/systemd/system`.

Las unidades de montaje son archivo de texto simples con la extensión `.mount`. El
formato básico se muestra a continuación:
```sh
[Unit]
Description=

[Mount]
What=
Where=
Type=
Options=

[Install]
WantedBy=
```
- **`Description=`**: breve descripción de la unida de montaje, algo así como `Monta el disco de respaldo`.
- **`What=`**: qué se debe montar. El volumen debe especificar como `/dev/disk/by-uudi/VOL_UUID` donde `VOL_UUID` es el UUID del volumen.
- **`Where=`**: debe ser la ruta completa hacia donde se debe montar el volumen.
- **`Type=`**: el tipo de sistema de archivos.
- **`Options=`**: las opciones de montaje que desee pasar, son las mismas que se utilizan con el comando `mount` o en `/etc/fstab`.
- **`WantedBy=`**: se utiliza para la gestión de dependencias. En este casoi, usaremos `multi-user.target`, lo que significa que soempre que el sistema se inicie en un entorno multiusuario (un inicio normal), se montará la unidad.

Nuesto ejemplo anterior del disco externo podría escribirse como:
```sh
[Unit]
Description=External data disk

[Mount]
What=/dev/disk/by-uuid/56C11DCC5D2E1334
Where=/mnt/external
Type=ntfs
Options=defaults

[Install]
WantedBy=multi-user.target
```
Pero aún no hemos terminado. Para que funcione correctamente, la unida de montaje debe
tener el mismo nombre que el punto de montaje. En este caso, el punto de montaje es
`/mnt/external`, por lo que el archivo debe llamarse `mnt-external.mount`.

Después de eso, debe reiniciar el demonio systemd con el comando `systemctl` e
iniciar la unidad:
```sh
systemctl daemon-reload
systemcl start mnt-exterlan.mount
```
Ahora el contenido del disco externo debería estar disponible en `/mnt/external`. Puede
verificar el estado del montaje con el comando `systemctl status mnt-external.mount`:

    sudo systemctl status mnt-external.mount

El comando `systemctl start mnt-external.mount` solo habilitará la unidad para la
sesión actual. Si desea habilitarlo en cada arranque, reemplace `start` por `enable`:

    sudo systemctl enable mnt-external.mount

#### Montaje automático de la unidad de montaje
La unidad de montaje se puede montar automáticamente siempre que se acceda al punto
de montaje. Para hacer esto, necesita un archivo `.automount`, junto con el archivo
`.mount` que describe la unidad. El formato básico es:
```sh
[Unit]
Description=

[Automount]
Where=

[Install]
WantedBy=multi-user.target
```
Como antes, `Description=` es una breve descripción del archivo y `WHere=` es el punto
de montaje. Por ejemplo, un archivo `.automount` para nuestro ejemplo anterior sería:
```sh
[Unit]
Description=Automount for the external data disk

[Automount]
Where=/mnt/external

[Install]
WantedBy=multi-user.target
```
Guarde el archivo con el mismo nombre que el punto de montaje
(`mnt-external.automount`), vuelva a cargar systemd e inicie la unidad:

    sudo systemctl daemon-reaload
    sudo system start mnt-external.automount

Ahora, siempre que se acceda al directorio `/mnt/external`, se montara el disco. Como
antes, para habilitar el montaje automático en cada arranque, usaría:

    sudo systemctl enable mnt-external.automount
