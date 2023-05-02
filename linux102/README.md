# Linux 102

## Personalizar y usar el entorno de shell
El shell es posiblemente la herramienta más poderosa en un sistema operativo Linux y puede
definirse como una interfaz entre el usuario y el kernel. Tiene la función de interpretar los
comandos introducidos por el usuario, por lo tanto, los administradore de sistemas debe ser
hábiles en su uso. Como probablemente sabemos, el Bourne Again Shell (*Bash*) es el shell de
facto en la gran mayoría de las distribuciones de Linux.

El momento que el sistema operativo inicia, lo primero que el Bash (o cualquier otro shell)
realiza, es ejecutar una serie de scripts de inicio. Estos scripts personalizan el entorno de
sesión. Existen varios scripts para todo el sistema operativo, así también para usuarios
específicos. En estos scripts podemos elegir las preferencias o configuraciones que mejor se
adapten a las necesidades de nuestros usuarios en forma de variables, alias y funciones.

La serie exacta de archivos de inicio depende de un parámetro muy importante: el tipo de shell.

#### Tipos de shell: interactivo vs. no interactivo y inicio de sesión vs. sin inicio de sesión

**Shell interactivos / no interactivo**: este tipo de shells se refiere a la intercomunicación
entre el usuario y el shell: mediante el teclado el usuario proporciona la entrada digitando
comandos en la terminal y a su vez el shell proporciona la salida imprimiendo mensajes en
la pantalla.

**Shells de inicio de sesión / sin inicio de sesión**: este tipo de shell se refiere al evento
de un suario cuando accede a un sistema informático por medio de sus credenciales, como el
nombre de usuario y la contraseña.

Tanto los shells interactivos como los no interactivos pueden ser de inicio de sesión o sin
inicio de sesión y cualquier combinación posible de estos tipos tiene sus usos específicos.

*Shells Interactivo de inicio de sesión* se ejecutan cuando los usuarios se conectan al sistema
y se utilizan para personalizar las configuraciones de los usuarios según sus necesidades. Un
buen ejemplo de este tipo de shell, sería el de un grupo de usuarios que pertenecen al mismp
departamento y necesitan un conjunto de variable determinadas en sus sesiones.

Para *shells Interactivos sin inicio de sesión* nos referimos a cualquier otro shell abierto por
el usuario después de entrar en el sistema. Los usuarios utilizan estos shell durante las sesiones
para llevar a cabo tareas administrativas y de mantenimiento, como la configuración de variables,
el tiempo, la copia de archivos, crear scripts, etc.

Por otro lado, los shells no interactivo no requieren ninǵun tipo de interacción humana. Por lo
tanto, estos shells no solicitan al usuario una entrada y su salida (si la hubiera) será
escrita en un registro (en la mayoría de los casos).

Los *shells de inicio de sesión no interactivos* son bastante raros y poco prácticos. Sus usos son
virtualmente inexistentes. Algunos ejemplos extraños incluyen forzar un script a ser ejecutado
después de un shell de inicio de sesión con `/bin/bash --login script` o canalizar la salida
estándar (stdout) de un comando a la entrada estándar (stdin) de una conexión ssh:

    <some_commnad> | ssh user@ip

en cuanto el shell *interactivo sin inicio de sesión* no hay interacción ni login en nombre del
usuario, por lo que aquí nos referimos al uso de scripts automatizados. Estos scrits se utilizan
principalmente para llevar a cabo tareas administrativas y de mantenimiento respectivamente
como las incluidas en los cronjobs. En algunos casos, `bash` no lee ningún archivo de inicio.

#### Ejecutando shells con Bash
Después de iniciar sesión, escribe `bash` en una terminal para abrir un nuevo shell.
Técnicamente, este shell es un proceso hijo del shell actual.

Al iniciar le proceso hijo de `bash`, podemos especificar varias opciones para definir qué tipo
de shell queremos iniciar. Aquí hay algunas importantes a la hora de invocarlo:

- **`bash -l o bash --login`**: invocará un shell de inicio de sesión
- **`bash -i`**: invocará un shel interactivo
- **`bash --noprofile`**: con shell de inicio de sesión ignorará tanto el archivo de inicio de todo el sisteama `/etc/profile` como los archivos de inicio a nivel de usuario `~/.bash_profile`, `~/.bash_login` y `~/.profile`.
- **`bash --norc`**: con shell interactivo ingorará tanto el archivo de inicio del sistema `/etc/bash.bashrc` como el archivo de inicio a nivel de usuario `~/.bashrc`.
- **`bash --rcfile file`**: con shell interactivo tomará `file` como el archivo de inicio ignorando a nivel de sistema `/etc/bash.bashrc` y a nivel de usuario `~/.bashrc`.

#### Ejecutando shell con `su` y `sudo`
A través del uso de estos dos programs (similares) podemos obtener tipos específicos de shells:

**`su`**: cambia el Id de usuaro o lo convierte en superusuario (`root`). Con este comando podemos
invocar ambos shells, el de inicio de sesión y sin inicio de sesión:

- `su - user2, su -l user2 o su --login user2` iniciará un shell de inicio de sesión interactivo como `user2`.
- `su user2` iniciará un shell interactivo y sin inicio de sesión como `user2`.
- `su - root o su -` iniciará un shell de inicio de inicio de sesión interactivo como `root`.
- `su root o su` iniciará un shell interactivo y sin inicio de sesión como `root`.

**`sudo`**: ejecutando comandos como otro usuario (incluyendo el superusuario). Debido a que este
comando se usa principalmente para obtener privilegios temporales de `root`, el usuario que lo
use debe estar en el archivo `sudoers`. Para añadir usuarios a `sudoers` necesitamos convertirnos en
`root` y luego ejecutar:

    usermod -aG sudo user2

Así como `su`, `sudo` nos permite invocar tanto los shell de inicio de sesión como los de sin inicio
de sesión:

- `sudo su - user2, sudo su -l user2 o sudo su --login user2` iniciará un shell de inicio de sesión interactivo como `user2`.
- `sudo su user2` iniciará un shell interactivo sin inicio de sesión como user `user2`.
- `sudo -u user2 -s` iniciará un shell interactivo sin inicio de sesión como `user2`.
- `sudo su - root o sudo su -` iniciará un shell de inicio de sesión interactivo como `root`.
- `sudo -i` iniciará un shell de inicio de sesión interactivo como `root`.
- `sudo -i soma_command` iniciará un shell de inicio de sesión interactivo como `root`, ejecutara el comando y volverá al usuario original.
- `sudo su root o sudo su` iniciará un shell interactivo sin inicio de sesión como `root`.
- `sudo -s o sudo -u root -s` iniciará un shell sin inicio de sesión como `root`.

Cuando se usa `su` o `sudo`, es importante considerar el inicio de un nuevo shell y preguntarnos:
¿necesitamos el entorno del usuario o no? Si es así, usaríamos las opciones que invocan las
shell de inicio de sesión; si no, las que invocan sin inicio de sesión.

#### ¿Qué tipo de shell tenemos?
Para saber en qué tipo de shell estamos trabajando, podemos escribir `echo $0` en la terminal y
obtener la siguiente salida:

- **Inicio de sesión interactivo**: `-bash or -su`
- **Sin inicio de sesión interactivo**: `bash or /bin/bash`
- **Sin inicio de sesión no interactivo (scripts)**: `{name_of_script}`

#### ¿Cuántas shell tenemos?
Para observar cuántos `bash` tenemos ejecutando en el sistema, poder usar el comando:

    ps aux | grep bash

El `user2` en Dbebian ha entrado en una sesión GUI y ha abierto gnome-terminal, luego ha pulsado
`Ctrl + Alt + F1` para entrar en una sesión terminal `tty1`. Finalmente, ha vuelto a la sesión
del GUI presionando `Ctrl + Alt F7` y ha escrito el comando `ps aux | grep bash`. De esta manera,
la salida muestra un shell interactivo sin inicio de sesión a través del emulador de terminal
(`pts/0`) y un shel de inicio de sesión interactivo a través de la propia terminal basada en
texto (`tty1`). Note también como el último campo de cada linea es `bash` para el primero y
`-bash` para el segundo.

#### ¿De dónde shell obtiene la configuración: archivo de inicio?
Note que los scripts de todo el sistema o globales se colocan en el directorio `/etc/`, mientras
que los locales o de nivel de usuario se encuentran en el directorio `home` del usuario (`~`).
Además, cuando hay más de un archivo que buscar, y una vez que ellos es encontrado y
ejecutado, los otros serán ingnorados.

#### Shell interactivo de inicio de sesión

**Nivel global**:
- **`/etc/profile`**: este es el archivo `.profile` de todo el sistema para el shell Bourne y los shells compatibles con Bourne (incluido `bash`). A través de una serie de declaraciones `if` este archivo establece un número de variables como `PATH` y `PS1`, así como origen (si existen) tanto para el archivo `/etc/bash.bashrc` como los del directorio `/etc/prfile.d`.
- **`/etc/profile.d/*`**: este directorio puede contener scripts que son ejecutados por `/etc/profile`.

**Nivel local**:
- **`~/.bash_profile`**: este archivo específico de Bash se utiliza para configurar el entorno del usuario. También puede ser usado para crear el `~/.bash_login` y `~/.profile`.
- **`~/.bash_login`**: este archivo, solo se ejecutará si no hay un archivo `~/.bash_profile`. Su nombre sigiere que debería ser usando para ejecutar los comandos necesarios para el inicio de sesión.
- **`·/.profile`**: este archivo no es específico de Bash y se obtiene solo si no existe `~/.bash_profile` ni `~/.bash_login`, que es lo que normalmente ocurre. Por lo tanto, el propósito principal de `~/.profile` es el de revisar si está ejecutando un shell de Bash, y si fuera afirmativo, obtener `~/.bashrc` (si existe). Normalmente establece la variable `PATH` para que incluya el directorio privado del usuario `~/bin` (si existe).
- **`~/.bash_logout`**: si existe, este archivo específico de Bash hace algunas operaciones de limpieza al salir del shell. Esto puede ser conveniente en casos como los de las sesiones remotas.

#### Exploración de los archivo de configuración de Bash de inicio de sesión interactivo
Mostremos algunos de estos archivos en acción modificando `/etc/profile` y `/home/user2/.profile`.
Añadiremos a cada uno una lína que nos recuerde el archivo que se está ejecutnado:
```sh
echo 'echo Hello from /etc/profile' >> /etc/profile
echo 'echo Hello from ~/.profile' >> ~/.profile
```

#### Shell interactivo sin inicio de sesión

**Nivel global**:
- **`/etc/bash.barcr`**: este es el archivo `.bashrc` de todo el sistema para los shells interactivos `bash`. A través de su ejecución. `bash` se asegura que se está ejecutando interactivamente, comprueba el tamaño de la ventana después de cada comando y establece algunas variables.

**Nivel local**:
- **`~/.bashrc`**: además de llevar a cabo tareas similiares a las descritas para `/etc/bash.bashrc` a nivel de ususario, este archivo específico de Bash suele establecer algunas variables de historial y origen `~/.bash_aliases`. Aparte de eso, este archivo se utiliza normalmente para almacenar alias y funciones específicas de los usuarios.

Asimismo, también vale la pena selañar que `~/.bashrc` se lee si `bash` detecta que su `stdin` es una
conexión de red, por ejemplo, SSH.

#### Exploración de los archivos de configuración de shell no interactivo y de inicio de sesión
Modifiquemos ahora `/etc/bash.bashrc` y `~/.bashrc`:
```sh
echo 'echo Hello from /etc/bash.bashrc' >> /etc/bash.bashrc
echo 'echo Hello from ~/.bashrc' >> ~/.bashrc
```

#### Shell no interactivo de inicio de sesión
Un shell no interactivo con las opciones `-l o --login` es forzado a comportarse como un shell de
inicio de sesión y así los archivos de inicio a ser ejecutados serán los mismo que los de los
shell de inicio de sesión interactivos.

