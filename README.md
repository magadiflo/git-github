# [Git](https://git-scm.com/book/en/v2)

---

Este proyecto está enfocado netamente en trabajar con los distintos `comandos de GIT`, para eso nos apoyaremos de esta
aplicación de Spring Boot.

---

# Primeros pasos

---

## Los tres estados

Git tiene **tres estados principales** en los que pueden residir sus archivos: `modified, staged, y committed`:

- `modified`, significa que ha cambiado el archivo pero aún no lo ha enviado a su base de datos.
- `staged`, significa que ha marcado un archivo modificado en su versión actual para pasar a su próxima instantánea de
  commit.
- `committed`, significa que los datos están almacenados de forma segura en tu base de datos local.

Esto nos lleva a las **tres secciones principales de un proyecto Git**: `working directory`, `staging area` y el
`git directory`.

![01.area-trabajo-area-preparacion-directorio-git](./assets/01.area-trabajo-area-preparacion-directorio-git.png)

- El `working directory` es una verificación única de una versión del proyecto. Estos archivos se extraen de la base de
  datos comprimida en el directorio Git y se colocan en el disco para que usted pueda usarlos o modificarlos.
- El `staging area` (área de preparación) es un archivo, generalmente contenido en su directorio Git, que **almacena
  información sobre lo que se incluirá en su próximo commit**. Su nombre técnico en el lenguaje Git es **"index"**,
  pero la frase **"staging area"** (área de preparación) funciona igual de bien.
- El `git directory` es donde **Git almacena los metadatos y la base de datos de objetos para su proyecto**. Esta es la
  parte más importante de Git y es lo que se copia cuando clonas un repositorio desde otra computadora.

Si una versión particular de un archivo está en el `Git directory`, se considera en estado `committed`. Si ha sido
modificado y agregado al `staging area`(área de preparación), está en estado `staged` (preparación). Y si se cambió
desde que se desprotegió, pero no está en estado `staged` (preparado), entonces está en `modified`.

## Primeros pasos: configuración de Git por primera vez

Puede ver **todas sus configuraciones y de dónde provienen** si usamos el siguiente comando:

````bash
$ git config --list --show-origin
````

Establece nombre de usuario y correo:

````bash
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
````

Nuevamente, necesitas hacer esto solo una vez si pasas la opción `--global`, porque entonces Git siempre usará esa
información para cualquier cosa que hagas en ese sistema. Si desea anular esto con un nombre o dirección de correo
electrónico diferente para proyectos específicos, puede ejecutar el comando sin la opción --global cuando esté en ese
proyecto.

Ver configuraciones **globales**:

````bash
$ git config --global --list

core.editor="C:\Users\USUARIO\AppData\Local\Programs\Microsoft VS Code\bin\code" --wait
user.name=Martín
user.email=magadiflo@gmail.com
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
filter.lfs.required=true
filter.lfs.clean=git-lfs clean -- %f
alias.lg=log --oneline --decorate --all --graph
alias.s=status -s -b
difftool.sourcetree.cmd=''
mergetool.sourcetree.cmd=''
mergetool.sourcetree.trustexitcode=true
````

Ver **todas** las configuraciones:

````bash
$ git config --list
 
...
init.defaultbranch=main
core.editor="C:\Users\USUARIO\AppData\Local\Programs\Microsoft VS Code\bin\code" --wait
user.name=Martín
user.email=magadiflo@gmail.com
...
remote.origin.url=https://github.com/magadiflo/spring-boot-git-github.git
...
````

## Primeros pasos: obtener ayuda

Obtener **ayuda de la página web del manual** para algún comando git, por ejemplo, para el comando `config`:

````bash
$ git config --help
````

Si solo queremos un **repaso rápido de las opciones disponibles** para un comando Git, podemos usar `-h`. Por ejemplo,
nuevamente consultemos la **ayuda** pero esta véz **rápida** para el comando `config`:

````bash
$ git config -h

usage: git config [<options>]

Config file location
    --global              use global config file
    --system              use system config file
    --local               use repository config file
    --worktree            use per-worktree config file
