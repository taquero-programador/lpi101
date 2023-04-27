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
