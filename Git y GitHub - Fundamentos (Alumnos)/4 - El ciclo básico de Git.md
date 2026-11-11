## 4.1 El modelo mental: las tres áreas

Este es **el concepto más importante del curso**.

```
 ┌────────────────────┐   git add    ┌──────────────────┐  git commit  ┌──────────────────┐
 │ Directorio de      │ ───────────> │  Staging area    │ ───────────> │  Repositorio     │
 │ trabajo            │              │  (índice)        │              │  (.git)          │
 │ (working directory)│ <─────────── │                  │              │  historial       │
 └────────────────────┘ git restore  └──────────────────┘              └──────────────────┘
```

| Área | Qué es | Analogía (mudanza) |
|------|--------|-----------------------|
| **Directorio de trabajo** | Los archivos tal como los ves y editas en tu IDE | Tu cuarto, con todas tus cosas tiradas |
| **Staging area (índice)** | Lista de cambios preparados para el próximo commit | La **caja** donde metes lo que vas a enviar |
| **Repositorio** | Historial de commits guardado en `.git` | El **camión** que se lleva la caja sellada y etiquetada |

> [!example] Otra analogía: la foto grupal
> - Directorio de trabajo = todas las personas que están en la fiesta.
> - `git add` = llamar a quienes van a salir en la foto.
> - `git commit` = tomar la foto. Queda guardada para siempre, con fecha y una descripción.

### ¿Por qué existe el staging area?

Porque te permite hacer **commits pequeños y atómicos**. Si modificaste 5 archivos pero 3 son de la nueva validación de usuarios y 2 son un fix del cálculo de impuestos, puedes hacer **dos commits separados**. Así, si el fix causa problemas, puedes revertirlo sin tocar la validación.

### Estados de un archivo

```
 Untracked ──git add──> Staged ──git commit──> Unmodified (committed)
                          ^                          │
                          │                     (lo editas)
                          └──────git add──── Modified <┘
```

- **Untracked (sin seguimiento):** Git lo ve pero no lo controla (ej. un `utils.py` recién creado).
- **Modified (modificado):** cambió desde el último commit, pero no está preparado.
- **Staged (preparado):** irá en el próximo commit.
- **Committed / Unmodified:** guardado de forma segura en el historial.

> [!tip] Es literalmente una **máquina de estados**. Cada comando es una transición.

## 4.2 Comandos del ciclo básico

### `git init` — crear un repositorio

**Función:** convierte una carpeta normal en un repositorio creando la carpeta oculta `.git`.

```bash
mkdir gestor-tareas
cd gestor-tareas
git init
# Initialized empty Git repository in .../gestor-tareas/.git/
```

Qué cambia en la carpeta:

```
 Antes                         git init        Después
 gestor-tareas/               ─────────>       gestor-tareas/
 └── README.md                                 ├── .git/        ← historial (base de datos)
                                               └── README.md
```

> [!example] Analogía
> Como **abrir una cuenta en el banco**: antes no había registro de nada; a partir de ese momento cada movimiento queda anotado.

> [!danger] Nunca borres ni edites a mano la carpeta `.git`: ahí vive todo el historial. Es la base de datos del repositorio.

---

### `git status` — ¿qué está pasando?

**Función:** muestra el estado del directorio de trabajo y del staging area.

```bash
echo "# Gestor de tareas CLI" > README.md
git status
```

```
On branch main
No commits yet
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md
```

```bash
git status -s     # versión corta
# ?? README.md    (?? = untracked, A = añadido, M = modificado)
```

Cómo leer la salida: cada sección corresponde a un área.

```
 Directorio de trabajo          Staging area                Repositorio
 ┌───────────────────────┐      ┌─────────────────────┐     ┌──────────────────┐
 │ utils.py   (nuevo)    │      │ app.py              │     │ último commit    │
 │ tareas.py  (cambiado) │      │                     │     │                  │
 └───────────────────────┘      └─────────────────────┘     └──────────────────┘
   "Untracked files"              "Changes to be              "nothing to commit"
   "Changes not staged             committed"                 (cuando las tres
    for commit"                                                áreas coinciden)

 git status -s
 ?? utils.py        sin seguimiento
  M tareas.py       modificado, sin preparar
 A  app.py          añadido al staging
```

> [!example] Analogía
> Es el **tablero del coche**: antes de arrancar te dice si falta gasolina, si una puerta está abierta o si todo está en orden. Úsalo **constantemente**.

---

### `git add` — preparar cambios

**Función:** mueve cambios del directorio de trabajo al staging area.

```bash
git add app.py             # un archivo
git add src/               # una carpeta
git add *.py               # por patrón
git add .                  # todo lo de la carpeta actual
git add -p                 # interactivo: elegir fragmentos (hunks) de un archivo
```