Para probarlo, escribamos un simple script y hagámoslo ejecutable. No incluiremos ningún shebang
porque invocaremos el ejecutable `bash` (`/bin/bash`) desde la línea de comandos.
```sh
echo 'echo "Hello from a script"' > test.sh
chmod +x test.sh
bash -l test.sh
```
¡Funciona! Antes de ejecutar el script, el login tuvo lugar y tanto el archivo `/et/profile` como
el `~/.profile` fueron ejecutados.

Tengamos ahora la salida estándar (stdout) del comando `echo` en la entrada estándar (stdin) de
una conexión `ssh` por medio de un pip (`|`):

    ssh "hello from a noninteractive login shell" | ssh user@ip

Una vez más, `/etc/profile` y `~/.profile` se ejecutan. Aparte de eso, la primera y la última
línea de la salida son bastante reveladoras en lo que respecta al comportamiento de shell. Shelll
no interactivo sin inicio de sesión los scripts no leeen ninguno de los archivos listados arriba,
pero busca en la variable de entorno `BASH_ENV` que expande su valor si es necesario y la usan
como el nombre de un archivo de inicio para lerr y ejecutar comandos.

Típicamente `/etc/profile` y `~/.profile` se aseguran de que tanto `/etc/bash.bashrc` como
`~/.bashrc` se ejecutan después de un inicio de sesión existoso.
```sh
root@debian:~# su - user2
Hello from /etc/bash.bashrc
Hello from /etc/profile
Hello from /home/user2/.bashrc
Hello from /home/user2/.profile
```
Teniendo en cuenta las líneas que se han agregado a los scripts de inicio e invocado un shell
de inicio de sesión interactivo a nivel de usuario con `su - user2` las cuatro líneas de
salida pueden explicarse de la siguiente manera:

1. `Hello from /etc/bash.bashrc` significa que `/etc/profile` se ha obtenido `/etc/bash.bashrc`.

2. `Hello from /etc/profile` significa `/etc/profile` ha sido completamente leído y ejecutado.

3. `Hello from /home/user2.bashrc` significa `~/.profile` se ha obtenido `~/.bashrc`.

4. `Hello from /home/user2/.profile` significa `~/.profile` ha sido completamente leído y ejecutado.

#### Archivos fuente
Algunos script de inicio incluyen o ejecutan otros scripts. Este mecanismo se llama "sourcing".

#### Ejecutando archivos con `.`
En el archivo `/etc/profile` podemos encontrar un ejemplo en el siguiente bloque:
```sh
# include .bashrc if exists
if [ -f "$HOME/.bashrc" ]; then
    . "$HOME/.bashrc"
fi
```
Hemos observado cómo la ejecución de un script puede llevar a la de otro. Así la declaración
`if` garantiza que el archivo `$HOME/.bashrc` (si existe `-f`) se obtendrá.

Además, podemos usar el `.` siempre que hayamos modificado un archivo de inicio y queramos hacer
efectivos los cambios sin necesidad de reiniciar. Por ejemplo:
- Agregar un alias a `~/.bashrc`:

    echo "alias hi='test alias'" >> ~/.bashrc

- Imprime la última línea de `~/.bashrc`:

    tail -n 1 !$

> `!$` se expande hasta el último argumento del comando anterior.

- Ejecutar el archivo manualmente:

    . ~/.bashrc

- Invoar el alias para probar que funciona:

    hi

#### Ejecutar comandos con `source`
El comando `source` es un sinónimp de `.`. Así que para ejecutar `~/.bashrc` tambien se puede
hacer de esta manera:

    source ~/.bashrc

#### El origen de los archivos de incio de Shell: SKEL
`SKEL` es una variable cuyo valor es la ruta absoluta al directorio `skel`. Este directorio sirve
como plantilla para estructura del sistema de archivos de los principales directorio de los
usuarios. Incluye los archivos que serán heredados por cualquier nueva cuenta de usuario que
se cree (incluyendo los archivos de configuración de los shells). El `SKEL` y otras variables
relacionadas se almacenan en `/etc/adduser.conf`, que es el archivo de configuración  para
`adduser`:

    grep -i "skel" /etc/adduser.conf

`SKEL` está configurado como `/etc/skel`; por lo tanto, los scripts de inicio que configurarn
nuestros shell están ahí:

    ls -a /etc/skel

Vamos a crear un directorio en `/etc/skel` para que todos los nuevos usuarios almacenen sus
scripts personales:

1. Como `root` nos movemos a `/etc/skel`:

    cd /etc/skel

2. Listamos su contenido:

    ls -a

3. Creamos un directorio y comprobamos que todo ha ido como se esperaba:
```sh
mkdir personal_scripts
ls -a
```

4. Añadimos un nuevo usuarios y validamos su directorio:
```sh
adduser user2
su - user2
ls -a
```

#### Variables: asignación y referencia
Una variable puede definirse como un nombre que contiene un valor.

En Bash, dar un valor a un nombre se llama *asignación* y es la forma en que creamos o establecemos
las variables. Por otro lado, el proceso de acceder al valor contenido en el nombre se llama
*variable referenciada*.

La sintaxis para la asignación de variables es:

    <variable_name>=<varaible_value>

Por ejemplo:

    distro=zorinos

La variables `distro` es igual a `zorinos`, es decir, hay una porción de memoria que contiene el
valor `zorinos` con `distro` siendo el puntero hacia este.

Tenga en cuenta, que no puede haber ningún espacio a ambos lados del signo de igual cuando se
asigna una variable:
```sh
distro =zorinos # error
distro= zorinos # error
```
A causa del error, Bash leyó `distro` y `zorinos` como órdenes.

Para referencias a una variable (es decir, para comprobar su valor) utilizamos el comando `echo`
que precede al nombre de la variable con un signo `$`:

    echo $distro

#### Nombres de variables
Al elegir un nombre de las variables, hay ciertas reglas que se deben tener en cuenta.

El nombre de una variable puede contener letras (a-z, A-Z), números (0-9) y giones bajos (_):
```sh
distro=zorinos
DISTRO=zorinos
distro_1=zorinos
_distro=zorinos
```
No puede empezar con un número o Bash se confundira:

    1distro=zorinos

No puede contener espacios (ni siquiera usando comillas); por convención, se usan los giones
bajos en su lugar:
```sh
"my distro"=zorinos # error
my_distro=zorinos
echo $my_distro
```

#### Valores de las variables
En lo que respecta a la referencia o valor de las variables, también es importante considerar
una serie de reglas.

las variables pueden contener cualquier carácter alfanumérico (a-z, A-Z, 0-9) así como la
mayoría de los caracteres (?, !, *, ., /, etc.):
```sh
distro=zorin12.4?
echo $distro
```
Los valores de las variables deben ser encerrados entre comillas si contienen espacios simples:
```sh
distro=zorin 12.4 # error
distro="zorin 12.4"
distro='zorsin 12.4'
```
Los valores de las variables también deben ser encerrados entre comillas si contienen
caracteres como los utilizados para la redirección (`<`, `>`) o el símbolo de pipe (`|`). Lo único que
hace el siguiente comando es crear un archivo vacío llamado `zorin`:
```sh
distro=>zorin
ls zorin
```
Esto funciona, siempre y cuando usemos comillas:

    distro=">zorin"

Sin embargo, las comillas simples y dobles no siempre son intercambiables. Según lo que
hagamos con una variable (asignación o referencia), el uso de una u otra tiene implicaciones y
dará resultado diferentes. En el contexto de la asignación de variables, las comillas simples
toman todos los caracteres del valor de la variable *literalmente*, mientras que las comillas
dobles permiten la sustitución de la variable:
```sh
lizard=uromastyx
animal='My $lizard'
echo $animal # My $lizard
animal="My $lizard"
echo $animal # My uromastyx
```
Por otra parte, cuando se hace referencia a una variable cuyo valor incluye algunos espacios
iniciales (o adicionales, a veces combinados con asteriscos) es ibligatorio utilizar comillas
dobles después del comando `echo` para evitar la división de los campos y la expasión de
nombres:
```sh
lizard="    genus   |   uromastyx"
echo $lizard # genus | uromastyx
echo "$lizard" #    genus   | uromastyx
```
Si la referencia de la variable contiene un signo de exclamación de cierre, este debe ser
el último carácter de la cadena (de lo contrario Bash pensará que nos refererimos a un evento
histórico):
```sh
distro=zorin.?/!os # error
distro=zorin.?/!
```
Cualquier "backslash" debe escapar por otro. Además, si una barra invertida es el útlimo
carácter de la cadena y no escapa, Bash interpretará que quremos un salto de línea y nos dará
una nueva línea:
```sh
distro=zorinsos\ # interpreta como nueva línea
distro=zorinos\\
echo $distro
```

#### Variables locales o de shell
Las variables locales o de shell existen solo en el shel en el que se crean. Por convención,
las variables locales se escriben en minúsculas.

Con el fin de realizar algunas pruebas, vamos a crear una variable local. Como se ha explicado
anteriormente. elegimos un nombre apropiado para la variable y la equiparemos a un valor
apropiado. Por ejemplo:
```sh
reptile=tortoise
echo $reptile
```
En ciertos escenarios, cuando se escriben los scripts, la inmutabilidad puede ser una
característica interesante de las variables. Si queremos que nuestras variables sean inmutables,
podemos crearlas en modo de solo lectura (`readonly`):

    readonly reptile=tortoise

Otra opción es convertirlas después de haberlas creado:
```sh
reptile=tortoise
readonly reptile
```
Ahora, si intentamos cambiar el valor de `reptile`, Bash lo negará:

    reptile=lizard # readonly variable

> Para listar todas las variables de solo lectura en nuestra sesióna actual, escriba `readonly -p`.

Un comando útil cuando se trata de variable locales es `set`.

`set` da salida a todas las variables y funciones de shell que se encuentran actualmente
asignadas. Dado que pueden ser muchas líneas, se recomienda usarlo en combinación con un
buscador como `less`:

    set | less

¿Se encuentra nuestra variable `tortoise`?

    set | grep reptile

Sin embargo, `reptile` siendo una variable local, no será heredado por ningún proceso hijo
generado desde el shell actual.

Para remover cualquier variable (local o global), usamos el comando `unset`:

    unset reptile

> `unset` debe ser seguido por el nombre de la variable, no por el simbolo `$`.

#### Variables globales o de entorno
Existen variable globales o de entorno para el shell actual, así como para todos los
procesos subsecuentes que se derivan de este. Por convención, las variables de entorno se
escriben en mayúsculas.

Podemos pasar recursivamente el valor de estas variables a otras variables y el valor de estas
últmias se expandirá finalmente a las primeras:
```sh
my_shell=$SHELL
echo $my_shell
/bin/bash
your_shell=$my_shell
echo $your_shell
/bin/bash
our_shell=$your_shell
echo $our_shell
/bin/bash
```
Para que una variable de shell se convierta en una variable de entorno, se debe utilizar el
comando `export`:

    export reptile

Con `export reptile` hemos convertido nuestra variable local en una variable de entorno para que
los shells hijos puedan reconocerla y usarla.

De la misma manera, `export` puede ser usado para asignar y exportar una variable, todo a la vez:

    export amphibian=frog

> Con `export -n <variable_name>` la variable se convertirpa de nuevo en una variable local.

El comando `export` también nos dará una lista de todas las variables de entorno existentes o
cuando se digita con la opción `-p`:

    export -p

> El comando `declare -x` es equivalente a `export`.

Dos comandos más que puede ser usados para imprimir una lista de todas las variable de entorno
son `env` y `printenv`:

    env | grep -i "pwd"

Además de ser un sinónimo de `env`, a veces podemos usar `printenv` de forma similar a como
usamos el comando `echo` para comprobar el valor de una variable

    printenv PWD

Sin embargo, con `printenv` el nombre de la variable no está precedido por `$`.

#### Ejecución de un programa en un entorno modificado
El comando `env` puede ser usado para modifcar el entorno de shell en el momento de la ejecución
de un programa.

