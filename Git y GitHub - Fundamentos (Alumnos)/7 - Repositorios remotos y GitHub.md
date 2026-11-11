## 7.1 ¿Qué es un remoto?

Un **remoto** es una copia del repositorio alojada en otro lugar (normalmente un servidor como GitHub). Por convención, el remoto principal se llama **`origin`**.

```
  Tu PC                                   GitHub
┌──────────────────┐   git push   ┌──────────────────┐
│ repo local       │ ───────────> │ repo remoto      │
│ main             │ <─────────── │ origin/main      │
└──────────────────┘ fetch / pull └──────────────────┘
```

> [!example] Analogía
> Tu repositorio local es el **documento en tu computadora**; el remoto es la **copia en Google Drive**. Pero a diferencia de Drive, Git **nunca sincroniza solo**: tú decides cuándo subir (`push`) y cuándo descargar (`pull`).

### Ramas de seguimiento remoto

`origin/main` es una referencia local que recuerda **cómo estaba `main` en GitHub la última vez que hablaste con él**. *Analogía:* la **foto que le tomaste al pizarrón de avisos** de la oficina: muestra cómo estaba cuando la tomaste, no cómo está ahora. Para actualizarla tienes que volver a tomarla (`git fetch`).

## 7.2 Autenticación con GitHub

GitHub **ya no acepta tu contraseña** desde la terminal. Opciones:

| Método | Cómo |
|--------|------|
| **HTTPS + Git Credential Manager** (recomendado en Windows) | Viene con Git for Windows; al primer `push` abre el navegador para iniciar sesión |
| **GitHub CLI** | `gh auth login` y seguir las instrucciones |
| **SSH** | Generar llave y registrarla en GitHub (abajo) |
| **Personal Access Token** | Se crea en *Settings → Developer settings* y se usa como contraseña |

Configurar SSH:

```bash
ssh-keygen -t ed25519 -C "tu-correo@ejemplo.com"
cat ~/.ssh/id_ed25519.pub          # copia la llave PÚBLICA
# GitHub → Settings → SSH and GPG keys → New SSH key → pegar
ssh -T git@github.com              # probar conexión
```

> [!example] Analogía SSH
> La llave **pública** es un **candado** que le das a GitHub para que lo ponga en su puerta; la **privada** es la única **llave** que lo abre, y solo tú la guardas. Puedes repartir candados a quien quieras, pero nunca prestes la llave.

## 7.3 Comandos de remotos

### `git clone` — copiar un repositorio existente

```bash
git clone https://github.com/usuario/proyecto.git
git clone git@github.com:usuario/proyecto.git mi-carpeta
```

Descarga **todo el historial**, configura `origin` automáticamente y crea la rama `main` local.

```
 GitHub                         git clone                Tu PC
 ┌───────────────────┐        ──────────────>       ┌───────────────────┐
 │ A ── B ── C       │                              │ A ── B ── C  main │
 └───────────────────┘                              │ origin → GitHub   │
                                                    └───────────────────┘
 Llega todo el historial, no solo la última versión.
```

> [!example] Analogía
> **Fotocopiar el libro completo**, con todas sus ediciones anteriores, no solo la última página. Por eso `clone` es más que "descargar un ZIP" del código.

---

### `git remote` — administrar conexiones

```bash
git remote -v                                         # ver remotos
git remote add origin https://github.com/yo/repo.git  # conectar
git remote rename origin github
git remote remove origin
```

```
 origin    ──>  https://github.com/yo/gestor-tareas.git
 upstream  ──>  https://github.com/autor/gestor-tareas.git
```

*Analogía:* tu **agenda de contactos**: guardas una dirección larga con un apodo corto (`origin`) para no tener que escribirla cada vez.

---

### `git push` — subir commits

```bash
git push -u origin main    # primera vez: sube y enlaza main ↔ origin/main
git push                   # siguientes veces
git push origin feat/login             # subir otra rama
git push origin --delete feat/login    # borrar rama remota
```

```
 Antes del push                          Después del push
 Tu PC:   A ── B ── C ── D               Tu PC:   A ── B ── C ── D
 GitHub:  A ── B ── C                    GitHub:  A ── B ── C ── D
                                         Tu commit D viajó a GitHub.
```

> [!example] Analogía
> **Enviar un paquete por paquetería**: lo que empacaste en casa (tus commits) viaja al servidor para que otros lo reciban.

> [!warning] Si el remoto tiene commits que tú no tienes, `push` será **rechazado**. Primero haz `git pull`. Evita `git push --force` en ramas compartidas.

---

### `git fetch` — descargar sin mezclar

```bash
git fetch origin
git log --oneline main..origin/main   # ver qué commits nuevos hay
```

`fetch` descarga los commits nuevos pero no mueve tu rama:

```
 GitHub:  A ── B ── C ── D ── E

 Tu PC:   A ── B ── C ── D ── E
                    ↑         ↑
                  main    origin/main      (main no se movió)
```

> [!example] Analogía
> **Revisar el buzón**: traes las cartas a casa, pero todavía no las abres ni cambias nada de lo que tienes.

---

### `git pull` — descargar y mezclar

```bash
git pull                   # = git fetch + git merge
git pull --rebase          # = git fetch + git rebase (historial lineal)
```

`pull` descarga y además integra: tu rama avanza.

```
 GitHub:  A ── B ── C ── D ── E

 Tu PC:   A ── B ── C ── D ── E
                              ↑
                       main, origin/main
```

> [!example] Analogía
> **Revisar el buzón, abrir las cartas y acomodarlas** en tus carpetas. Si una carta contradice algo que ya tenías archivado, toca resolverlo (conflicto).

**`fetch` vs `pull`:** `fetch` es seguro y no toca tu trabajo; `pull` integra los cambios y puede generar conflictos.

## 7.4 Práctica: subir tu primer repositorio

**Camino A — tienes un repo local (`gestor-tareas`):**
1. En GitHub: **New repository** → nombre `gestor-tareas` → **sin** README ni `.gitignore` (para evitar conflictos) → *Create*.
2. En la terminal:
   ```bash
   cd gestor-tareas
   git remote add origin https://github.com/TU-USUARIO/gestor-tareas.git
   git branch -M main
   git push -u origin main
   ```
3. Recarga la página de GitHub y explora tus commits. Verifica que `.env` y `.venv/` **no** se subieron.

**Camino B — empiezas en GitHub:**
1. Crea un repo **con** README y la plantilla de `.gitignore` para Python.
2. `git clone` → crea un `main.py` → `git add` → `git commit` → `git push`.

**Simular a otro desarrollador:**
1. En GitHub, edita `README.md` desde la web agregando una sección "Instalación" (`python app.py`) y haz commit.
2. En local: `git fetch`, luego `git status` (dirá *"Your branch is behind..."*), luego `git pull`.

### El flujo diario

```
git pull  →  (editar)  →  git status  →  git add  →  git commit  →  git push
```
