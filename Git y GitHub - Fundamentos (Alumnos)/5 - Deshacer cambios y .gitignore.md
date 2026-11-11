
> [!tip] La regla de oro
> En Git **casi todo se puede deshacer** si ya está en un commit. Lo que más fácil se pierde son los cambios que **nunca** se guardaron.

## 5.1 ¿Dónde está el cambio que quiero deshacer?

| Situación | Comando |
|-----------|---------|
| Edité `tareas.py`, lo rompí y quiero volver al último commit (no hice `add`) | `git restore tareas.py` |
| Hice `git add` de un archivo con `print()` de depuración y quiero sacarlo del staging | `git restore --staged archivo` |
| Hice commit y olvidé incluir el test, o el mensaje tiene un error | `git commit --amend` |
| Quiero deshacer commits **locales** (no subidos) | `git reset` |
| Un commit **ya subido** rompió producción | `git revert` |
| Llega un bug urgente y estoy a medio programar otra cosa | `git stash` |

## 5.2 `git restore`

**Función:** restaura archivos del directorio de trabajo o del staging area.

```bash
git restore tareas.py                    # descarta cambios no preparados (irreversible)
git restore --staged tareas.py           # quita del staging (el cambio sigue en el archivo)
git restore --source=HEAD~2 config.py    # recupera el archivo como estaba hace 2 commits
```

¿De dónde copia cada variante?

```
 Directorio de trabajo        Staging area          Último commit (HEAD)
          │                        │                         │
          │<── git restore ────────│                         │
          │    tareas.py           │                         │
          │    (se pierden tus     │<── git restore ─────────│
          │     cambios)           │    --staged tareas.py   │
          │                        │    (tu archivo no cambia)
```

> [!example] Analogía
> - `git restore archivo` = **cerrar un documento sin guardar**: vuelves a la última versión guardada y lo que escribiste después se pierde.
> - `git restore --staged` = **sacar un producto del carrito** y devolverlo al estante: el producto sigue existiendo, solo que ya no lo vas a pagar.

> [!note] En versiones antiguas se usaba `git checkout -- archivo` y `git reset HEAD archivo`. Verás eso en tutoriales viejos y en Stack Overflow.

## 5.3 `git reset`

**Función:** mueve la rama actual (y HEAD) a otro commit. Tiene tres modos:

```
                 Historial   Staging     Directorio de trabajo
--soft            mueve       intacto     intacto
--mixed (def.)    mueve       se limpia   intacto
--hard            mueve       se limpia   SE BORRA (irreversible)
```

```bash
git reset --soft HEAD~1    # deshace el último commit; los cambios quedan preparados
git reset HEAD~1           # deshace el commit; cambios quedan en el archivo, sin preparar
git reset --hard HEAD~1    # deshace el commit y BORRA los cambios
```

Qué pasa con `git reset HEAD~1`:

```
 Antes:    A ── B ── C  ← main          Después:   A ── B  ← main      (C queda fuera)

 ¿A dónde van los cambios que traía C?
   --soft    → al staging, listos para un nuevo commit
   --mixed   → a tus archivos, sin preparar
   --hard    → a ninguna parte: se pierden
```

> [!example] Analogía: rebobinar una película
> - `--soft`: rebobinas, pero el guion de la escena sigue en tu mano, listo para volver a grabar.
> - `--mixed`: rebobinas y el guion vuelve a tu escritorio, sin preparar.
> - `--hard`: rebobinas y **tiras el guion a la basura**.

> [!danger] No uses `reset` sobre commits que ya subiste a GitHub: reescribe el historial y tus compañeros tendrán un historial distinto al tuyo.

## 5.4 `git revert`

**Función:** crea un **nuevo commit** que hace lo contrario de un commit anterior. **No borra historia.**

```bash
git revert a1b2c3d
```

```
Antes:   A ── B ── C          (C introdujo un bug en producción)
Después: A ── B ── C ── C'    (C' deshace lo que hizo C)
```

> [!example] Analogía
> Un periódico no puede "desimprimir" una noticia equivocada: al día siguiente publica una **fe de erratas**. La noticia original y la corrección quedan ambas en el archivo.

**reset vs revert:** `reset` = editar el historial (solo válido en tu rama local). `revert` = agregar una corrección al historial (seguro en ramas compartidas y en producción).

## 5.5 `git stash`

**Función:** guarda temporalmente los cambios sin commitearlos y deja el directorio limpio.

```bash
git stash              # guarda y limpia
git stash list         # ver lo guardado
git stash pop          # recupera lo último y lo elimina del stash
git stash apply        # recupera sin eliminar
git stash drop         # elimina el último stash
```

> [!example] Analogía
> Estás armando un **rompecabezas** en la mesa y te llaman a comer: lo guardas en una caja tal como está (`stash`), comes, y luego lo vuelves a sacar para seguir donde ibas (`stash pop`). En la práctica: estás a medio programar un *feature* y te piden un *hotfix* urgente.

## 5.6 `.gitignore`

**Función:** archivo de texto que indica qué archivos **no** debe rastrear Git.

```gitignore
# Dependencias
node_modules/
venv/
.venv/

# Archivos compilados / build
*.pyc
__pycache__/
dist/
build/
*.class
target/

# Secretos
.env
*.pem
*.key
credentials.json

# Logs y bases de datos locales
*.log
*.sqlite3

# Sistema operativo / IDE
.DS_Store
Thumbs.db
.vscode/
.idea/

# Excepción: sí rastrear este
!.env.example
```

Cómo lo ve Git:

```
 gestor-tareas/                         git status -s
 ├── app.py           Git lo ve         ?? .env.example
 ├── tareas.py        Git lo ve         ?? .gitignore
 ├── .gitignore       Git lo ve         ?? app.py
 ├── .env.example     Git lo ve         ?? tareas.py
 ├── .env             ignorado
 ├── .venv/           ignorado          Los ignorados ni siquiera aparecen:
 └── __pycache__/     ignorado          no puedes agregarlos por accidente.
```

> [!example] Analogía
> La lista de **"esto no se empaca"** en una mudanza: la basura, lo que puedes volver a comprar en el nuevo lugar y tus papeles privados no van en el camión. Igual, lo que se puede **regenerar** (dependencias, compilados) o lo que es **secreto** no se versiona.

> [!warning] `.gitignore` solo afecta archivos **no rastreados**. Si ya hiciste commit de un archivo, primero sácalo del índice: `git rm --cached archivo`.

Plantillas por lenguaje: https://github.com/github/gitignore

## 5.7 Práctica

En tu repositorio `gestor-tareas`:
1. Rompe `tareas.py` (borra una función) y recupéralo con `git restore`.
2. Agrega un `print("DEBUG")` a `app.py`, haz `git add` y luego quítalo del staging.
3. Haz un commit con el mensaje `wip` y deshazlo con `git reset --soft HEAD~1`; vuelve a commitear con un buen mensaje.
4. Cambia la validación de `agregar_tarea`, haz commit, y luego revíertelo con `git revert`. Observa `git log --oneline`.
5. Crea un entorno virtual (`python -m venv .venv`) y un archivo `.env` con `API_KEY=123`. Agrégalos a `.gitignore` y comprueba que `git status` ya no los muestra. Crea y versiona un `.env.example` con `API_KEY=`.