Para iniciar una nueva sesión de Bash con un entorno tan vacío como sea posible, despejando
la mayoría de las variables (así como las funciones y alias), usaremos `env` con la opción `-i`:

    env -i bash

Ahora la mayoría de las variables de nuestro entorno han desaparecido. Y solo quenda unas pocas:

    env

También podemos usar `env` para establecer una variable particular para un programa particular.

Cuando hablamos de los shell no interactivos sin inicio de sesión, observamos como los scripts
no leen ningún archivo de inicio estándar, sino que buscan el valor de la variable
`BASH_ENV` y lo usan como un archivo de inicio si existe.

Demostrémos este proceso:

1. Creamos nuestro propio archivo de inicio llamado `.startup_script` con el siguiente contenido:

    CROCODILIAN=caiman

2. Escribimos un script Bashh llamado `test_env` con el siguiente contenido:
```sh
#!/bin/bash

echo $CROCODILIAN
```

3. Establecemos el bit ejecutable para nuestro script `test_env.sh`:
```sh
chmod +x test_env.sh
```

4. Por último, usamos `env` para establecer `BASH_ENV` en `startup_script` para `test_env.sh`:


#### Variables comunes de entorno
Es hora de revisar algunas de las variables de entorno más relevantes que se establecen en los
archivos de configuración de Bash.

**`DISPLAY`**: en relación con el servidor X, el valor de esta variable se compone normalmente de
tres elementos:
- El hostname (la ausencia de este significa `localhost`) donde se ejecuta el servidor X.
- Dos puntos como delimitdor.
- Un números (normalmente es `0` y se refiere a la pantalla de la computadora).
```sh
printenv | grep -i "display"
```

**`HISTCONTROL`**: esta variable controla qué comandos se guardan en `HISTFILE`. Hay tres valores
posibles:
- **`ignorespace`**: los comandos que empiecen con un espacio no se guardarán.
- **`ignoredups`**: un comando que es el mismo que el anterior no se guardará.
- **`ignoreboth`**: los comandos que caen en cualquiera de las dos categorías no se guardarán.
```sh
echo $HISTCONTROL
```

**`HISTSIZE`**: esto establece el número de comandos que se almacenarán en la memoria mientras dure la
sesión de shell.
```sh
echo $HISTSIZE
```

**`HISTFILESIZE`**: esto establece el número de comandos que se guardarán en `HISTFILE` tanto al
principio como al final de la sesión:
```sh
echo $HISTFILEZIE
```

**`HISTFILE`**: el nombre del archivo que almacena todos los comandos a medida que se escriben. Por
defecto este se encuentra en `~/.bash_history`:
```sh
echo $HISTFILE
```

**`HOME`**: esta variable almacena la ruta absoluta del directorio principal del usuario y se establece
cuando el usuario se conecta. `~` es equivalente a `$HOME`.

**`HOSTNAME`**: variable que almacena el nombre de la máquina en la red:
```sh
echo $HOSTNAME
```

**`HOSTYPE`**: esto almacena la arquitectura de la unidad central de procesamiento (CPU) del equipo:
```sh
echo $HOSTTYPE
```

**`LANG`**: esta variable guarda la información de localización que se utiliza para el sistema:
```sh
echo $LANG
```

**`LD_LIBRARY_PATH`**: esta variable consiste en un conjunto de directorios separados por dos puntos
donde las bibliotecas compartidas son compartidas por los programas.

**`MAIL`**: esta variable almacena el archivo en el que Bash revisa el correo electrónico.

**`MAILCHECK`**: esta variable almacena un valor numérico que indica en segundos la frecuencia con la
que Bash comprueba si hay correo nuevo.

**`PATH`**: esta variable de entorno almacena la lista de directorios donde Bash busca los archivos
ejecutables cuando se le indica que ejecute cualquier programa.

Dos cosas con respecto al valor de `PATH`:
- Los nombre de los directorios se escriben usando rutas absolutas.
- Los dos puntos se usan como delimitadores.

**`PS1`**: esta variable almacena el valor del indicador Bash.

**`PS2`**: normalmente se establece en `>` y se usa como un mensaje de continuidad para comandos largo
de varias líneas.

**`PS3`**: usando como el indicador para el comando `select`.

**`PS4`**: normalmente se establece en `+` y se usa para la depuración.

**`SHELL`**: esta variable almacena la ruta absoluta del shell actual:

**`USER`**: esto almacena el nombre del usuario actual.

#### Creando Alias
Un alias es un nombre sustituto de otro(s) comando(s). Puede ejecutarse como un comando normal,
pero en su lugar ejecuta otro comando según la definición de alias.

La sintaxis para declarar alias es muy sencillas. Estos se declaran escribiendo la palabra clave
`alias` seguida de su asignación. A su vez, dicha asignación consiste en el nombre del alias, un
signo igual y uno o más comandos:

    alias alias_name=command

Por ejemplo:

    alias oldshell=sh

El poder de los alias radica en que nos permiten escribir versiones cortas de comandos largos:

    alias ls="ls --color=auto"

De la misma manera, podemos crear alias para una serie de comandos concatenados, usando el
punto y coma (`;`) como delimitador. Por ejemplo, podemos tener un alias que nos de información
sobre la ubicación de l ejecutable `git` y su versión:

    alias git_info="which git ; git --version"

El comando `alias` producirá un listado de todos los alias disponibles en el sistema.

El comando `unalias` eliminas los alias:

    unalias git_info

Debemos encerrar los comandos entre comillas (simples o dobles) cuando los argumentos o
parámetros tengan espacios.

La variable también puede ser asignada dentro del alias.

Escapara de un alias es útil cuando un alias tiene el mismo nombre que un comando normal. En
este caso, el alias tiene prioridad sobre el comando original, sin embargo, este sigue siendo
accesible al espacar del alias.

#### Expansión y evaluación de las comillas en los alias
Cuando se usan comillas con variable de entorno, las comillas simples hacen que la expresión
sea dinámica:
```sh
alias where?='echo $PWD'
where?
/home/user2
cd Music
where?
/home/user2/Music
```
Sin embargo, con las comillas dobles la expansión se hace de forma estática:
```sh
$ alias where?="echo $PWD"
$ where?
/home/user2
$ cd Music
$ where?
/home/user2
```

#### Persistencia de Alias: scripts de inicio
Al igual que con las variables, para que nuestroas alias ganen persistencia, debemos escribirlo
en scripts de Inicialización que se ejecuten al inicio. Como ya sabemos, un buen archivo para
que los usuarios agreguen sus alias personales es `~/.bashrc`.

#### Creando funciones
En comparación con los alias, las funciones son más programables y flexibles, especialmente
cuando se trata de explotar todo el potencial de las variables incorporadas y parámetros
posicionales de Bash. También son muy buenas para trabajar con estructuras de control de flujo,
como bucles o conficionales. Podemos pensaer en una función como un comando que incluye la
lógica a través de bloques o colecciones de otros comandos.

#### Dos sintaxis para crear funciones
**Usando la palabra clave `function`**: podemos usar la palabra clave `function`, seguida del nombre
de la función y los comandos entre corchetes:
```sh
function function_name {
    commands
}
```

**Usar `()`**: por otro lado, podemos omitir la palabra `function` y usar dos paréntesis justo
después del nombre de la función:
```sh
function_name() {
    commands
}
```
Es común agregar funciones en archivos o scripts. Sin embargo, también se puede escribir con
cada comando en el shell prompt, en una línea diferente que indica una nueva línea después de
un salto de línea.

En cualquier caso, e independientemente de la sintaxis que elijamos, si decidimos saltarnos los
saltos de línea y escribir una funcioón en una sola línea, los comandos deben estar separados
por punto y coma:

    greet() { greeting="Hello world!"; echo $greeting; }

Al igual que con las variables y los alias, si queremos que las funciones sean persistentes a
través de los reinicios del sistema tenemos que agregarlas en los scripts de inicialización
de shell como `~/etc/bash.bashrc` o `~/.bashrc`.

#### Variables especiales incorporadas en Bash
Bourne Again Shell llega con un conjunto de variables especiales que son particularmen útiles
para funciones y scripts. Estas son especiales porque solo pueden ser referenciadas,
no asignadas. Aquí hay una lista de las már relevantes:

**`$?`**: la referencia de esta variable se expande hasta el resultado de la última ejecución
del comando. Un valor de `0` significa éxito y un valor distinto de `0` es error.

**`$$`**: se expande hasta el PID del shell (process ID).

**`$!`**: se expabde hasta el PID del último trabajo en segund plano.

#### Parámetros de posición de `0` a `9`
Se expande a los prámetros o argumentos que se pasan a la función, `$0` expandiéndose al
nombre del script o shell.

Otras variables especiales incorporadas en Bash incluyen:

**`$#`**: se expande al número de argumentos que se le pasan al comando.

**`$@, $*`**: se extiende a los argumentos pasados al comando.

**`$_`**: se expande hasta el último parámetro o el nombre de script.

#### Parámetros de posición en las funciones
Podemos pasarlos a las funciones desde adentro del archivo o el script:
```sh
editors() {
    edit=emcas

    echo "The text editor of $USER is: $editor"
    echo "Bash is not a $1 shell"
}
editors tortoise
```

## Instalar y configurar X11
El sistema X Window es una pila de software que se utiliza para mostrar texto y gráficos en una
pantalla. El aspecto y diseño de un cleinte X no está dictado por el sistema X Window, sino
que es manejado por cada cliente X individual, un administrador de ventanas, o un entorno
de escritorio completo como KDE, GNOME o Xfce.

El sistema X Window es multiplataforma y funciona en varios sistemas operativos como Linux,
BSD, Solaris y otros sistemas de tipo Unix. También hay implementaciones disponibles para
MacOS de Apple y Window de Microsoft.

#### Arquitectura de sistema X Window
El sistema X Window proporciona los mecanismos para dibujar formas bidimensionales básicas
en una pantalla. Se divide en un cliente y un servidor; en la mayoría de las intalaciones en
las que se requeire un escritorio gráfico, estos componentes estáran presentes en el mismo
equipo. El componente cliente toma la forma de una aplicación, como un emulador de terminal,
un juego o un navegador web. Cada aplicación cliente informa al servidor X sobre la ubicación
y el tamaño de su ventana en la pantalla de la computadora. El cliente también maneja lo
que entra en esa ventana, y el servidor X coloca el dibujo solicitado en la pantalla. El
sistema X Window también maneja la entrada de dispositivos como mouse, teclados,
trackpads y más.

El sistema X Window es capaz de funcionar en red donde múltiples clientes X de diferentes
computadoras de una red pueden hacer solicitudes a un solo servidor X remoto. El razonamiento
que subyace a esto es que un adminstrador o usuario pueda tener acceso a una aplicación gráfica
en un sistema remoto que puede no estar disponible en sus sistema local X Window es un sistema
modular, y esto es una característica clave. A lo largo de la existencia de este sistema se han
desarrollado nuevas características que se han añadido a su marco (Framework). Estos nuevos
componentes solo fueron añadidos como extensiones al servidor X, dejando al núcleo del
protocolo X11 intacto. Estas extensiones están contenidas dentro de los archivos de la
biblioteca Xorg. Algunos ejemplos de bibliotecas Xorg incluyen: `libxrandr`, `libXcursor`,
`libX11`, `libxkbfile` así como otras, cada una de ellas proporciona una funcionalidad extendida
al servidor X.

Un administrador de pantalla proporciona un acceso gráfico a un sistema. Este sistema puede
ser un ordenador local o un ordenador a través de una red. El administrador de pantalla se lanza
después de que la máquina se inicia y así comenzará una sesión de servidor X para el usuario
autenticado. El administrador de pantalla también es responsable de mantener el servidor X en
funcionamiento. Algunos ejemplos de gestores de pantalla son: GDM, SDDM y LightDM. Cada
instancia de un servidor X en funcionamiento tiene un nombre de pantalla para indentificarlo.
El nombre de la pantalla contiene lo siguiente:

    hostname:displaynumber.screennumber