```
 Directorio de trabajo                        Staging area
 ┌───────────────────────┐  git add app.py   ┌─────────────────────┐
 │ app.py      (cambiado)│ ────────────────> │ app.py              │
 │ tareas.py   (cambiado)│                   │                     │
 └───────────────────────┘                   └─────────────────────┘
   tareas.py se queda sin preparar
```

> [!example] Analogía
> Meter productos al **carrito del súper**: ya los elegiste, pero todavía no pagas y puedes devolverlos al estante.

---

### `git commit` — guardar una versión

**Función:** crea una instantánea permanente con lo que hay en el staging area.

```bash
git commit -m "Agrega estructura inicial del CLI"
git commit                      # abre el editor para un mensaje largo
git commit -am "Corrige typo en mensaje de error"  # add + commit de archivos YA rastreados (no incluye nuevos)
git commit --amend -m "Nuevo mensaje"  # corrige el último commit (solo si no lo has subido)
```

Cada commit contiene: **hash SHA-1** único (ej. `a1b2c3d`), autor, fecha, mensaje, puntero al commit padre y la instantánea.

Qué pasa al hacer commit:

```
 Antes                                    Después de git commit
              HEAD                                         HEAD
               ↓                                            ↓
 A ── B  ← main                           A ── B ── C  ← main
 Staging: [ app.py ]                      C guarda la instantánea, autor, fecha,
                                          mensaje y su padre B
```

> [!example] Analogía
> **Pagar en la caja y recibir el ticket**: queda registro de qué compraste, cuándo y con un **número de folio único** (el hash). Lo que está en el ticket ya no se puede cambiar.

#### Buenas prácticas para mensajes de commit

- Usa **imperativo**: "Agrega login", no "Agregué" ni "agregando".
- Primera línea ≤ 50 caracteres; si hace falta, línea en blanco y explicación del **porqué**.
- Un commit = un cambio lógico.

| **Desventajas:** Malo | Bueno |
|--------|---------|
| `cambios` | `Agrega validación de correo en el registro` |
| `ya funciona` | `Corrige NullPointerException al consultar pedidos sin cliente` |
| `asdf` | `Elimina dependencia no utilizada de requests` |
| `fix` | `fix: usa Decimal en el cálculo de IVA para evitar errores de redondeo` |

#### Conventional Commits

Es una convención popular para escribir mensajes de commit con un formato fijo. Muchas herramientas la usan para generar changelogs y versiones automáticas.

```
tipo(alcance opcional): descripción en imperativo
```

Ejemplos: `feat(tareas): agrega prioridad`, `fix: valida título vacío`, `docs(readme): explica la instalación`.

| Tipo | Significado | Ejemplo |
|------|-------------|---------|
| `feat` | Nueva funcionalidad para el usuario | `feat: agrega comando para listar tareas` |
| `fix` | Corrección de un error | `fix: valida que el título no esté vacío` |
| `docs` | Solo documentación (README, comentarios) | `docs: explica cómo instalar el CLI` |
| `style` | Formato del código sin cambiar su comportamiento (espacios, comillas) | `style: aplica Black a tareas.py` |
| `refactor` | Reorganiza el código sin agregar funciones ni corregir errores | `refactor: separa la validación en una función` |
| `perf` | Mejora de rendimiento | `perf: evita recorrer la lista dos veces` |
| `test` | Agrega o corrige pruebas | `test: cubre eliminar_tarea con índice inválido` |
| `build` | Sistema de construcción o dependencias | `build: actualiza requests a la versión 2.32` |
| `ci` | Configuración de integración continua | `ci: ejecuta pytest en GitHub Actions` |
| `chore` | Mantenimiento que no toca el código de la aplicación | `chore: agrega .venv al .gitignore` |

Un `!` después del tipo (`feat!: ...`) indica un cambio que rompe la compatibilidad.

---

### `git log` — ver el historial

**Función:** muestra los commits.

```bash
git log
git log --oneline                     # una línea por commit
git log --oneline --graph --all       # árbol con todas las ramas
git log -3                            # últimos 3
git log --author="Ana"
git log -- tareas.py                  # historial de un archivo
git log -p                            # con los cambios de cada commit
git log --grep="fix"                  # buscar en los mensajes
```

```
a1b2c3d (HEAD -> main) feat: agrega comando para listar tareas
9f8e7d6 Agrega README inicial
```

Cómo leer `git log --oneline --graph`:

```
 * a1b2c3d (HEAD -> main) feat: lista tareas       ← más nuevo
 * 7d4e2f1 fix: valida título vacío
 * 4c2d9e0 feat: agrega módulo de tareas
 * 9f8e7d6 Agrega README inicial                   ← más antiguo
 │    │          │               │
 │    │          │               └── mensaje
 │    │          └── HEAD -> main: dónde estás ahora
 │    └── hash corto: el folio del commit
 └── la línea (--graph) une cada commit con su padre
```

