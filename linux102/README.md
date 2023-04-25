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

Demostrémos este proceso: pg48
