## 6.1 ¿Qué es una rama? (15 min)

> [!note]- Nota para el instructor
> - Dibuja el grafo. Para demostrar que una rama es solo un puntero, ejecuta `cat .git/refs/heads/main` (contiene un hash) y `cat .git/HEAD`.

Una **rama (branch)** es una línea de desarrollo independiente. Técnicamente, en Git una rama es solo **un puntero móvil a un commit** (un archivo de texto que contiene un hash). Por eso crearlas es instantáneo.

```
                 C4 ── C5   ← feat/login (rama de funcionalidad)
                /
C1 ── C2 ── C3 ── C6       ← main (rama principal, lo que está en producción)
                    ↑
                   HEAD
```

> [!example] Analogías
> - Un **videojuego**: antes de enfrentar al jefe final guardas en otro slot. Si sale mal, tu partida principal sigue intacta.
> - Un **árbol**: el tronco es `main` y cada rama crece en su propia dirección sin afectar al tronco.
> - Un **borrador**: en lugar de rayar el documento oficial, haces los cambios en una copia y solo los pasas en limpio cuando están aprobados.

### ¿Para qué sirven?

- Desarrollar un *feature* (ej. autenticación con JWT) sin romper la versión estable.
- Corregir un bug en producción (*hotfix*) mientras otro *feature* sigue a medias.
- Probar un *refactor* arriesgado o actualizar una dependencia mayor sin miedo.
- Que cada desarrollador trabaje en su propio ticket.

## 6.2 Comandos de ramas (20 min)

> [!note]- Nota para el instructor
> - Error común: crear la rama con `git branch` y olvidar cambiarse; los commits terminan en `main`. Señala que el prompt de Git Bash muestra la rama actual.

### `git branch` — listar, crear, borrar

```bash
git branch                     # listar ramas locales (* = actual)
git branch -a                  # incluir ramas remotas
git branch feat/login          # crear rama (no te cambia a ella)
git branch -d feat/login       # borrar rama ya fusionada
git branch -D spike/graphql    # borrar a la fuerza (aunque no esté fusionada)
git branch -m viejo nuevo      # renombrar
```

Crear una rama es crear un puntero nuevo en el mismo commit:

```
                 main  ← HEAD
                  ↓
 C1 ── C2 ── C3
                  ↑
              feat/login        (git branch feat/login: HEAD sigue en main)
```

### `git switch` — cambiar de rama

```bash
git switch feat/login          # ir a la rama
git switch -c fix/calculo-iva  # crear y cambiarse en un paso
git switch -                   # volver a la rama anterior (como cd -)
```

`git switch` mueve HEAD a otra rama y tus archivos cambian con él:

```
 En main                                 Después de git switch feat/login

       C4  ← feat/login                        C4  ← feat/login  ← HEAD
      /                                       /
 C1 ── C2 ── C3  ← main  ← HEAD           C1 ── C2 ── C3  ← main
```

> [!note] Antes de Git 2.23 se usaba `git checkout feat/login` y `git checkout -b fix/calculo-iva`. Siguen funcionando.

> [!example] Analogía
> Es **cambiar de canal de TV**: la pantalla es la misma, pero el contenido cambia. Al hacer `switch`, Git reescribe tus archivos para que coincidan con esa rama.

> [!warning] Si tienes cambios sin guardar que entran en conflicto con la otra rama, Git no te dejará cambiar. Haz commit o `git stash` primero.

## 6.3 Fusionar: `git merge` (15 min)

> [!note]- Nota para el instructor
> - Insiste: **te paras en la rama que recibe** y luego haces `merge` de la otra. Muestra el resultado con `--graph` en ambos tipos de fusión.

**Función:** integra los cambios de otra rama **en la rama actual**.

```bash
git switch main          # 1. ve a la rama que RECIBE los cambios
git merge feat/login     # 2. trae los cambios de feat/login
```

### Tipo 1: Fast-forward (avance rápido)
Si `main` no avanzó desde que se creó la rama, Git solo **mueve el puntero**.

```
Antes:  C1 ── C2 (main)            Después:  C1 ── C2 ── C3 ── C4 (main, feat/login)
                 \
                  C3 ── C4 (feat/login)
```
*Analogía:* imprimiste un reporte de 10 páginas y tu compañero tomó una copia para agregarle las páginas 11 y 12, sin tocar las tuyas. Para tener el reporte completo no hay nada que comparar: **solo engrapas las páginas nuevas al final**.

### Tipo 2: Three-way merge (fusión de tres vías)
Si ambas ramas avanzaron, Git crea un **commit de fusión** con dos padres.

```
C1 ── C2 ── C5 ────── M   (main)
        \            /
         C3 ── C4 ──     (feat/login)
```
*Analogía:* dos personas corrigieron **capítulos distintos** de un mismo documento. Comparando ambas copias con el original (el **ancestro común**, C2), se pueden juntar las dos correcciones sin problema.

## 6.4 Conflictos (15 min)