El nombre de la pantalla también indica una aplicación gráfica donde debe ser renderizada y
sobre cuál host (si se utiliza una conexión X remotea).

El `hostname` se refiere al nombre del sistema que monstrará la aplicación. Si falta un nombre
de host en el nombre de pantalla, entonces se asume que es el host local.

El `displaynumber` hace referencia a la colección de "pantallas" que están en uso, ya sea una
sola o varias pantallas en una estación de trabajo. A cada sesión de servidor X en ejecución se
le da un número de pantalla que comienza `0`.

Por defecto el `screennumber` es `0`. Esto puede ser el caso si solo una pantalla o varias pantallas
físicas están configuradas para trabajar como una sola pantalla. Cuando todas la pantallas en
una configuración de múltiples monitores se combinan en una lógica, las ventanas de la
aplicación pueden moverse libremente entre las pantallas. En situaciones en las que cada
pantalla está configurada para funcionar de manera independiente, cada una albergará las
ventanas de aplicación que se abran dentro de ella y las ventanas no podrán moverse de una
patalla a otra. Cada pantalla independiente tendrá un propio número asignado. Si solo hay
una lógica en uso, entonces se omite el punto y el número de pantalla.

El nombre de una sesión X en curso se almacena en la variable de entorno `DISPLAY`:

    echo $DISPLAY # :0

La salida detalla lo siguiente:

1. El servidor X en uso está en el sistema local, por lo tanto no hay nada impreso a la izquierda de los dos puntos.

2. La sesión actual del servidor X es la primera como indica el `0` que sigue a los dos puntos.

3. Solo hay una pantalla lógica en uso, por lo que un número de pantalla no es visible.

Ejemplo de configuración en pantalla:

![example](../pics/01.png)

**(A)**: un solo monitor, con una sola configuración de visualización y una sola pantalla.

**(B)**: configurado como una sola pantalla, con dos monitores físicos como una sola. Las ventanas
de aplicación pueden moverse libremente entre los dos monitores.

**(C)**: una configuración de pantalla única, sin embargo, cada monitor es una pantalla
independietne. Ambas pantallas seguirán compartiendo los mismos dispositivos de entrada como
el teclado y el mouse, pero una aplicación abierta en la plantalla `:0.0` no puede ser movida
a la pantalla `:0.1` o viceversa.

Para iniciar una aplicación en una pantalla específica, asigne el número de pantalla a la
variable de entorno `DISPLAY` antes de lanzar la aplicación:

    DISPLAY=:0.1 firefox &

Este comando iniciaría el navegador web Firefoz en la pantalla de la derecha. Algunos conjuntos
de herramientas también proporcionan opciones de línea de comandos para ordenar a una
aplicación que se ejecute en una pantalla especifica. Por ejemplo, `--screen` y `--display`
de `gtk-options`.

#### Configuración de un servidor X
Tradicionalmente, el principal archivo de configuración que se utiliza para configurar un
servidor X es el archivo `/etc/X11/xorg.conf`. En las distribuciones modernas de Linux, el
servidor X se configurará así mismo en timpo de ejecución cuando este es iniciado, y por lo
tanto no puede existir ningún archivo `xorg.conf`.

El archivo `xorg.conf` está dividido en estrofas llamadas `secciones`. Cada sección comienza con
el término `Section` y después de este término se encuentra el nombre de la sección que se
refiere a la configuración de un componente. Cada `Section` está correspondientemente terminada
por una `EndSection`. El típico archivo `xorg.conf` contiene las siguientes secciones:

**`InputDevice`**: se utiliza para configurar un modelo específico de taclado o mouse.

**`InputClass`**: en las distribuciones modernas de Linux esta sección se encuetra típicamente en
un archivo de configuración separado y localizado en `/etc/X11/xorg.conf.d/`. El `InputClass`
se usa para configurar una clase de dispositivos de hardware como teclados y mouses en
lugar de un componente específico de hardware. Ejemplo del archivo
`/etc/X11/xorg.conf.d/00-keyboard.conf`:
```conf
Section "InputClass"
    Identifier "system-keyboard"
    MatchIsKeyboard "on"
    Option "XkbLayout" "us"
    Option "XkbModel" "pc105"
EndSection
```
La opción para `XkbLayout` determina la disposición de las teclas de un teclado, como Dvorak para
diestros y zurdos, QWERTY e idioma. La opción para `XkbModel` se usa para definir el tipo de
teclado en uso. Una tabla de modelos, diseños y sus descripciones se pueden encontrar en
`xkeybpoard-config`. Los archivos asociados a las distribuciones de teclado se pueden encontrar
en `/usr/share/X11/xkb`. Un ejemplo de siseño de teclado griego plotónico en una computadora de
Chromebook se vería como el siguiente:
```conf
Section "InputClass"
    Identifier "system-keyboard"
    MatchIsKeyboard "on"
    Option "XkbLayout" "gr(polytonic)"
    Option "XkbModel" "chromebook"
EndSection
```
Alternativamente, la disposición del teclado puede ser modificada durante una sesión X en curso
con el comando `setxkbmap`. Aqupi hay un ejemplo de este comando que configura la disposición del
Politónico Griego en una computadora Chromebook:

    setxkbmap -model chromebook -layout "gr(polytonic)"

Este ajuste solo durará mientras la sesión X esté en uso. Para que estos cambios sean
permanentes, modifique el archivo `/etc/X11/xorg.conf.d/00-keyboard.conf` para incluir los ajustes
necesarios.

Las distribuciones modernas de Linux proporciona el comando `localectl` a través de `systemd`
que tamién puede ser usado para modificar una disposición del teclado y creará automáticamente
el archivo de configuración `/etc/X11/xorg.conf.d/00-keyboard.conf`. Aquí hay un ejemplo de
configuración de un teclado Politónico Griego en una Chromebook, esta vez usando el comando
`localectl`:

    localectl --no-convert set-x11-keymap "gr(polytonic)" chromebook

La opción `--no-convert` se usa aquí para evitar que `localectl` modifique el mapa de teclas de la
consola de host.

**`Monitor`**: la sección `Monitor` describe el monitor físico que se utiliza y dónde está conetado.
El siguiente es un ejemplo de configuración que muestra un monitor físico conetado al segundo
puerto de la pantalla y que se utiliza como monitor primario:
```sh
Section "Monitor"
    Identifier"DP2"
    Option "Primary" "true"
EndSection
```
**`Device`**: la sección `Device` describe la tarjeta de video física que se utiliza. tambińe
contendrá el módulo del núcleo utilizado como controlador de la tarjeta de video, junto con su
ubicación física en la placa base:
```sh
Section "Device"
    Identifier "Device0"
    Driver "i915"
    BusID "PCI:0:2:0"
EndSection
```
**`Screen`**: la sección `Screen` una las secciones `Monitor` y `Device`. Un ejemplo de la sección
`Screen` podría ser la siguiente:
```sh
Section "Screen"
    Identifier "Screen0"
    Device "Device0"
    Monitor "DP2"
EndSection
```
**`ServerLayout`**: la sección `ServerLayout` agrupa todas las secciones como el mouse, teclado y
las pantallas en una interfaz del sistema X Window:
```sh
Section "ServerLayout"
    Identifier "Layout-1"
    Screen "Screen0" 0 0
    InputDevice "mouse1"
    InputDevice "system-keyboard" "CorePointer" "CoreKeyboard"
EndSection
```
Los archivos de configuración especificados por el usuario también residen en
`/etc/X11/xorg.conf.d/`. Los archivos de configuración proporcionados por la distribución se
localizan en `/usr/share/X11/xorg.conf.d/`. Los archivos de configuración ubicados dentro de
`/etc/X11/xorg.conf.d/` son analizados antes del archivo `/etc/X11/xorg.conf` si existen en el
sistema.

El comando `xdpinfo` se una en una computadora para mostrar información sobre una instancia de
servidor X en ejecución:

    xdpinfo

#### Creando un archivo de configuración básica de Xorg
Aunque X creará su configuración después del inicio del sistema en instalaciones modernas de
Linux, un archivo `xorg.conf` todavía puede ser usado. Para generar un archivo
`/etc/X11/xorg.conf` permanente, ejecute el siguiente comando:

    sudo Xorg -configure

> Si ya existe una sesión X en ejecución. tendra que especificar un `DISPLAY` diferente en el comando, por ejemplo: `sido Xorg :1 -configure`.

En alguas distribuciones de Linux, el comando `X` puede ser usado en lugar de `Xorg`, ya que `X`
es un enlace simbólico a `Xorg`.

Se creará un archivo `xorg.conf.new` en su actual directorio de trabajo. El contenido de este
archivo se derova de lo que el servidor X ha encontrado disponible en el hardware y los
controladores del sistema local. Para user este archivo, tendrá que ser movido al directorio
`/etc/X11/` y renombrado a `xorg.conf`:

    sudo mv xorg.conf.new /etc/X11/xorg.conf

#### Wayland
Wayland es el nuevo protocolo de visualización diseñado para reemplazar el sistems W Window.
Muchas distribuciones modernas de Linux lo usarn como un servidor de visualización por defecto.
Se supone que es más ligero en cuanto a recursos del sistema y su instalación ocupa menos
espacio en disco que X. El proecto comeznzó en 2010 y todavía esta en desarrollo activo,
incluyendo el trabajo de los desarrolladores activos y anteriores de X.org.

A diferencia del sistema X Window, no hay niniguna instancia de servidor que se ejecute entre el
cliente y el kernel. En su lugar, una ventana cliente trabaja con su propio código o el de un
kit de herramientas (como Gtk+ o Qt) para proporcionar el renderizado. Para hacer el
renderizado, se hace una petición al kernel de Linux a través del protocolo Wayland. El
kernel reenvía la solicitud a través de este protocolo al Wayland compositor que se encarga
de la entrada de dispositivos, la gestión de ventanas y la composición. El compositor es
parte del sistema que combina los elementros renderizados en una salida visual en la
pantalla.

La mayoría de las herramientas modernas como Gtk+ 3 y Qt 5 han sido actualizada para
permitir la renderización a un sistema X Window o a una computadora con Wayland. No todas las
aplicaciones autónomas han sido escritas para soportar el renderizado en Wayland hasta ahora.
Para las aplicaciones y frameworks que todavía toene como objetivo que se ejecute el
sistema X Window, la aplicación puede ejecutarse dentro de XWayland. El sistems XWayland
es un servidor X separado que se ejecuta dentro de un cliente de Wayland y por lo tanto,
renderiza el contenido de una ventana de cliente dentro de una isntancia de servidor X
independiente.

Así como el sistema X Windows utiliza una vaiable de entorno `DISPLAY` para controlar las
pantallas en uso, el protocolo Wayland utiliza una variable de entorno `WAYLAND_DISPLAY`. A
continuación se muestra la salida de un sistema que ejecuta una pantalla Wayland:
```sh
echo $WAYLAND_DISPLAY
wayland-0
```
Esta variable de entorno no está disponible en los sistemas que ejecutan X.

## Escritorios gricos
#### Sistema X Window
En Linux y otros sistemas operativos similares a Unix en los que se emplea, el sistema X
Window (conocido como X11 o simplemente X) proporciona los recuros de bajo nivel relacionados
con la reepresentación de la interfaz gráfica y la interacción del usuario con ella.
Por ejemplo:

- El manejo de los eventos de entrada, como los movimiento del mouse o las pulsaciones de teclas.
- La capacidad de cortar, copiar y pegas el contenido del texto entre aplicaciones separadas.
- La interfaz de programación que otros programas utilizan para dibujar elementos gráficos.

Aunque el sistema X Window se encarga de controlar la pantalla gráfica, no pretende dibujar
elementos visuales complejos. Las formas, los colores, los matices y cualquier otro efecto
visual son generados por la aplicación que se ejecuta sobre X. Este enfoque da a las
aplicaciones mucho espacio para crear interfaces personalizadas, pero también pueden dar
lugar a sobreusos de desarrollo que están fuera del alcande de la aplicación y a incoherencias
en el aspecto y comportamiento cuando se comparan con las interfaces de otros programas.

