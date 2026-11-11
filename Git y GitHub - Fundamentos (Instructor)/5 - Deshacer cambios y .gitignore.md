
> [!tip] La regla de oro
> En Git **casi todo se puede deshacer** si ya está en un commit. Lo que más fácil se pierde son los cambios que **nunca** se guardaron.

## 5.1 ¿Dónde está el cambio que quiero deshacer? (5 min)

> [!note]- Nota para el instructor
> - Usa la tabla como **mapa de decisión**: ante cada problema pregunta primero "¿dónde está el cambio: en el archivo, en el staging o en un commit?".

| Situación                                                                             | Comando                        |
| ------------------------------------------------------------------------------------- | ------------------------------ |
| Edité `tareas.py`, lo rompí y quiero volver al último commit (no hice `add`)          | `git restore tareas.py`        |
| Hice `git add` de un archivo con `print()` de depuración y quiero sacarlo del staging | `git restore --staged archivo` |
| Hice commit y olvidé incluir el test, o el mensaje tiene un error                     | `git commit --amend`           |
| Quiero deshacer commits **locales** (no subidos)                                      | `git reset`                    |
| Un commit **ya subido** rompió producción                                             | `git revert`                   |
| Llega un bug urgente y estoy a medio programar otra cosa                              | `git stash`                    |

## 5.2 `git restore` (10 min)

> [!note]- Nota para el instructor
> - Remarca que `git restore archivo` es **irreversible**. Demuéstralo rompiendo un archivo y restaurándolo.

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

## 5.3 `git reset` (10 min)

> [!note]- Nota para el instructor
> - Demuestra los tres modos seguidos de `git status` para que vean dónde quedaron los cambios.
> - Presenta `git reflog` como red de seguridad: registra hacia dónde ha apuntado HEAD y permite recuperar commits tras un `reset --hard`.

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

## 5.4 `git revert` (5 min)

> [!note]- Nota para el instructor
> - Muestra que el historial **crece** con el revert. Pregunta: "¿por qué es más seguro que reset en un equipo?".

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

## 5.5 `git stash` (5 min)

> [!note]- Nota para el instructor
> - Error común: acumular stashes olvidados. Muestra `git stash list` y menciona `git stash -u` para incluir archivos nuevos.

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

## 5.6 `.gitignore` (5 min)

> [!note]- Nota para el instructor
> - Cuenta casos reales de llaves de nube filtradas en repositorios públicos: hay bots que las detectan y explotan en minutos.
> - Muestra el sitio de plantillas `github/gitignore` y la plantilla de Python.

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

## 5.7 Práctica (5 min)

> [!note]- Nota para el instructor
> - Al terminar, todos deberían tener un `.gitignore` versionado y `git status` limpio.

En tu repositorio `gestor-tareas`:
1. Rompe `tareas.py` (borra una función) y recupéralo con `git restore`.
2. Agrega un `print("DEBUG")` a `app.py`, haz `git add` y luego quítalo del staging.
3. Haz un commit con el mensaje `wip` y deshazlo con `git reset --soft HEAD~1`; vuelve a commitear con un buen mensaje.
4. Cambia la validación de `agregar_tarea`, haz commit, y luego revíertelo con `git revert`. Observa `git log --oneline`.
5. Crea un entorno virtual (`python -m venv .venv`) y un archivo `.env` con `API_KEY=123`. Agrégalos a `.gitignore` y comprueba que `git status` ya no los muestra. Crea y versiona un `.env.example` con `API_KEY=`.

---

## Guía del instructor

### Posibles preguntas de los alumnos

> [!question] Hice `git reset --hard` por error, ¿perdí todo?
> Si los cambios estaban en un commit, no: busca el hash con `git reflog` y vuelve con `git reset --hard <hash>`. Si nunca se hizo commit, Git no puede recuperarlos (a veces sí el historial local del editor, como el *Timeline* de VS Code).

> [!question] ¿Cuál es la diferencia entre `restore` y `checkout`?
> `checkout` hacía demasiadas cosas distintas; en Git 2.23 se dividió en `switch` (cambiar de rama) y `restore` (restaurar archivos). `checkout` sigue funcionando.

> [!question] ¿El `.gitignore` se sube al repositorio?
> Sí, debe versionarse para que todo el equipo ignore lo mismo. Las exclusiones personales van en `.git/info/exclude` o en un gitignore global.

> [!question] Ya subí una contraseña, ¿basta con borrarla y hacer commit?
> No: sigue en el historial. Primero **revoca o cambia** la contraseña, después limpia el historial con herramientas como `git filter-repo`.

> [!question] ¿Cómo sé por qué un archivo está siendo ignorado?
> Con `git check-ignore -v archivo`, que indica qué regla y de qué archivo lo ignora.

### Respuestas de las actividades

**5.7 Práctica**
1. Editar `tareas.py` → `git restore tareas.py` → `git status` queda limpio.
2. `git add app.py` → `git restore --staged app.py` → el archivo aparece como *modified*. Para quitar el `print` del todo: `git restore app.py`.
3. `git commit -am "wip"` → `git reset --soft HEAD~1` → `git status` muestra los cambios preparados → `git commit -m "fix: mensaje descriptivo"`.
4. `git log --oneline` muestra arriba un commit `Revert "..."` y, debajo, el commit original, que **sigue existiendo**.
5. `.gitignore` con las líneas `.venv/` y `.env`. El archivo `.env.example` no coincide con la regla `.env`, así que se puede versionar:
   ```bash
   git add .gitignore .env.example
   git commit -m "chore: ignora entorno virtual y archivo de secretos"
   ```