> [!note]- Nota para el instructor
> - Provoca un conflicto en vivo a propósito. Normalízalo: un conflicto **no es un error**, es Git pidiendo una decisión humana.
> - Muestra el *merge editor* de VS Code y también cómo resolverlo a mano.

Un **conflicto** ocurre cuando dos ramas modificaron **las mismas líneas** del mismo archivo y Git no puede decidir cuál conservar.

> [!example] Analogía
> Dos editores corrigieron **la misma frase** de un libro de forma distinta. El asistente no puede adivinar cuál es la buena, así que deja ambas versiones marcadas y le pide a una persona que decida.

### Cómo se ve

```bash
git merge feat/timeout
# CONFLICT (content): Merge conflict in config.py
# Automatic merge failed; fix conflicts and then commit the result.
```

```python
<<<<<<< HEAD
DB_TIMEOUT = 30
=======
DB_TIMEOUT = 60
>>>>>>> feat/timeout
```

- Entre `<<<<<<< HEAD` y `=======`: tu versión (rama actual).
- Entre `=======` y `>>>>>>> feat/timeout`: la versión que llega.

### Cómo resolverlo (4 pasos)

1. Abre el archivo y **decide** qué queda (una, otra o una mezcla). Si no sabes, **pregúntale a quien hizo el otro cambio**.
2. **Borra los marcadores** `<<<<<<<`, `=======`, `>>>>>>>`.
3. Verifica que el código compile / los tests pasen. `git add config.py`
4. `git commit` (Git propone un mensaje de merge).

```bash
git status            # ver qué archivos están en conflicto
git merge --abort     # cancelar el merge y volver a como estaba
```

> [!tip] VS Code muestra botones: *Accept Current*, *Accept Incoming*, *Accept Both*, y un *merge editor* de 3 paneles.

> [!danger] Un error clásico: hacer commit con los marcadores `<<<<<<<` todavía en el código. El programa ni siquiera arrancará.

### Cómo evitar conflictos
- Commits y merges **frecuentes y pequeños**.
- Ramas de **vida corta** (horas o pocos días, no semanas).
- Traer los cambios de `main` a tu rama a menudo.
- Evitar *refactors* masivos (renombrar todo, reformatear todo) mezclados con *features*.
- Usar un formateador automático (Black, Prettier) para que todo el equipo tenga el mismo estilo.

## 6.5 `git rebase` (8 min)

> [!note]- Nota para el instructor
> - Dibuja en el pizarrón los tres diagramas (antes, con merge y con rebase) y señala que X' y Y' tienen hashes nuevos: de ahí sale la regla de oro.
> - Si vas justo de tiempo, quédate con el diagrama, la analogía del papel calca y la regla de oro.

`git rebase` es otra forma de integrar cambios. Toma los commits de tu rama, los quita por un momento, mueve tu rama a la punta de otra rama y **vuelve a aplicar tus commits uno por uno** encima. Es como si hubieras empezado tu trabajo hoy, sobre la versión más reciente.

### El problema que resuelve

Creaste `feat/prioridad` desde `main` e hiciste dos commits (X y Y). Mientras tanto, un compañero subió dos commits a `main` (C y D). Tu rama está **atrasada**: se basa en B y no tiene C ni D.

```
            X ── Y        ← feat/prioridad
           /
A ── B ── C ── D          ← main
```

**Con `git merge main`** (desde tu rama) se unen las dos líneas con un commit de fusión M:

```
            X ── Y ── M   ← feat/prioridad
           /         /
A ── B ── C ── D ────
```

**Con `git rebase main`** (desde tu rama) tus commits se rehacen sobre D:

```
A ── B ── C ── D ── X' ── Y'   ← feat/prioridad
               ↑
              main
```

El historial queda en **una sola línea recta**, sin commit de fusión.

### Qué hace Git paso a paso

```bash
git switch feat/prioridad
git rebase main
```

1. Busca el **ancestro común** de las dos ramas (B).
2. **Guarda aparte** los cambios de tus commits (X y Y).
3. **Mueve tu rama** a la punta de `main` (D).
4. **Vuelve a aplicar tus cambios uno por uno**: primero X, luego Y.

Por eso aparecen como **X' y Y'**: mismo contenido y mismo mensaje, pero **commits nuevos con hashes nuevos**. Los originales quedan abandonados.

> [!example] Analogía: el papel calca
> Un arquitecto te da el plano de una casa. Pones encima una hoja de **papel calca** y dibujas tus cambios: una ventana y luego una puerta. Mientras tanto, el arquitecto **actualiza el plano** y agrega una cochera.
> - **Merge** es engrapar tu hoja junto al plano nuevo con una nota: "aquí se juntan las dos versiones".
> - **Rebase** es levantar tu papel calca, ponerlo **sobre el plano nuevo** y volver a trazar tus cambios, primero la ventana y luego la puerta. Parece que siempre dibujaste sobre el plano con cochera.
>
> Si tu puerta cae justo donde ahora está la cochera, tienes que detenerte y decidir: eso es un conflicto durante el rebase.

### Conflictos durante un rebase

Como Git aplica los commits uno por uno, se detiene en el commit que choca:

```bash
git status                # ver qué archivos tienen conflicto
# resolver como en un merge: decidir y borrar los marcadores
git add config.py
git rebase --continue     # seguir con el siguiente commit
git rebase --abort        # cancelar y dejar la rama como estaba
```

En un merge los conflictos se resuelven **una sola vez**; en un rebase pueden aparecer **en cada commit**.

### La regla de oro

> [!danger] Nunca hagas rebase de commits que ya compartiste
> El rebase **crea commits nuevos y abandona los originales**. Si ya subiste X y Y y un compañero los descargó, tú tendrás X' y Y' y él X y Y. Para subir los tuyos necesitarías `git push --force`, y tu compañero vería commits duplicados y conflictos.
>
> Haz rebase solo en **tu propia rama, antes de compartirla**. En `main` y en ramas compartidas, usa merge.

### merge vs. rebase

| | `git merge` | `git rebase` |
|---|---|---|
| Historial | Muestra la bifurcación y la fusión | Queda en línea recta |
| Commit extra | Crea un commit de fusión | No crea commit de fusión |
| Commits existentes | No los toca | Los reescribe (hashes nuevos) |
| Conflictos | Se resuelven una vez | Pueden aparecer en cada commit |
| Seguro en ramas compartidas | Sí | No |
| Uso típico | Integrar una rama terminada a `main` | Poner al día tu rama con `main` antes del PR |

### Dónde se usa en la práctica

```bash
# Antes de abrir un PR: poner tu rama al día con main
git switch feat/prioridad
git fetch origin
git rebase origin/main

# Traer cambios del remoto sin crear commits de fusión
git pull --rebase
```

Después de un rebase, integrar tu rama a `main` es un **fast-forward**: `main` solo avanza hasta Y'.

## 6.6 Práctica guiada (8 min)

> [!note]- Nota para el instructor
> - Error común en la parte del conflicto: olvidar `git add` antes del `git commit`, o dejar los marcadores `<<<<<<<` en el archivo. Pide que ejecuten `python app.py` después de resolver.

Crea primero un `config.py` en `main`:

```bash
cd gestor-tareas
echo "MAX_TAREAS = 50" > config.py
git add config.py && git commit -m "chore: agrega archivo de configuración"
```

**Fast-forward:**

```bash
git switch -c feat/prioridad
```

Agrega a `tareas.py`:

```python
def tareas_prioritarias(prefijo="!"):
    return [t for t in tareas if t.startswith(prefijo)]
```

```bash
git commit -am "feat: agrega filtro de tareas prioritarias"
git switch main
git merge feat/prioridad          # fast-forward
git log --oneline --graph --all
```

**Provocar un conflicto:**

```bash
git switch -c feat/limite-alto
echo "MAX_TAREAS = 100" > config.py
git commit -am "feat: aumenta el límite de tareas a 100"

git switch main
echo "MAX_TAREAS = 20" > config.py
git commit -am "fix: reduce el límite de tareas a 20 por rendimiento"

git merge feat/limite-alto        # CONFLICTO en config.py
# → edita config.py y decide el valor final, por ejemplo: MAX_TAREAS = 50
git add config.py
git commit -m "Fusiona feat/limite-alto resolviendo el límite de tareas"
git log --oneline --graph --all
git branch -d feat/prioridad feat/limite-alto
```

---

## Guía del instructor

### Posibles preguntas de los alumnos

> [!question] ¿Cuántas ramas debería tener?
> `main` más una por cada funcionalidad o corrección en curso. Las ramas se borran después de fusionarse.

> [!question] ¿Uso merge o rebase?
> Merge conserva la historia tal como ocurrió; rebase la deja lineal. Cada equipo define su convención; para empezar, merge es más seguro.

> [!question] ¿Qué pasa con mis cambios sin commit si cambio de rama?
> Si no chocan con la otra rama, viajan contigo. Si chocan, Git no te deja cambiar: haz commit o `git stash` primero.

> [!question] ¿Puedo traer un solo commit de otra rama?
> Sí, con `git cherry-pick <hash>` (tema intermedio).

> [!question] ¿Qué significa "detached HEAD"?
> Que estás parado en un commit y no en una rama. Para volver: `git switch main`. Si quieres conservar lo que hiciste ahí: `git switch -c nueva-rama`.

> [!question] ¿Borrar una rama borra sus commits?
> Si ya estaba fusionada, no: los commits siguen en `main`. Si la borras con `-D` sin fusionar, los commits quedan huérfanos y solo se recuperan con `git reflog` durante un tiempo.

### Respuestas de las actividades

**6.6 Práctica guiada**
- El primer merge muestra `Fast-forward` en la salida: no se crea commit de fusión.
- En el conflicto, `config.py` debe quedar con **una sola línea** sin marcadores, por ejemplo `MAX_TAREAS = 50`. Cualquier valor es válido si el alumno puede justificarlo.
- `git log --oneline --graph --all` debe mostrar el commit de fusión con dos líneas que se unen.
- Comprobación: `python app.py` ejecuta sin errores.