Desde el punto de vista del desarrollador, la introducción del ambiente de escritorio facilita
la programación de la interfaz gráfica de usuario vinculada al desarrollo de la aplicación
subyacente, mientras que desde el punto de vista del usuario proporciona una experiencia
coherente entre las distintas aplicaciones. Los entornos de escritorio reunen interfaces de
programación, bibliotecas y programas de apoyo que cooperan para ofrecer conceptos de diseño
tradicionales pero aún en evolución.

#### Ambientes de escritorio
La tradicional interfaz gráfica de la computadora de escritorio consiste en varias ventanas
asociada con procesos en ejecución. Como el sistema X Window por sí solo ofrece solo
características interactivas básicas, la experiencia completa del usuario depende de los
componentes proporcionados por el entorno de escritorio. Probablemente el componente más
importante de un entorno de escritorio, es el administrador de ventanas (windows manager)
que controla la colocación y decoración de las ventanas. Es el gesto de ventanas que añade
la barra de título a la ventan, los botones de control y gestiona el cambio entre las
ventanas abiertas.

Todos los entornos de escritorio proporcionan un gestor de ventnas que se ajusta al aspecto
y a la sensación de un kit de herramientas de widgets. Los widgets son elementos visuales
informativos o interactivos, como botnoes o campos de entrada de texto, distribuidos dentro
dentro de la ventana de la aplicación. Los componentes estándares del escritorio y el
propio gesto de ventanas dependen de tales kits de herramientas de widgets para ensamblar
sus interfaces.

Las bibliotecas de software, como Gtk+ y Qt, proporcionan widgets que los programadores
pueden usar para construir elaboradas interfaces gráficas para sus aplicaciones.
Históricamente, las aplicaciones desarrolladas con GTK+ no se parecían a las aplicaciones
hechas con Qt y viceversa, pero el mejor soporte de temas de los entornos de escritorio de
hoy en día hacen que la distinción sea menos obvia.

En general, Gtk+ y Qt ofrecen las mismas características en cuanto a los widgets. Los
elementos interactivos simples pueden ser indistinguibles, mientas que los widgets compuestos,
como la ventan de diálogo que utilizan las aplicaciones para abrir y gurardar archivos,
sin embargo, pueden tener un aspecto bastante diferente. No obstante, las aplicaciones
construidas con conjuntos de herramientas distintos pueden ejecutarse simultáneamente,
independietemente del conjunto de herramientas de widgets utilizado por los demas componentes
del escritorio.

Además de los componentes básicos del escritorio, que podrían considerarse programas
individuales por sí mismos, los entornos de escritorio persiguen la metáfora del escritorio
proporcionando un conjunto mínimo de accesorios desarrollados bajo las mismas pautas de
diseño. Las variaciones de las siguientes aplicaciones son comúnmente proporcionadas por
todos los principales entornos de escritorio:

**Aplicaciones relacionadas con el sistema**: emuladores de terminal, gestor de archivos, gestor de
instalación de paquetes, herramientas de configuración del sistema.

**Comunicación e Internet**: Administrador de contactos, cliente de correo electrónico, navegador web.

**Aplicaciones de oficina**: calendario, calculadora, editor de texto.

Los entornos de escritorio pueden incluir muchos otros servicios y aplicaciones: el salduo de
la pantalla de inicio de sesión, el gestero de sesiones, la comunicación entre procesos,
el agente de credenciales, etc. También incorporan características proporcionadas por
servicioss de sistemas de terceros, como PulseAudio para el sonido y CUPS para la impresión.
Estas características no necesitan el entorno gráfico para funcionar, pero el entorno de
escritorio proporciona gráficas frontend para facilitar la configuración y el funcionamiento
de esos recursos.

#### Entornos de escritorio populares
Muchos sistemas operativos patentados solo admiten un único entorno oficial de escritorio
que está vinculador a su lanzamiento particular y que se supone no debe ser modifiado. A
diferencia de ellos, los sistemas operativos basados en Linux soportan diferentes opciones de
entornos de escritorio que pueden utilizarse en conjunto con X. Cada entorno de escritorio
tiene sus propias características, pero normalmente comparten algunos conceptos de diseño
comunes:
- Un lanzador de aplicaciones que lista las aplicaciones integradas y de terceros disponibles en el sistema.
- Reglas que definen las aplicaciones predeterminadas asociadas a los tipos de archivos y protocolos.
- Herramientas de configuración para personalizar la apariencia y el comportamiento del entorno de escritorio.

Gnome es uno de los entorno de escritorio más populares, siendo la primera opción en
distribuciones como Fedora, Debian, Ubuntu, SUSE Linux Enterprise, Red Hat Enterprise Linux,
CentOS, etc. En su versión 3, Gnome trajo grande cambios en su aspecto y estructura, alejándose
de la metáfora del escritorio e introduciendo el Gnome SHell como su nueva interfaz.

El lanzador de pantalla completa Gnome Shell Activities reemplazó al tradicional lanzador
de aplicaciones y a la barra de tareas. Sin embargo, todavía es posible usar Gnome 3 con
el aspecto antiguo eligiendo la opción de Gnome Classic en la pantalla de inicio de sesión.

KDE es un gran ecosistema de aplicaciones y plataforma de desarrollo. Su últoma versión de
entorno de escritorio, KDE Plasma, se utiliza por defecto en openSUSE, Mageia, Kubuntu, etc.
El emploe de la biblioteca Qt es la característica más destacada de KDE, que le da su aspecto
inconfunfible y una plétora de aplicaciones originales. KDE incluso proporciona una herramienta
de configuración para asegurar la cohesión visual con las aplicaciones Gtk+.

Xfce es un entorno de escritorio que pretende ser eséticamente agradable sin consumir muchos
recuros de la máquina. Su estructura está alamente modularizada, permitiendo al usuario activar
y descativar componentes según sus necesidades y preferencias.

Hay muchos otros entornos de escritorio para Linux, normalmente proporcionados por
bifuraciones de distribuciones alternativas. La distribución Linux Mint, por ejemplo, proporciona
dos entornos de escritorio originales: Cinnamon Y MATE. LXDE es un entorno de escritorio
adaptado al bajo consumo de recursos, lo que lo convierte en una buena opción para su
instalación en equipos antiguos o en ordenadores monoplaca. Aunque no ofrece todas las
características de los entornos de escritorio más pesados, LXDE ofrece todas las
características básicas que se esperan de una moderna interfaz gráfica de usuario.

#### Interoperabilidad de escritorio
La diversidad de entornos de escritorio en los sistemas operativos basados en Linux impone
un reto: ¿cómo hacer que funcionen correctamente con aplicaciones gráficas o servicios de
sistema de teceros sin tener que implementar un soporte específico para cada uno de ellos?
los métodos y especificaciones compartidas entre entornos de escritorio mejoran en gran
medida la experiencia del usuario y resuelven muchos problemas de desarrollo, ya que las
aplicaciones gráficas deben interactuar con el entorno de escritorio actual, independietemente
del entorno de escritorio para el que fueron diseñadas originalmente. Además, es importante
mantener la configuración general del escritorio si el usuario acaba cambiando su elección
de entorno de escritorio.

La organización freedesktop.org mantinene un gran cuerpo de especificaciones para la
interoperabilidad de los ordenadores de sobremesa. La adopción de la especificación completa
no es obligatoria, pero muchas de ellas se utilizan ampliamente:

**Ubicaciones de directorios**: donde se encuentran los ajustes personales y otros archivos
específicos del usuario.

**Entradas en el escritorio**: las aplicaciones de línea de comandos pueden ejecutarse en el
entorno de escritorio a través de cualquier emulador de terminal, pero sería demasiado
confuso hacer que todas ellas estuvieran disponibles en el lanzado de aplicaciones. Las
entradas de escritorio son archivos de tesxto que terminan en `.desktop` que son utilizados
por el entorno de escritorio para recopilar información sobre las aplicaciones de escritorio
disponibles y cómo utilizarlas.

**Aplicación de arranque automático**: entradas de escritorio que indican la aplicación que
debe iniciarse automáticamente después de que el usuario haya iniciado la sesión.

**Arrastrar y soltar**: cómo las aplicaciones deben manejar los eventos de arrastart y soltar.

**Papalera (trash can)**: la ubicación común de los archivos eliminados por el administrador de
archivos, así como los métodos para almacenar y eliminar archivos de allí.

**Temas de iconos**: el formato común de las bibliotecas de iconos intercambiables.

La facilidad de uso que ofrecen los entornos de escritorio tienen un inconveniente en comparación
con las interfaces de texto como shell: la capacidad de proporcionar acceso remoto. Mientras
que un entorno de shell de línea de comandos de un equipo remoto puede ser fácilmente accesible
con herramientas como `ssh`, el acceso remoto a entorno gráficos requiere métodos diferentes
y pueden no lograr un rendimiento satisfactorio en conexiones más lentas.

#### Acceso no local
El sistema X Window adopta un diseño basado en pantallas autónomas, donde el mismio
administrador de pantalla puede controlar máas de una sesión de escritorio gráfico al mismo
tiempo. En esencia, una pantalla es análoga para un terminal de texto: ambas se refieren a una
máquina o aplicación de software utilizada como punto de entrada para establecer una sesión
de sistema operativo independiente. Aunque la configuración más común implica una sesión gráfica
singular que se ejecuta en la máquina local, también son posibles otras configuraciones
menos convencionales:
- Cambiar entre sesiones de escritorio gráfico activas en la misma máquina.
- Más de un conjunto de dispositivos de visualización (por ejemplo, pantalla, telcado, mouse) conectados a la misma máquina, cada una controlando su propia sesión de escritorio gráfica.
- Sesiones remotas de escritorio gráfico, donde la interfaz gráfica se envía a través de la red a una pantalla remota.

Las sesiones de escritorio remotos son soportadas por X, que emplea el X Display Manager
Control Protocol (XDMCP) para comunicarse con las pantallas remotas. Debido a su alto uso de
ancho de badan, el XDMCP se utiliza raramente a través de Internet o en redes LAN de baja
velocidad. Los problemas de seguridad también son una preocupación con XDMCP: la pantalla
local se comunica con un administrador de pantalla X remoto con privilegios para ejecutar
procedimientos remotos, por lo que una eventual vulnerabilidad podría favorcer la ejecución de
comandos arbitrarios ocon privilegios en el equipo remoto.

Además, el XDMCP requiere que se ejecute X instancias en ambos extremos de la conexión, lo
que puede hacerla inviable si el sistema X Window no está disponible para todas las máquinas
involucradas. En la práctica, se utilizan otros métodos más eficientes y menos invasivos para
establecer sesiones de escritorio gráfico a distancia.

Virtual Network Computing (VNC) es una herramienta de plataforma independiente para ver y
controlar entorno de escritorios remotos usando el protocolo Remote Frame Buffer (RFB). A
través de este, los eventos producidos por el teclado y el mouse, se transmiten al escritorio
remotor, que a su vex devuelven cualquier actualización de la pantalla para ser mostrados
localmente. Es posible ejecutar muchos servidors VNC en la misma máquina, pero cada servidor
VNC necesita un puerto TCP exclusivo en la interfaz de red que acepte las solicitudes de sesión
entrantes. Por convención, el primer servidor VNC debe user el puerto TCP 5900, el segundo
debe usar el 5901, y así sucesivamente.

