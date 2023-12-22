# 📄 Respuestas Ejercicio 1

## ¿Qué comando utilizaste en el paso 11? ¿Por qué?
```
git log
git reset <id-commit>
git restore git-nuestro.md
```
Deshacer el último commit descartando los cambios realizados en nuestra working copy implica mover el puntero HEAD a un commit anterior y restaurar los archivos afectados (en este caso *git-nuestro.md*) al estado en el que se encontraban en ese momento. Para ello he empezado haciendo un `git log` para obtener la id del commit al cual necesitaba volver (en este caso nuestro commit inicial), y entonces he utilizado `git reset` seguido de la id de dicho commit + `git restore` seguido del nombre del archivo, con tal de mover el HEAD hasta ese punto y devolver el contenido de *git-nuestro.md* a su estado anterior.  

## ¿Qué comando o comandos utilizaste en el paso 12? ¿Por qué?
```
git reflog 
git show <id-incompleta-commit> 
git reset --hard <id-completa-commit>
```
La otra opción que tenemos para volver a un estado o punto determinado en nuestro grafo y nuestra working copy es utilizar un `git reset --hard` (que viene a ser la suma de los dos comandos anteriores en uno solo, `git reset` + `git restore`). En este caso nos interesaba volver justo al momento en el que habíamos hecho nuestro segundo commit, así que he hecho:  
- `git reflog` para poder recuperar el identificador del commit que habíamos hecho en el paso 10 (he utilizado este comando porque es el que recoge el historial de commits _completo_; al haber vuelto al primer commit, el segundo no está accesible mediante `git log` - pero sí podemos acceder a él con git `git reflog` ya que éste recoge absolutamente todos los commits por los que HEAD ha ido pasando)  
- Reflog muestra una id incompleta, y aunque ésta suele ser más que suficiente me he asegurado de obtener y copiar la ID completa del commit mediante el comando `git show` 
- Y finalmente he hecho un `git reset --hard` apuntando a dicho commit   

## El merge del paso 13, ¿Causó algún conflicto? ¿Por qué?
No. Git por defecto intenta hacer merge _fast-forward_ salvo que le indiquemos lo contrario, y en este caso no hay cambios contradictorios en las mismas el mismo archivo en dos ramas distintas (que es lo que causa los problemas) así que conflicto como tal no se produce.

## El merge del paso 19, ¿Causó algún conflicto? ¿Por qué?
En este caso sí, porque el merge fast-forward no es posible ya que existen cambios contradictorios entre la versión de `git-nuestro.md` que existe en la rama `htmlify` y la que existe en la rama `styled`.

## El merge del paso 21, ¿Causó algún conflicto? ¿Por qué?
No, porque puede volverse a llevar a cabo un merge _fast-forward_. Nos lo especifica git tal cual al hacerlo:
```
Fast-forward
 git-nuestro.md | 19 +++++++++----------
 1 file changed, 9 insertions(+), 10 deletions(-)
```

## ¿Qué comando o comandos utilizaste en el paso 25?
```
git log --graph
```
Opcionalmente también podría usarse `git log --graph --oneline` para una versión algo más pequeña y simplificada del diagrama

## El merge del paso 26, ¿Podría ser fast forward? ¿Por qué?
Podría serlo porque no hay cambios contradictorios. En realidad al especificar que queremos hacer un `git merge --no-ff title` tal y como se nos pide, git nos dice lo siguiente:
```
$ git merge --no-ff title
Merge made by the 'recursive' strategy.
 git-nuestro.md | 2 ++
 1 file changed, 2 insertions(+)
```
Siendo _recursive_ la estrategia de fusión predeterminada a la hora de incorporar cambios de una rama o fusionarla, esto nos indica que git ha podido hacer una fusión de ramas por defecto sin ningún problema

## ¿Qué comando o comandos utilizaste en el paso 27?
Como simplemente necesitamos volver un paso atrás, al ser el merge lo último que acabamos de hacer, podemos hacerlo así:
```
git reset HEAD~1
```

## ¿Qué comando o comandos utilizaste en el paso 28?
```
git restore git-nuestro.md
```

## ¿Qué comando o comandos utilizaste en el paso 29?
```
git branch -D title
```
Normalmente para borrar ramas es suficiente con hacer este mismo comando pero con `-d` (minúscula), pero en este caso utilizar esta opción nos da error ya que la rama `title` no está _"fully merged"_, así que tenemos que hacerlo con `-D` (que nos permite borrar ramas a pesar de eso)

## ¿Qué comando o comandos utilizaste en el paso 30?
Estando en la rama `main`, utilizamos `git reflog` y `git show` para obtener la id del commit con el título y poder regresar a él. Luego nos movemos a dicho commit, recreamos la rama y rehacemos el merge:
```
git reset --hard <id-commit>
git branch title
git merge --no-ff title
```
## ¿Qué comando o comandos usaste en el paso 32?
Generamos de nuevo el diagrama o hacemos un `git log` para acceder fácilmente el listado de commits y:
```
git reset <id-commit-inicial>
```
... ya que `reset` lo que hace es simplemente movernos (el puntero HEAD) hasta el punto que nosotros le digamos, _sin cambiar nada_

## ¿Qué comando o comandos usaste en el punto 33?
Tomando como referencia el log o diagrama generado en el paso anterior, tomamos la id del commit indicado (al que en el paso 34 le pondremos el tag `'title'`) y nos movemos a él:
```
git reset <id-commit-titulo>
```