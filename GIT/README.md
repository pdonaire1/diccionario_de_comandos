# git_comandos
Lista de comandos mas utilizados para la utilización de repositorios git en proyectos.

Iniciar un repositorio
----------------------
Para esto se utiliza simplemente el comando 
```git init ```

Agregar repositorio
-------------------
Para agregar un repositorio externo y trabajar de forma remota o por equipos de trabajo se añade a la configuración
del .git la dirección del repositorio remoto con el siguiente comando.
```
git remote add origin https://github.com/user/repo.git
```

Operaciones sobre Archivos
--------------------------
Adiciona un archivo o un directorio de manera recursiva
```git add path```

Preparar todos los archivos para el siguiente commit
```git add . ```

Remueve un archivo o directorio del árbol de trabajo
```git rm ruta```
      -f : Fuerza la eliminación de un archivo del repositorio

Mueve el archivo o directorio a una nueva ruta
``` git mv origen destino```
      -f : Sobre-escribe los archivos existentes en la ruta destino

Recupera un archivo desde la rama o revisión actual
```git checkout [rev] archivo```
      -f : Sobre-escribe los cambios locales no guardados

Añadir un commit
----------------
Para ello se coloca la linea de comando
``` git commit ```

Saldrá un archivo en el cual se coloca la descripción del commit que se desea añadir.
A su vez se puede hacer en una sola linea de comando de la siguiente manera:
``` git commit -a -m "Descripción del commit"  ```

Modificar un commit
-------------------
Si ocurre un error al escribir la descripción del commit se puede modificar utilizando el siguiente comando.
```
git commit --amend
```
--------------------------------------------------------------------------------
Trabajando sobre el código
--------------------------
```git status```
    Imprime un reporte del estado actual del árbol de trabajo local
```git diff [ruta]```
    Muestra la diferencia entre los cambios en el árbol de trabajo local
```git diff HEAD ruta```
    Muestra las diferencias entre los cambios registrados y los no registrados
```git add path```
    Selecciona el archivo para que sea incluido en el próximo commit
```git reset HEAD ruta```
    Marca el archivo para que no sea incluido en el próximo commit
```git commit```
    Realiza el commit de los archivos que han sido registrados (con git-add)
      -a : Automáticamente registra todos los archivos modificados
```git reset --soft HEAD^```
    Deshace commit & conserva los cambios en el árbol de trabajo local
```git reset --hard HEAD^```
    Restablece el árbol de trabajo local a la versión del ultimo commit
```git clean```
    Elimina archivos desconocidos del árbol de trabajo local

Examinando el histórico:

```git log [ruta]```
    Muestra el log del commit, opcionalmente de la ruta especifica
```git log [desde [..hasta]]```
    Muestra el log del commit para un rango de revisiones dado
      --stat : Lista el reporte de diferencias de cada revisión
      -S'pattern' : Busca el historial de cambios que concuerden con el patrón de búsqueda
```git blame [archivo]```
    Muestra el archivo relacionado con las modificaciones realizadas

Repositorios remotos:
```git fetch [remote]```
    Trae los cambios desde un repositorio remoto
```git pull [remote]```
    Descarga y guarda los cambios realizados desde un repositorio remoto
```git push [remote]```
    Guarda los cambios en un repositorio remoto
```git remote```
    Lista los repositorios remotos
```git remote add remote url```
    Añade un repositorio remoto a la lista de repositorios registrados

Ramas:
```git checkout rama```
    Cambia el árbol de trabajo local a la rama indicada
      -b rama : Crea la rama antes de cambiar el árbol de trabajo local a dicha rama
```git branch```
    Lista las ramas locales
```git branch -f rama rev```
    Sobre-escribe la rama existente y comienza desde la revisión
```git merge rama```
    Guarda los cambios desde la rama
Exportando e importando:
```git apply - < archivo```
    Aplica el parche desde consola (stdin)
```git format-patch desde [..hasta]```
    Formatea un parche con un mensaje de log y un reporte de diferencias (diffstat)
```git archive rev > archivo```
    Exporta resumen de la revisión (snapshot) a un archivo
      --prefix=dir/ : Anida todos los archivos del snapshot en el directorio
      --format=[tar|zip] : Especifica el formato de archivo a utilizar: tar or zip
Etiquetas:
```git tag name [revision]```
    Crea una etiqueta para la revisión referida
      -s : Firma la etiqueta con su llave privada usando GPG
      -l [patrón] : Imprime etiquetas y opcionalmente los registros que concuerden con el patrón de busqueda

Banderas de Estado de los Archivos:
M (modified) : El archivo ha sido modificado
C (copy-edit) : El archivo ha sido copiado y modificado
R (rename-edit) : El archivo ha sido renombrado y modificado
A (added) : El archivo ha sido añadido
D (deleted) : El archivo ha sido eliminado
U (unmerged) : El archivo presenta conflictos después de ser guardado en el servidor (merge)