El servior VNC no necessita privilegios especiales para funcioanr. Por ejemplo, un usuario
ordinario puede iniciar sesión en su cuenta remota e iniciar su propio servidor VNC dese allí.
Luego, en la máquina local puede utilizar cualquier aplicación cliente de VNC para acceder
al escritorio remoto. El archivo `~/.vnc/xstartup` es un script de shell eejecutado por el
servidor VNC cuando se inicia y puede ser utilizado para definir qué entorno de escritorio,
el servidor VNC podrá a disposición del cliente VNC. Es importante señalar que VNC no
proporciona métodos modernos de cifrado y autenticación de forma nativa, por lo que debe
utilizarse junto con una aplicación de terceros que proporcione esas características. Los
métodos que implicant túneles de VPN y SSH se utilizan a menudo para asegurar las conexiones
de VNC.

Remote Desktop Protocol (RDP) se utiliza principalmente para acceder de forma remota al
escritorio de un sistema operativo Microsoft Windows a través del puerto de red TCP 3389.
Aunque utiliza el protocolo RDP de Microsoft, la implementación cliente en los sistemas
Linux son programas de código abierto licenciado bajo GNU General Public License (GLP) y no
tienen restricciones legales de uso.

Simple Protocol for Independiente Computing Envirionments (Spice) comprenden un conjunto de
herramientas destinadas para acceder al entorno de escritorio de los sistema virtualizados,
ya sea en la máquina local o en una ubicación remoto. Además, el protocolo Spice ofrce
características nativas para integrar los sistemas locales y remotos como la posibilidad de
acceder a los pospositivos locales (por ejemplo, los altavoces de sonido y los dispositivos
USB conectados) desde la máquina remota y el intercambio de archivos entre los dos sistemas.

Hay comandos específicos del cliente para conectarse a cada uno de estos protocolos de
escritorio remoto, pero el cliente de escritorio remoto Remmina proporciona una interfaz
gráfica integrada que facilita el proceso de conexión, almacenando opcionalmente la
configuración de la conexión para su uso posterior. Remmina tiene plugins para cada protocolo
individual y hay un plugin para XDMCP, VNC, DRP y Spice. La elección de la herramienta
adecuada depdende de los sistemas operativos implicados, la calidad de la conexión de red
y las características del entorno de escritorio remoto que debe estar disponibles.

## Administrar cuentas de usuario y de grupo y los archivos de sistema relacionados con ellas
#### Agregando cuentas de usuario
En linux, puedeañadir una nueva cuenta de usuario con el comando `useradd`. Por ejemplo,
con privilegios de root, puede crear una nueva cuenta de usuario llamada `Michael` con una
configuración por defecto, usando lo siguiente:

    useradd michael

Cuando se ejecuta el comando `useradd`, la información de usuario y grupo almacenada en la
base de datos de contraseñas y grupos se actualiza para la cuenta de usuario recién creada,
y si especifica, también se crea el directorio principal del nuevo usuario. Adicionalmente
se crea un grupo con el mismo nombre de la cuenta de usuario.

Una vez que haya creado el nuevo usuario, puede establecer su contraseña usando el comando
`passwd`. Puede reviar su ID de usuario, ID de grupo y los grupos a los que pertenece a través
de los comandos `id` y `groups`.
```sh
passwd michael
id michael
groups michael
```
Las opciones más importantes que se aplican al comando `useradd` son:

- **`-c`**: crear una nueva cuenta de usuario con comentarios personalizados.
- **`-d`**: crear una nueva cuenta de usuario con un directorio de inicio específico.
- **`-e`**: crear una nueva cuenta de usuario estableciendo una fecha específica en la que se desactivará.
- **`-f`**: crear una nueva cuenta estableciendo el número de días después de que expire una contraseña, durante los cuales el usuario debe actualizar (de lo contrario, la cuenta se desactivará).
- **`-g`**: crear una nueva cuenta de usuario con un GID específico.
- **`-G`**: crear una nueva cuenta de usuario añadiéndola a múltiples grupos secundarios.
- **`-k`**: crear una nueva cuenta de usuario copiando los archivos del "kel" de un directorio personzalizado específico (opción válida con `-m`).
- **`-m`**: crear una nueva cuenta de usuario con su directorio principal.
- **`-M`**: crear una nueva cuenta de usuario sin su directorio principal
- **`-s`**: crear una nueva cuenta de usuario con un shell de acceso específico.
- **`-u`**: crear una nueva cuenta de usuario con un UID específico.

#### Modificación de las cuentas de usuario
En ocasiones es necesario cambiar un atributo de una cuenta de usuario existente, como
también el nombre de usuario, el shell, la fecha de caducidad de la contraseña, etc. En tales
casos, necesitas usar el comando `usermod`:
```sh
usermod -s /bin/tcsh michael
usermod -c "Micheal User Account" michael
```
Al igual que el comando `useradd` el comando `usermod` requiere privilegios de root.

Las opciones más importantes que se aplican al comando `usermod` son:
- **`-c`**: agrega un breve comentario a la cuente de usuario.
- **`-d`**: cambiar el directorio princial de la cuenta de usuario. Cuando se usa con la opción `-m`, los contenidos del directorio principal actual se mueven al nuevo directorio principal, que a su vez se crea de no existir.
- **`-e`**: establece la fecha de expiración de la cuenta de usuario.
- **`-f`**: establece el número de días después de que una contraseña expira, durante los cuales el usuario debe actualizar la contraseña (de lo contrario la cuenta de desactivará).
- **`-g`**: cambia el grupo primario de la cuenta de usuario (el grupo debe existir).
- **`-G`**: añade grupos secundarios a la cuenta de usuario especificada. Cada grupo debe existir y debe estar separado por coma, sin espacios en blanco. Si se usa sola, esta opción elimina todos los grupos existente a los que pertenece, mientras que cuando se usa con la opción `-a`, simplemente añade nuevos grupos secuandarios a los ya existentes.
- **`-l`**: cambia el nombre de usuario de la cuenta de usuario especificada.
- **`-L`**: bloquea la cuenta de usuario especificada. Esto pone un signo de exclamación delante de la contraseña ecriptada dentro del archivo `/etc/shadow`, deshabilitando así el acceso con una contraseña para ese usuario.
- **`-s`**: cambia el shell de acceso de la cuenta de usuario especificado.
- **`-u`**: cambia el UID de la cuenta de usuario especificado.
- **`-U`**: desbloquea la cuenta de usuario especificada. Esto elimina el signo de exclamación delante de la contraseña cifrada en el archivo `/etc/shadow`.

#### Eliminando cuentas de usuario
Si quieres borrar una cuenta de usuario, puedes usar el comando `userdel`. En particular, este
comando actualiza la información almacenada en las bases de datos de las cuentas, borrando
todas las entradas referentes al usuario especificado. La opción `-r` también elimina el
directorio principal del usuario y todo su contenido, junto con el spool de correo del
usuario. Otros archivos, localizados en otros lugares, deben ser buscados y eliminados
manualmente.

    userdel -r michael

Al igual que con la gestión de usuarios, puede añadir, modificar y eliminar grupos usando
los comandos `groupadd`, `groupmod`, y `groupdel` con privilegios de root.

    groupadd -g 1090 developer

La opción `-g` crea un grupo con un GID específico.

Si luego desea renombrar el grupo de `developer` a `web-developer` y cambiar su GID, puede
ejecutar lo siguiente:

    groupmod -n web-developer -g 1050 developer

Finalmente, si quieres borrar el grupo `web-developer`, puede ejecutar lo siguiente:

    groupdel web-debeloper

No se puede eliminar un grupo si es el grupo principal de una cuenta de usuario. Por lo tanto
debe eliminar el usuario antes de eliminar el grupo. En cuanto a los usuarios, si elimina un
grupo, los archivos pertenecientes a ese grupo pemanecen en su sistema de archivo y no se
eliminan ni se asignan a otro grupo.

#### El directorio `skel`
Cuando añades una nueva cuenta de usuario, incluso creando su directorio principal, el
directorio recién creado se carga de archivos y carpetas que se copian del directorio skel
(por defecto `/etc/skel`). La idea detrás de esto es simple: un administrador del sistema quiere
agregar nuevos usuarios que tengan los mismos archivos y directorios en su carpeta principal.
Por lo tanto, si desea personalizar los archivos y carpetas que se crean automáticamente en
el directorio principal, debe añadir estos nuevos archivos y carpetas al directorio skel.

#### El archivo `/etc/login.defs`
En linux, el archivo `etc/login.defs` especifica los parámetros de configuración que controlan
la creación de usuarios y grupos. Además, los comandos anteriores toman por defecto valores
de este archivo.

Las directivas más importantes son:

**`UID_MIN y UID_MAX`**: el rango de ID de usuari que puede ser asignado a los nuevos usuarios
ordinarios.

**`GID_MIN y GID_MAX`**: el rango de ID de grupo que puede ser asignado a nuevos grupos ordinarios.

**`CREATE_HOME`**: especifica si un directorio principal debe ser creado por defecto para los
nuevos usuarios.

**`USERGROUPS_ENAB`**: especifica si el sistema debe crear por defecto un nuevo grupo para cada
nueva cuenta de usuario con si mismo nombre, y a su vez al eliminar la cuenta también se debe
eliminar el grupo principal del usuario si ya no contiene miembros.

**`MAIL_DIR`**: el directorio de la cola de correo.

**`PASS_MAX_DAYS`**: el número máximo de días que una contraseña puede ser usada.

**`PASS_MIN_DAYS`**: el número mínimo de días permitido entre los cambios de contraseña.

**`PASS_MIN_LEN`**: la longitud mínima aceptable de la contraseña.

**`PASS_WARN_AGE`**: el número de días de advetencia antes de que una contraseña expire.

#### El comando `passwd`
Este comando se utiliza principalmente para cambiar la contraseña de un usuario. Cualquier
usuario puede cambiar su propia contraseña, pero solo root puede cambiar la contraseña de
cualquier usuario. Esto sucede porque el comando `passwd` tiene el bit SUID puesto (una
`s` en lugar del flag ejecutable para el propietario), lo que significa que se ejecuta con los
privilegios del propietario del archivo (root).

Dependiendo de las opciones de `passwd` utilizadas, puede controlar aspectos específicos del
envejecimiento de las contraseñas tales como:
- **`-d`**: borrar la contreñas de una cuenta de usuario (deshabilitando así al usuario).
- **`-e`**: fuerza la cuenta de usuario a cambiar de contraseña.
- **`-i`**: establecer el número de días de incatividad después de que una contraseña expire, durante los cuales el usuario debe actualizar la contraseña (de lo contrario, la cuenta será desactivada).
- **`-l`**: bloquea la cuenta de usuario (la contraseña cifrada se prefija con un signo de exclamación en el archivo `/etc/shadow`).
- **`-n`**: establece la duración mínima de la contraseña.
- **`-S`**: información de salida sobre el estado de la contraseña de una cuenta de usuario específica.
- **`-u`**: desbloquea la cuenta de usuario (el signo de exclamación se elimina del campo de la contraseña en el archivo `/etc/shadow`).
- **`-x`**: establece la duración máxima de la contraseña.
- **`-w`**: determina el número de días de advertencia antes de que la contraseña expire, durante los cuales se advierte al usuario que debe cambiarla.

#### El comando `change`
Este comando determinado como "change age", se usa para cambiar el período de la contraseña
de un usuario. El comando `change` está restringido a root, expecto la opción `-l`, que puede ser
usada por usuarios ordinarios para listar el timpo de su contraseña.

Las otras opciones que se aplican al comando `change` son:
- **`-d`**: establece el último cambio de contraseña para una cuenta de usuario.
- **`-E`**: establece la fecha de caducidad de una cuenta de usuario.
- **`-I`**: establece el número de días de incatividad después de que una contraseña expire, durante los cuales el usuario deberá actualizaral (de lo contrario la cuenta será desactivada).
- **`-m`**: establece la duración mínima de la contraseña para una cuenta de usuario.
- **`-M`**: establece la duración máxima de la contrasela para una cuenta de usuario.
- **`-W`**: establece el número de días de advertencia antes que la contraseña expire, durante los cuales se le advierte al usuario que deberá cambiarla.

