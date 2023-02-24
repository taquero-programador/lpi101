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
En Linux, los dispotivos de almacenamiento se denominan genéricamente dispositivos de
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

pg29