> [!example] Analogía
> El **estado de cuenta del banco**: todos los movimientos en orden, con fecha, concepto y quién los hizo. Puedes filtrarlos por persona (`--author`) o buscar un concepto (`--grep`).

**HEAD** es un puntero que indica "dónde estás ahora" — normalmente, el último commit de la rama actual. *Analogía:* el punto **"Usted está aquí"** del mapa de un centro comercial.

---

### `git diff` — ver diferencias

**Función:** muestra línea por línea qué cambió.

```bash
git diff                 # directorio de trabajo vs staging (lo NO preparado)
git diff --staged        # staging vs último commit (lo que vas a commitear)
git diff a1b2c3d 9f8e7d6 # entre dos commits
git diff main feat/login # entre dos ramas
```

```diff
 def agregar_tarea(titulo):
-    tareas.append(titulo)
+    if not titulo.strip():
+        raise ValueError("El título no puede estar vacío")
+    tareas.append(titulo.strip())
```

¿Qué compara cada variante?

```
 Directorio de trabajo        Staging area          Último commit (HEAD)
          │                        │                         │
          │<──── git diff ────────>│                         │
          │   (lo que aún no       │                         │
          │    preparas)           │<── git diff --staged ──>│
          │                        │   (lo que entra al      │
          │                        │    commit)              │
          │<─────────────────── git diff HEAD ──────────────>│
          │        (todo lo que cambió desde el último commit)
```

> [!example] Analogía
> El **control de cambios de Word**: en rojo y tachado lo que se quitó, en verde lo que se agregó.

---

### `git show` — ver un commit concreto

```bash
git show a1b2c3d
git show HEAD~1     # el commit anterior a HEAD
```

### Otros útiles

```bash
git rm debug.log             # borrar archivo y preparar el borrado
git mv utils.py helpers.py   # renombrar/mover
git blame tareas.py          # quién modificó cada línea y en qué commit
```

```
 git blame tareas.py

 9f8e7d6 (Ana  01-mar)  tareas = []
 4c2d9e0 (Ana  01-mar)  def agregar_tarea(titulo):
 7d4e2f1 (Luis 05-mar)      if not titulo.strip():
 7d4e2f1 (Luis 05-mar)          raise ValueError("Título vacío")
 7d4e2f1 (Luis 05-mar)      tareas.append(titulo.strip())
 4c2d9e0 (Ana  01-mar)  def listar_tareas():
 └─ commit  └─ autor y fecha de la última modificación de esa línea
```

> [!note] ¿Y para ver lo que hizo una persona en específico?
> `git blame` no filtra por persona: muestra quién cambió por última vez **cada línea** de un archivo. Para ver los cambios de alguien usa `git log --author="Ana"` (agrega `-p` para ver el código, o `-- tareas.py` para limitarlo a un archivo).

> [!example] `git blame` — Analogía
> Un documento que pasó por muchas manos y en cada párrafo aparece **la firma de quien lo escribió por última vez**. (No es para "culpar", es para saber a quién preguntarle por qué está así.)

## 4.3 Práctica guiada

Empezamos el proyecto del curso: **`gestor-tareas`**, un CLI en Python.

```bash
mkdir gestor-tareas && cd gestor-tareas
git init

echo "# Gestor de tareas CLI" > README.md
git status
git add README.md
git commit -m "Agrega README del proyecto"
```

Crea `tareas.py`:

```python
tareas = []

def agregar_tarea(titulo):
    tareas.append(titulo)

def listar_tareas():
    return tareas
```

Crea `app.py`:

```python
from tareas import agregar_tarea, listar_tareas

agregar_tarea("Configurar Git")
print(listar_tareas())
```

```bash
git status -s
git add tareas.py
git commit -m "feat: agrega módulo de tareas"
git add app.py
git commit -m "feat: agrega punto de entrada del CLI"
python app.py           # ['Configurar Git']
```

Ahora mejora `agregar_tarea` con la validación del ejemplo de `git diff` de arriba:

```bash
git diff
git add tareas.py
git diff --staged
git commit -m "fix: valida que el título de la tarea no esté vacío"

git log --oneline
git show HEAD~2
```

### Reto
1. Agrega a `tareas.py` una función `eliminar_tarea(indice)` y, en otro commit, su uso en `app.py`.
2. Usa `git log --oneline --graph` y explica qué ves.
3. ¿Qué muestra `git diff` justo después de `git add`? ¿Y `git diff --staged`?
4. Usa `git add -p` para preparar solo una parte de un archivo con dos cambios distintos.