## Automatizar tareas administradivas del sistema mediante la programación de trabajos
#### Programar trabajos con Cron
En sistemas Linux `cron` es un demonio que se ejecuta continuamente y se activa a cada minuto
para comprobar un conjunto de tablas en busca de tareas a ejecutar. Estas tablas se conocen
como *crontabs* y contienen las llamadas cron jobs. Cron es adecuado para servidores y
sistemas que están encendidos constantemente, porque cada trabajo de cron se ejecuta solo si
el sistema se está ejecutando a la hora programada. Puede ser usado por usuarios ordinarios,
cada uno de los cuales tiene su propio `crontab`, así como el usuario root que gestiona los
crontabs del sistema.

#### Crontab de usuario
Los crontabs de usuario son archivos de texto que gestionan la programación de los trabajos
cron definidos por el usuario. Siempre tiene el nombre de la cuente del usuario que lo creó,
pero la ubicación de estos archivos depende de la distribución utilizada (generlamente un
subdirectorio de `/var/spoll/cron`).

Cada línea en un crontab de usuario contiene seis campos separados por un espacio:
- El minuto de la hora (0-59)
- La hora del día (0-23)
- El día del mes (1-31)
- El mes del año (1-12)
- El día de la semana (0-7 con domingo=0 o domingo=7)
- La orden a ejecutar

Para el mes del año y el día de la semana puede usar las tres primeras letras del nombre en
lugar del número correspondiente.

Los primeros cinco campos indican cuándo ejecutar el comando que se especifica en el sexto
campo, y puede conteneruno o más valores. En particular, se pueden especificar múltiples
valores utilizando:

- **`*` (asterisco)**:  se refiere a cualquier valor
- **`,` (coma)**: especifica una lista de posibles valores
- **`-` (guión)**: especifica un rango de valores posibles
- **`/`**: especifica valores escalonados

Muchas de las distribuciones incluyen el archivo `/etc/crontab` que puede ser usado como referencia
para la disposición de un archivo `cron`. A continuación se muestra un ejemplo de archivo
`/etc/crontab` de una instalación de Debian:
```sh
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
```

#### Crontabs del sistema
Los crontabs del sistema son archivos de texto que gestionan la programación de los trabajos del
cron del sistema y solo pueden ser editados por el usuario root. El archivo `/etc/crontab`
y todos los que se encuentran del dirctorio `/etc/cron.d` son crontabs del sistema.

La mayoría de las distribuciones también incluyen los directorios `/etc/cron.hourly`,
`/etc/cron.daily`, `/etc/cron.weekly` y `/etc/cron.monthly` que contiene scripts para ser ejecutados
con la frecuencia apropiada. Por ejemplo, si quiere ejecutar un script diariamente puede
colocarlo en `/etc/cron.daily`.

La sintaxis de los crontabs del sistema es similar a la de los crontabs de los usuarios,
sin embargo, también requiere un campo obligatorio adicional que especifica qué usuario
ejecutará el trabajo de cron. Por lo tanto, cada línea de un crontab de sistema contiene
siete campos separados por un espacio:

- El minuto de la hora (0-59)
- La hora del día (0-23)
- El día del mes (1-31)
- El mes del año (1-12)
- El día de la semana (0-7 con domingo=0 o domingo=7)
- El nombre de la cuenta de usuario que se utilizará al ejecutar el comando.
- La orden a ejecutar

En cuanto a los crontabs de usuario, puede especificar múltiples valores para los campos de
tiempo usando los operadores `*, , , -` y `/`. También puede indicar el mes y el día de la semana
con las tres primeras letras del nombre en lugar del número correspondiente.

#### Especificaciones de tiempo particulares
Al editar los archivos crontab, también puede usar atajos especiales en las primeras cinco
columnas en lugar de las especificaciones de tiempo:

- **`@reboot`**: ejecutara la tarea especificada una vez después de reiniciar.
- **`@hourly`**: ejecutara la tarea especificada una vez por hora al iniciar.
- **`@daily (0 @midnight)`**: ejecutara la tarea especificada una vez al día a medianoche.
- **`@weekly`**: ejecutara la tarea especificada una vez a la semana a medianoche del domingo.
- **`@monthly`**: ejecutara la tarea especificada una vez a la medianoche del primer día del mes.
- **`@yearly (o @annualy)`**: ejecutara la tarea especificada una vez al año a medianoche del 1 de enero.

#### Variables de crontab
En ocasiones, dentro de un archivo crontab, hay variables definidas antes de que se declaren
las tareas programas. Las variables de entorno establecida (comúnmente) son:

**`HOME`**: el directorio donde `cron` invoca los comandos (por defecto el directorio principal
del usuario).

**`MAILTO`**: el nombre del usuario o la dirección a la que se envía la salida estándar y el
error (por defecto, el propietario del crontab). También se permiten múltiples valores
separados por comas y un valor vacío indica que no se debe enviar ningún correo.

**`PATH`**: la ubicación de los comandos en los sistemas de archivos.

**`SHELL`**: el shell a usar (por defecto `/bin/sh`).

#### Crear trabajos en un cron de usuario
El comando `crontab` se usa para mantener los archivos `crontab` para usuarios individuales. En
particular, puede ejecutar el comando `crontab -e` para editar su propio archivo crontab o para
crear uno si aún no existe.

Por defecto, el comando `crontab` abre el editor especificado por las variables de entorno
`VISUAL` o `EDITOR` para que puede empezar a editar su archivo `crontab`. Algunas distribuciones
le permiten elegir un editor de una lista cuando `crontab` se ejecuta por primera vez.

Si quiere ejecutar rl script `foo.sh` ubicado en su directorio principal todos los días a las
10:00 am, puede agregar la siguiente línea a su archivo crontab:

    0 10 * * * /home/bender/foo.sh

Ejemplos adicionales:
```sh
0,15,30,45 08 * * 2 /home/frank/bar.sh
30 20 1-15 1,6 1-5 /home/frank/foobar.sh
```
En la primera línea, el script `bar.sh` se ejecuta todos los martes a las 08:00 am, 08:15 am,
08:30 am y a las 08:45 am. En la segunda línea el script `foobar.sh` se ejecuta a las 08:30 pm
de lunes a viernes durante los primeros quince días de enero y julio.

Además de la opción `-e`, el comando `crontab` tiene otras opciones útiles:
- **`-l`**: muestra el crontab actual en la salida estándar
- **`-r`**: quita el crontab actual
- **`-u`**: especifica el nombre del usuario cuyo crontab necesita ser modificado. Esta opción requiere privilegios de root y permite que el usuario root edite los archivos crontab de otro usuario.

#### Crear crones de sistema
A diferencia de los crontabs de usuario, los crontabs de sistema se actualizan usando un
editor: por lo tanto, no es necesario ejecutar el comadno `crontab` para editar `/etc/crontab` y
los archivos en `/etc/cron.d/`. Recuerde que cuando edite los crontabs del sistema, debe
especificar la cuenta que se usará para ejecutar el trabajo cron (normalmente root).

Por ejemplo, si quiere ejecutar el script `barfoo.sh` ubicado en el directorio `/root` todos los
días a la 01:30 am, puede abrir `/etc/crontab` con un editor y agregar la siguiente línea:

    30 01 * * * root /root/barfoo.sh >>/root/output.log 2>>/root/error.log

En el ejemplo anterior, la salida del `job` se añade a `/root/output.log`, mientras que los
errores se añaden a `/root/error.log`.

#### Configurar el acceso a la programación de tareas
En Linux los archivos `/etc/cron.allow` y `/etc/cron.deny` se usan para establecer las restricciones
`crontab`. En particular, se usan para permitir o no la programación de trabajos cron para
diferentes usuarios. Si existe el archivo `/etc/cron.allow`, solo los usuarios no root listados
dentro de él pueden programar trabajos cron usando el comando `crontab`. Si `/etc/cron.allow`
no existe pero `/etc/cron.deny` existe, solo los archivos no root listatos dentro de este
archivo no pueden programar trabajos cron usando el comando `crontab` (en este caso un
`/etc/cron.deny` vacío significa que a cada usuario se le permite programar trabajos con `crontab`).
Si no existe ninguno de los archivos, el acceso del usuario a la programación de trabajos
cron dependerá de la distribución utilizada.

> Los archivo `/etc/cron.allow` y `/etc/cron.deny` contienen una lista de nombre de usuario, cada uno en una línea separada.

#### Una alternativa a cron
Usando systemd como el administrador del sistema y del servicio, puede establecer *timers*
como una alternativa a `cron` para programar sus tareas. Los temporizadores son archivos de
unidad systemd indentificados por el sufijo `.timer`, y para cada uno de ellos debe haber un 
archivo de unidad correspondiente que describa la unidad que se activará cuando el
temporizados transcurra. Por defecto, un `timer` activa un servicio con el mismo nombre,
excepto por el sufijo.

Un temporizador incluye una sección de `[Timer]` que especifica cuándo deben ejecutarse los
trabajos programados. Específicamente, puede usar la opción `OnCalendar=` para definir
temporizadores en tiempo real que funcionan de la misma manera que los trabajos cron (están
basados en expresiones de eventos de calendario). La opción `OnCalendar=` requiere la siguiente
sintaxis:

    DayOfWeek Year-Month-Day Hour:Minute:Second

Con el `DayOfWeek` siendo opcional. Los operadores `*, /` y `,` tienen el mismo significado que los
usados por los trabajos de corn, mientras que puede usar `...` entre dos valores para indicar
un rango contiguo. Para la especificación `DayOfWeek`, puede usar las tres primeras letras del
nombre o el nombre completo.

por ejemplo, si quiere ejecutar el servicio llamado `/etc/systemd/systemd/foobar.service` a
las 05:30 del primer lunes de cada mez, puede añadir las siguientes líneas en el archivo
correspondiente de la unidad `/etc/systemd/system/foobar.time`:
```sh
[Unit]
Description=Run the foobar service

[Timer]
OnCalendar=Mon *-*-1..7 05:30:00
Persistent=true

[Install]
WantedBy=timers.target
```
Una vez que haya creado el nuevo temporizados, puede activarlo e iniciarlo ejecutando los
siguientes comandos como root:
```sh
systemctl enable foobar.timer
systemctl start foobar.timer
```
Puede cambiar la frecuencia de su trabajo programado, modificando el valor `OnCalendar` y
luego escribiendo el comando `systemctl daemon-reload`.

Finalmente, si quiere ver la lista de temporizadores activos ordenados por el tiempo que
transcurre a continuación, puede usar el comando `systemctl list-timers`. Puede añadir la opción
`--all` para ver también las unidades de temporizadores inactivos.

En lugar de las formas mencionadas anteriormente, se pueden utilizar algunas exoresiones
especiales que describen frecuencias particulares para la ejecución del trabajo:
- **`hourly`**: ejecutar la tarea especificada una vez por hora al comienzo de la hora.
- **`daily`**: ejecutara la tarea especificada una vez al día a medianoche.
- **`weekly`**: ejecutara la tarea especificada una vez a la semana a medianoche del lunes.
- **`monthly`**: ejecutara la tarea especificada una vez al mes a la medianoche del primer día del mes.
- **`yearly`**: ejecutara la tarea especificada una vez al año a medianoche del primer día de enero.

#### Programar tareas con `at`
el comando `at` se utiliza para la programación de tareas una única vez y solo requiere que se
especifique cuándo se debe ejecutar una tarea en el futuro. Después de introducir `at` en la
línea de comandos, seguido de las especificación de tiempo, entrará en la línea de comandos
`at` donde puede definir los comandos a ejecutar. Puede salir del prompt con la secuencia de
teclas `Ctrl + D`.
```sh
at now +5 minutes
warning: commands will be executed using /bin/sh
at> date
at> Ctrl+D
job 12 at Sat Sep 14 09:15:00 2019
```
El comando anterior simplemente ejecuta el comando `date` después de cinco minutos. Similar a
`cron`, la salida estándar y el error se envía por correo electrónico. Tenga en cuenta que el
demonio `atd` tendrá que estar ejecutándose en el sistema para que puede usar la programación
de `at`.