...
````

---

# Conceptos básicos de Git

---

## Obtener un repositorio Git

**Inicialización de un repositorio en un directorio existente**: Si tiene un directorio de proyecto que actualmente no
está bajo control de versiones y desea comenzar a controlarlo con Git, primero debe ir al directorio de ese proyecto,
luego ejecutar `git init`:

````bash
M:\PROGRAMACION\DESARROLLO_GIT\documentacion_oficial\git-github-practice
$ git init

Initialized empty Git repository in M:/PROGRAMACION/DESARROLLO_GIT/documentacion_oficial/git-github-practice/.git/
````

Si desea comenzar a controlar las versiones de los archivos existentes (en lugar de un directorio vacío), probablemente
debería comenzar a rastrear esos archivos y realizar una confirmación inicial. Puedes lograrlo con algunos comandos de
git add que especifican los archivos que deseas rastrear, seguidos de una confirmación de git:

````bash
$ git add *.c
$ git add LICENSE
$ git commit -m 'Initial project version'
````

**Clonando un repositorio existente**: Si desea obtener una copia de un repositorio Git existente, por ejemplo, un
proyecto en el que le gustaría contribuir, el comando que necesita es `git clone <url>`:

````bash
$ git clone https://github.com/magadiflo/spring-boot-git-github.git
````

## Registrando cambios en el repositorio

Recuerde que cada archivo en su directorio de trabajo puede estar en uno de dos estados: `tracked o untracked` (con
seguimiento o sin seguimiento). Los archivos `tracked` son archivos que Git conoce, es decir que ya está dándole
seguimiento.

![02.ciclo-vida-estado-archivos](./assets/02.ciclo-vida-estado-archivos.png)

### Comprobación del estado de los archivos

````
git status                    <-- Ver el estado con información completa
git status -s                 <-- Ver el estado de maner más compacta
git status --short            <-- --short es la forma completa del -s

git status -s -b              <-- Ver el estado junto a la rama
git status --short --branch   <-- --branch es la forma completa del -b
````

### Seguimiento de archivos nuevos

Para empezar a hacer **tracking** (seguimiento) de los archivos:

````
git add User.java   <-- Agrega el archivo User.java
git add *.html      <-- Agrega todos los archivos que tienen extensión .html
git add .           <-- Agrega todos los archivos
````

### Preparación de archivos modificados

Cuando un archivo ya es conocido por Git (`tracked`) y se hace una modificación se puede usar nuevamente el comando
`git add` para agregarlo al área de preparación (`staging area`).

### Ignorando archivos

Para ignorar aquellos archivos que **nos queremos que Git de seguimiento**, podemos crear un archivo llamado
`.gitignore` y agregarlo a la raíz del proyecto:

````gitignore
HELP.md
target/

### IntelliJ IDEA ###
.idea
*.iws
*.iml
*.ipr
````

**NOTA**
> En el caso simple, **un repositorio podría tener un único archivo** `.gitignore` en su **directorio raíz**, que **se
> aplica de forma recursiva a todo el repositorio**. Sin embargo, **también es posible tener archivos** `.gitignore`
> **adicionales en subdirectorios**. Las reglas de estos archivos .gitignore anidados **se aplican solo a los
> archivos del directorio donde se encuentran**.

### Visualización de cambios en Staged and Unstaged (preparados y no preparados)

Visualizar **qué cambios se han realizado**, no solo qué archivos cambiaron. Para eso usamos el comando `git diff` para
mostrar las líneas exactas agregadas y eliminadas:

````bash
$ git diff
````

Si ejecutamos el comando `git diff` veremos los **cambios que se han realizado a los archivos modificados**, pero que
aún no se han agregado al staging area:

````bash
$ git status -s -b
## main...origin/main
 M CONTRIBUTING.md
M  README.md
````

````bash
$ git diff
diff --git a/CONTRIBUTING.md b/CONTRIBUTING.md
index 8dea0e2..1c5d7a7 100644
--- a/CONTRIBUTING.md
+++ b/CONTRIBUTING.md
@@ -1,3 +1,3 @@
 # Archivo de contribución