Las opciones mpas importantes que se aplican al comando `at` son:
- **`-c`**: imprime los comandos de una tarea específica (por medio del ID) a la salida estándar.
- **`-d`**: borra las tareas basdas en su ID. Es una alias para `atrm`.
- **`-f`**: lee las tareas desde un archivo en lugar de la entrada estándar.
- **`-l`**: lista las tareas pendientes del usuario. Si el usuario es root, se listan todas las tareas de todos los usuarios. Es un alias para `atq`.
- **`-m`**: envía un correo al usuario al final de la tarea aunque no haya mostrado la salida.
- **`-q`**: especifica una cola en forma de una sola letra de `a` a `z` y de `A` a `Z` (por defecto `a` para `at` y `b` para `batch`). Las tareas en las colas con las letras más altas se ejecutan con mayor prioridad. los trabajos enviados a una cola con mayúsculas son tratados como tareas `batch`.
- **`-v`**: muestra el tiempo en el que la tarea se ejecutará antes de leerla.

## Localización e internacionalización
Todas las principales distribuciones de Linux pueden ser configuradas para utilizar ajustes
de localización personalizados. Estos ajustes incluyen definiciones relacionadas con la
región y el idioma, como la zona horaria, el idioma de la interfaz, así como la codificación
de caracteres que pueden ser modificados durante la instalación del sistema operativo o en
cualquier momento posterior.

Las aplicaciones se basan en variables de entorno, archivos de configuración del sistema y
comando para seleccionar la hora y el idioma adecuados; por lo tanto, la mayoría de las
distribuciones comparte una forma estandarizada de ajustar la hora y los ajustes de
localización. Estos ajustes son importantes no solo para mejorar la experiencia del usuario,
sino también para asegurar que la hora de los eventos importantes del sistema se calculen
correctamente, por ejemplo, informar sobre temas relaciondos con la seguridad.

Para poder representar cualqueier texto escrito, independientemente del idioma hablado, los
sistemas operativos modernos necesitan una referencia estándar de codificación de caracteres,
y lo sistemas Linux no son la excepción. Como las computadoras solo pueden tratar con
números, un carácter de texto no es mpas que un número asociado a un símbolo gráfico.
Distintas plataformas informáticas pueden asocia valores numéricos distintos al mismo cáracter,
por lo que se necesita una norma de codificación de caracteres común para que sean
compatibles. Un documento de texto creado en un sistema solo será legible en otro sistema
si ambos coinciden en el formato de codificación y en qué número se asocia a que carácter,
o al menos si saben cḿo convertirlo entre las dos normas.

La naturaleza heterogénea de los ajustes de localización en los sistemas basados en Linux da
lugar a sutiles diferencias entre las distribuciones. A pesar de esas diferencias, todas las
distribuciones comparten las mismas herramientas y concpetos básicos para configurar los
conceptos de internacionalización de un sistema.

#### Zonas horarias
Las zonas horarias son bandas discretas de la superficie de la Tierra que abarcan el
equivalente a una hora, es decir, regiones del mundo que experimentan la misma hora del día
en un momento dado. Como no hay una sola longitud que pueda considerarse como el comienzo
del día para todo el mundo, las zonas horarias son relativas al meridiano principal, donde
el ángulo de la longitud de la Tierra se define como 0. La hora del meridiano principal se
denomina Hora Univarsal Coordinada, por convención abreviada como UTC. Por razones practicas,
los usos horarios no siguen la distancia longitudinal exacta del punto de referencia
(el meridiano principal). En su lugar, los usos horarios se adaptan aritificialmente para
seguir las fronteas de los países u otras subdivisiones importantes.

Las subdivisiones políticas son tan relevantes que las zonar horarias recibe el nombre
de algún agente geográfico importante de esa zona en particular, normalmente basado en el
nombre de un gran país o ciudad dentro de la zona. Sin embargo, los husos horarios se dividen
según su desfase horario en relación con el UTC y este desfase también puede utilizarse
para indicar la zona en cuestion. La zona horaria GMT-5, por ejemplo, indica una región cuya
hora UTC está cinco horas adelantada, es decir, esa región está cinco horas atrasada
respecto de la UTC. Asimismo, el huso horario GTM+3 indica una región para la cual la
hora UTC estpa tres horas por detrás. El término GMT (Greenwich Mean Time) se utiliza
como sinónimo de UTC en los nombre de las zonas basadas en la compensación.

Se puede acceder a una máquina conecada desde diferentes partes del mundo, por lo que es una
buena práctica ajustar el reloj del hardware a UTC (la zona horaria GMT+0) y dejar la
elección de la zona horaria a cada caso particular. Los servicios en la nibe, por ejemplo,
se configuran comúnmente para usar UTC, ya que puede ayudar a mitigar las inconsistencias
ocasionales entre la hora local y la hora de los clientes o en otros servidores. Por el
contrario, los usuarios que abren una sesión remota en el servidor pueden querer utilizar
su zona horaria local. Por lo tanto, dependerá del sistema operativo establecer la zona
horaria correcta según cada caso.

Además de la fecha y la hora actual, el comando `date` también imprimirá la zona horaria
actualmente configurada:

    date

El desplazamiento relativo al UTC viene dado por el valor `-03`, lo que significa que la hora
monstrada tiene 3 horas de retraso con respecto a UTC. Por lo tanto, la hora UTC está tres
horas por delante, haciendo que GMT-3 sea el huso horario correspondiente a la hora dada.
El comando `timedatectl`, que está disponible en las distribuciones que utilizan systemd,
muestras más detalles sobre la hora y la fecha del sistema:
```sh
timedatectl
               Local time: mar 2023-05-02 01:38:08 CST
           Universal time: mar 2023-05-02 07:38:08 UTC
                 RTC time: mar 2023-05-02 07:38:08
                Time zone: America/Mexico_City (CST, -0600)
System clock synchronized: yes
              NTP service: active
          RTC in local TZ: no
```
Como se muestra en la entrada `Time zone`, los nombres de las zonas horarias basadas en
localidades (como `America/Mexico_City`) también se aceptan. La zona horaria por defecto del
sistema se mantiene en el archivo `/etc/timezone`, ya sea por el nombre descriptivo completo
de la zona o por la diferencia de horas. Los nombres genéricos de la zona horaria dados por
la diferencia de horas UTC deben incluir `Etc` como la primera parte del nombre. Así que para
fijar la zona horaria por defecto en GMT+3, el nombre de la zona horaria debe ser `Etc/GMT+3`.

Aunque los nombre de las zonas horarias basadas en las localidades no requieren el
desplazamiento de la hora para funcionar, no es tan sencillo elegirlos. La misma zona puede
tener más de un nombre, lo que puede ser difícil de recordad. Para facilitar esto, el
comando `tzselect` ofrece un método interactivo que guiará al usuario hacia la definición
correcta de la zonra horaria. El comando `tzselect` debería estar disponible por defecto en
todas las distribuciones de Linux, ya que lo proporciona el paquete que contiene los programas
de utilidades necesarios relacionados con la Biblioteca C de GNU.

El comando `tzselect` será útil, por ejemplo, para un usuario que quiera identidicar la zona
horaria de "Sao Paulo City" en "Brazil". El comando `tzselect` comienza preguntador por la
macro región de la ubicación deseada:
```sh
tzselect
Please identify a location so that time zone rules can be set correctly.
Please select a continent, ocean, "coord", or "TZ".
    1) Africa
    2) Americas
    3) Antarctica
    4) Asia
    5) Atlantic Ocean
    6) Australia
    7) Europe
    8) Indian Ocean
    9) Pacific Ocean
    10) coord - I want to use geographical coordinates.
    11) TZ - I want to specify the time zone using the Posix TZ format.
#? 2
```
La opción `2` es para las regiones de América (Norte Y Sur), no necesariamente en la misma zona
horaria. También es posible especificar el huso horario con coordenadas geográficas o con
la notación de desplazamiento, también conocido como el formato Posix TZ format. El siguiente
paso es elegir el país:
```sh
Please select a country whose clocks agree with yours.
    1) Anguilla             19) Dominican Republic      37) Peru
    2) Antigua & Barbuda    20) Ecuador                 38) Puerto Rico
    3) Argentina            21) El Salvador             39) St Barthelemy
    4) Aruba                22) French Guiana           40) St Kitts & Nevis
    5) Bahamas              23) Greenland               41) St Lucia
    6) Barbados             24) Grenada                 42) St Maarten (Dutch)
    7) Belize               25) Guadeloupe              43) St Martin (French)
    8) Bolivia              26) Guatemala               44) St Pierre & Miquelon
    9) Brazil               27) Guyana                  45) St Vincent
    10) Canada              28) Haiti                   46) Suriname
    11) Caribbean NL        29) Honduras                47) Trinidad & Tobago
    12) Cayman Islands      30) Jamaica                 48) Turks & Caicos Is
    13) Chile               31) Martinique              49) United States
    14) Colombia            32) Mexico                  50) Uruguay
    15) Costa Rica          33) Montserrat              51) Venezuela
    16) Cuba                34) Nicaragua               52) Virgin Islands (UK)
    17) Curaçao             35) Panama
    53) Virgin Islands (US)
    18) Dominica            36) Paraguay
#? 9
```
El territorio de Brasil abarca cuatro zonas horarias, por lo que la información del país por
sí sola no es suficiente para establecer la zona horaria. En el siguiente paso, el comando
`tzselect` requerirá que el usuario especifique la región local:
```sh
Please select one of the following time zone regions.
    1) Atlantic islands
    2) Pará (east); Amapá
    3) Brazil (northeast: MA, PI, CE, RN, PB)
    4) Pernambuco
    5) Tocantins
    6) Alagoas, Sergipe
    7) Bahia
    8) Brazil (southeast: GO, DF, MG, ES, RJ, SP, PR, SC, RS)
    9) Mato Grosso do Sul
    10) Mato Grosso
    11) Pará (west)
    12) Rondônia
    13) Roraima
    14) Amazonas (east)
    15) Amazonas (west)
    16) Acre
#? 8
```
No están disponibles todos los nombres de las localidades, pero elegir la región más cercana
será suficiente. La información dada será utilizada por `tzselect` para mostrar la zona horaria
correspondiente:
```sh
Se ha dado la siguiente información:
        Brazil
        Brazil (southeast: GO, DF, MG, ES, RJ, SP, PR, SC, RS)
Therefore TZ='America/Sao_Paulo' will be used.
Selected time is now: sex out 18 18:47:07 -03 2019.
Universal Time is now: sex out 18 21:47:07 UTC 2019.
Is the above information OK?
    1) Yes
    2) No
#? 1
You can make this change permanent for yourself by appending the line
    TZ='America/Sao_Paulo'; export TZ
to the file '.profile' in your home directory; then log out and log in again.
Here is that TZ value again, this time on standard output so that you
can use the /usr/bin/tzselect command in shell scripts:
America/Sao_Paulo
```
El nombre de la zona horaria resultante `America/Sao_Paulo`, también puede ser usada como el
contenido del fichero `/etc/timezone` para informar la zona horaria por defecto del sistema:

    cat /etc/timezone

Como se indica en la salida de `tzselect`, la variable de entorno `TZ` define la zona horaria de
la sesión de shell. sea cual sea la zona horaria (por defecto) del sistema. Añadiendo la
línea `TZ='America/Sao_Paulo'; export TZ` al archivo `~/.profile` hará que `America/Sao_Paulo` sea
la zona horaria para las futuras sesiones del usuario. La variable `TZ` también puede ser
modificada temporalmente durante la sesión actual, para mostrar la hora en una zona horaria
diferente:

    env TZ='Africa/Cairo' date

En el ejemplo, `env` ejecutará el comando dado en una nueva sesisón de sub-shell con las mismas
variables de entorno de la sesión actual, excepto la variable `TZ`, modificado por el
argumento `TZ='Africa/Cairo'`.

#### Horario de verano (Daylight Savin Time)
pg236