-Archivo de Marckdown
\ No newline at end of file
+Archivo de contribución.
\ No newline at end of file
````

**NOTA 1**
> `??`, indica que es un archivo nuevo que aún no está siendo rastreado.<br>
> `A`, indica que es un nuevo archivo el que se ha agregado al staging area.<br>
> `M`, indica que es un archivo modificado.<br>

**NOTA 2**
> La columna de la **izquierda** indica el **estado del área de preparación** y la columna de la **derecha** indica el
> estado del **working directory**.
>
> En el resultado anterior observamos que el archivo `CONTRIBUTING.md` tiene una `M` en la columna derecha, lo que
> indica que el archivo está en estado de preparación (staging area), mientras que el archivo `README.md` tiene la `M`
> en la columna izquierda, lo que indica que ha sido modificado y está en el working directory.

Si queremos ver lo que está en el `staging area` y que será incluido en el próximo commit, podemos usar el mismo comando
anterior agregando `--staged`:

````bash
$ git diff --staged
diff --git a/README.md b/README.md
index 316739e..d581f1f 100644
--- a/README.md
+++ b/README.md
@@ -2,6 +2,8 @@

 ---

+**Descripción General:**
+
 Este proyecto de Spring Boot 3, está creado netamente para practicar con `Git y GitHub`. Esta práctica la estoy
 realizando usando la documentación oficial de`Git` y mi repositorio donde estoy detallando los pasos seguidos es en
 [git-github](https://github.com/magadiflo/git-github.git).
````

### Confirmar sus cambios

Para commitear los cambios del staging area y agregarlos al git directory:

````bash
$ git commit -m "Primer cambio"
````

### Eliminando archivos

Para eliminar un archivo de Git, debe eliminarlos de sus archivos rastreados (tracked) y luego hacer un commit.

### Ejemplo 1: Eliminando archivo con rm

Veamos el siguiente ejemplo donde eliminaremos el archivo `PROJECTS.md`, pero antes, usando el comando de linux `ls`
mostramos todos los archivos contenidos en el directorio actual:

````bash
$ ls
HELP.md  mvnw*  mvnw.cmd  pom.xml  PROJECTS.md  README.md  src/
````

Ahora, utilizando el comando de la terminal de linux `rm` eliminaremos el archivo `PROJECTS.md` y luego ejecutamos
el comando para ver el estado de los mismos:

````bash
$ rm PROJECTS.md

$ git status
Changes not staged for commit:
        deleted:    PROJECTS.md
````

Si observamos en el resultado anterior nos muestra el mensaje **"Changes not staged for commit"**, es decir que tenemos
**cambios que aún no están preparados para hacer commit**. Por lo tanto, agregamos los cambios al staging area y luego
vemos el estado de los archivos:

````bash
$ git add .

$ git status
Changes to be committed:
        deleted:    PROJECTS.md
````

Ahora, observamos en el resultado anterior que **los cambios están listos para ser commiteados**.

### Ejemplo 2: Eliminando archivo con git rm

Eliminar el archivo con el comando `git rm`, **no solo elimina el archivo**, sino que **lo deja preparado en el
staging area**, evitando hacer más pasos como en el caso anterior:

````bash
$ ls
HELP.md  mvnw*  mvnw.cmd  pom.xml  PROJECTS.md  README.md  src/
````

Ahora eliminamos el archivo `PROJECTS.md` y ese cambio será **colocado directamente en el staging area**:

````bash
$ git rm PROJECTS.md
rm 'PROJECTS.md'

$ git status
Changes to be committed:
        deleted:    PROJECTS.md
````

**NOTA 1**
> Como observamos, al final tanto el ejemplo 1 y 2 llegan al mismo resultado, la diferencia está en que en el ejemplo
> 2 solo se usa un comando `git rm` mientras que el ejemplo 1 se usan más comandos.

**NOTA 2**

> Si **elimino el archivo manualmente**, es decir vamos al directorio donde se encuentra el archivo, lo seleccionamos y
> lo eliminamos, estaríamos en el caso de la eliminación del `ejemplo 1`.
>
> Pero si uso `IntelliJ IDEA` y desde el IDE selecciono el archivo y presiono la tecla `del` para eliminarlo, en este
> caso el IDE aplica la eliminación del `ejemplo 2`.

### Ejemplo 3: eliminar archivo del staging area pero conservarlo en el working directory

Otra cosa útil que quizás quieras hacer es mantener el archivo en tu `working directory` pero `eliminarlo de tu
staging area`. En otras palabras, es posible que desee conservar el archivo en su disco duro, pero `que Git ya no lo
rastree`. Esto es particularmente útil si olvidó agregar algo a su archivo .gitignore y accidentalmente lo preparó
(staged). Para hacer esto, use la opción `--cached`:

````bash
$ ls
HELP.md  mvnw*  mvnw.cmd  pom.xml  PROJECTS.md  README.md  src/

$ git rm --cached PROJECTS.md
rm 'PROJECTS.md'

$ git status
Changes to be committed:
        deleted:    PROJECTS.md

Untracked files:
        PROJECTS.md
````

Luego de aplicar el comando anterior, el archivo `PROJECTS.md` aún se mantiene físicamente en el directorio actual y eso
es gracias a la bandera `--cached`. En pocas palabras, le dijimos a git que deje de rastrear al archivo `PROJECTS.md`
(eso se verá reflejado cuando hagamos commit, por el momento ese cambio solo está preparado en el staging area, por eso
el mensaje **Changes to be committed**) y lo mantenga en el directorio **como un nuevo archivo agregado sin
seguimiento (untracked)**.

### Renombrando archivos (mover archivos)

### Ejemplo 1: Renombrando archivo con mv

`mv` es un **comando de consola de linux que permite mover o renombrar** archivos y directorios.

Al igual que hicimos en el ejemplo del eliminar, en este ejemplo, **para renombrar los archivos también podemos ejecutar
una serie de pasos**:

````bash
$ ls
FILE.md  HELP.md  mvnw*  mvnw.cmd  pom.xml  PROJECTS.md  README.md  src/

$ mv FILE.md MY_FILE.md

$ git status
Changes not staged for commit:
        deleted:    FILE.md

Untracked files:
        MY_FILE.md
        
$ git rm FILE.md
rm 'FILE.md'

$ git status
Changes to be committed:
        deleted:    FILE.md

Untracked files:
        MY_FILE.md
        
$ git add MY_FILE.md

$ git status
Changes to be committed:
        renamed:    FILE.md -> MY_FILE.md
````

El ejemplo anterior muestra paso a paso el proceso de renombrado, pero en resumen estos serían los comandos ejecutados:

````bash
$ mv FILE.md MY_FILE.md
$ git rm FILE.md
$ git add MY_FILE.md
````

### Ejemplo 2: Renombrando archivo con git mv

Si desea **cambiar el nombre de un archivo en Git**, puede ejecutar algo como `git mv file_from file_to`.

A continuación, **renombraremos** el archivo `FILE.md` a `MY_FILE.md`, previamente listamos los archivos del directorio:

````bash
$ ls
FILE.md  HELP.md  mvnw*  mvnw.cmd  pom.xml  PROJECTS.md  README.md  src/

$ git mv FILE.md MY_FILE.md

$ git status
Changes to be committed:
        renamed:    FILE.md -> MY_FILE.md
````

Como observamos el archivo fue renombrado `(renamed)` y está en el estado de `staged`.

**NOTA**

> Git se da cuenta de que se trata de un cambio de nombre implícito, por lo que no importa si cambia el nombre de un
> archivo de esa manera o con el comando mv.
>
> **La única diferencia real es que git mv es un comando en lugar de tres**: es una función de conveniencia. Más
> importante aún, **puede utilizar cualquier herramienta que desee para cambiar el nombre de un archivo** y abordar el
> add/rm más tarde, antes de confirmar.

## Visualización del historial de commits

### Viendo el historial de commits

Ver el **historial de commits**:

````bash
$ git log

commit 843428eb126d62da12ad7fda6c44dc12112bb8df
Author: Martín <magadiflo@gmail.com>
Date:   Fri Sep 15 13:24:27 2023 -0500

    Agregando archvo PROJECTS.md

commit 104cb823b8b2bd8da1b1cc377a0067e559bf10fc
Author: Martín <magadiflo@gmail.com>
Date:   Fri Sep 15 10:39:57 2023 -0500

    Inicio
````

Existe una gran cantidad y variedad de opciones para el comando `git log` están disponibles para mostrarte exactamente
lo que estás buscando. Aquí te mostraremos algunos de los más populares.

El uso de`-p` o `--patch` muestra la **diferencia (salida del parche) introducida en cada commit**. También puede
limitar la cantidad de entradas de registro que se muestran, como usar `-2` para **mostrar solo las dos últimas
entradas:**

````bash
$ git log -p -2
commit de7978cff269dd1f52b71692debf454f78d7180c
Author: Martín <magadiflo@gmail.com>
Date:   Fri Sep 15 23:13:55 2023 -0500

    Nuevos cambios en los archivos Markdown

diff --git a/FILE.md b/FILE.md
deleted file mode 100644
index e69de29..0000000
diff --git a/MY_FILE.md b/MY_FILE.md
new file mode 100644
index 0000000..b9fc54e
--- /dev/null
+++ b/MY_FILE.md
@@ -0,0 +1 @@
+# Mi nuevo archivo
\ No newline at end of file
diff --git a/README.md b/README.md
index 5873f96..021c543 100644
--- a/README.md
+++ b/README.md
@@ -2,8 +2,10 @@

 ---

-Este proyecto de Spring Boot 3, está creado netamente para practicar con `Git y GitHub`. Esta práctica la estoy
+Este proyecto está creado netamente para practicar con `Git y GitHub`. Esta práctica la estoy
 realizando usando la documentación oficial de`Git` y el repositorio donde estoy detallando los pasos seguidos es en
 [git-github](https://github.com/magadiflo/git-github.git)

----
\ No newline at end of file
+---
+
+Historial de Commits
\ No newline at end of file

commit a1de54eed0176e6ff32b2804d5ed1a294a8be31b (origin/main)
Author: Martín <magadiflo@gmail.com>
Date:   Fri Sep 15 20:03:44 2023 -0500

    Archivo renombrado

diff --git a/MY_FILE.md b/FILE.md
similarity index 100%
rename from MY_FILE.md
rename to FILE.md
````

Ver algunas **estadísticas abreviadas para cada commit**, puede usar la opción `--stat`:

````bash
$ git log --stat -2
commit de7978cff269dd1f52b71692debf454f78d7180c (HEAD -> main)
Author: Martín <magadiflo@gmail.com>
Date:   Fri Sep 15 23:13:55 2023 -0500

    Nuevos cambios en los archivos Markdown

 FILE.md    | 0
 MY_FILE.md | 1 +
 README.md  | 6 ++++--
 3 files changed, 5 insertions(+), 2 deletions(-)

commit a1de54eed0176e6ff32b2804d5ed1a294a8be31b (origin/main)
Author: Martín <magadiflo@gmail.com>
Date:   Fri Sep 15 20:03:44 2023 -0500

    Archivo renombrado

 MY_FILE.md => FILE.md | 0
 1 file changed, 0 insertions(+), 0 deletions(-)
````

Como puede ver, la opción `--stat` imprime debajo de cada entrada de confirmación una lista de archivos modificados,
cuántos archivos se cambiaron y cuántas líneas en esos archivos se agregaron y eliminaron. También pone un resumen de la
información al final.

Mostrar la **lista de los archivos modificados** después de la información del commit:

````bash
$ git log --name-only -2
commit de7978cff269dd1f52b71692debf454f78d7180c (HEAD -> main)
Author: Martín <magadiflo@gmail.com>
Date:   Fri Sep 15 23:13:55 2023 -0500

    Nuevos cambios en los archivos Markdown

FILE.md
MY_FILE.md
README.md

commit a1de54eed0176e6ff32b2804d5ed1a294a8be31b (origin/main)
Author: Martín <magadiflo@gmail.com>
Date:   Fri Sep 15 20:03:44 2023 -0500

    Archivo renombrado

FILE.md
````

Ver cada **commit en una sola línea** usando `--pretty=oneline`:

````bash
$ git log --pretty=oneline
de7978cff269dd1f52b71692debf454f78d7180c Nuevos cambios en los archivos Markdown
a1de54eed0176e6ff32b2804d5ed1a294a8be31b Archivo renombrado
1449f19a78991a1bf871821ab0be50fd8cb2c68b Archivo renombrado
b736e0ae3ed627d1772c5fc9bbeedf92e41f9efa Nuevo archivo FILE.md
843428eb126d62da12ad7fda6c44dc12112bb8df Agregando archvo PROJECTS.md
104cb823b8b2bd8da1b1cc377a0067e559bf10fc Inicio
````

Ver cada **commit en una sola línea** usando `--pretty=oneline --abbrev-commit` o que es lo mismo, su equivalente
`--oneline`:

````bash
$ git log --oneline
de7978c (HEAD -> main, origin/main) Nuevos cambios en los archivos Markdown
a1de54e Archivo renombrado
1449f19 Archivo renombrado
b736e0a Nuevo archivo FILE.md
843428e Agregando archvo PROJECTS.md
104cb82 Inicio
````

Especificar una **salida de formato personalizado:**

````bash
$ git log --pretty=format:"%h - %an, %ar : %s"
de7978c - Martín, 19 minutes ago : Nuevos cambios en los archivos Markdown
a1de54e - Martín, 4 hours ago : Archivo renombrado
1449f19 - Martín, 4 hours ago : Archivo renombrado
b736e0a - Martín, 4 hours ago : Nuevo archivo FILE.md
843428e - Martín, 10 hours ago : Agregando archvo PROJECTS.md
104cb82 - Martín, 13 hours ago : Inicio
````

Los valores de las opciones `oneline y format` son particularmente útiles con otra opción de registro llamada `--graph`.
Esta opción agrega un pequeño y agradable gráfico ASCII que muestra su historial de branch y merge:

````bash
$ git log --pretty=format:"%h %s" --graph
* de7978c Nuevos cambios en los archivos Markdown
* a1de54e Archivo renombrado
* 1449f19 Archivo renombrado
* b736e0a Nuevo archivo FILE.md
* 843428e Agregando archvo PROJECTS.md
* 104cb82 Inicio
````

Ver **historial de commits** sin mostrar los **merge commits**, esto es simplemente para evitar que la visualización de
commits de fusión abarrote su historial de logs:

````bash
$ git log --oneline --no-merges
bc616b3 (HEAD -> feature/git-basics) Registrando cambios en el repositorio
46b9424 Conceptos básicos: obtener un repositorio Git
60e5b0e (origin/feature/getting-started, feature/getting-started) Primeros pasos: obtener ayuda
5675752 Primeros pasos: configuración de git por primera vez
17237cc Inicio
````

### Limitar la salida del Log

Podemos limitar la salida a un subconjunto de commits. Ya has visto una de esas opciones: `la opción -2`, que **muestra
solo las dos últimas confirmaciones**. De hecho, puedes hacer `-<n>`, **donde n es cualquier número entero** para
mostrar las últimas n confirmaciones.

Ver la **lista de commits** realizadas en las últimas dos semanas:

````bash
$ git log --since=2.weeks
commit de7978cff269dd1f52b71692debf454f78d7180c (HEAD -> main, origin/main)
Author: Martín <magadiflo@gmail.com>
Date:   Fri Sep 15 23:13:55 2023 -0500

    Nuevos cambios en los archivos Markdown
....
````