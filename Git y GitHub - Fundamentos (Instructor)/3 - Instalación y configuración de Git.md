## 3.1 Instalación (10 min)

> [!note]- Nota para el instructor
> - Idealmente ya llegaron con Git instalado. Usa este tiempo para resolver a quien no lo logró.
> - En el instalador de Windows las opciones por defecto están bien; sugiere elegir **VS Code** como editor y **main** como nombre de rama inicial.

| Sistema | Cómo instalar |
|---------|---------------|
| **Windows** | Descargar de https://git-scm.com (incluye **Git Bash**) o `winget install Git.Git` |
| **macOS** | `xcode-select --install` o `brew install git` |
| **Linux (Debian/Ubuntu)** | `sudo apt install git` |

Verifica:

```bash
git --version
# git version 2.4x.x
```

## 3.2 Configuración inicial (10 min)

> [!note]- Nota para el instructor
> - Error común: usar un correo distinto al de GitHub; los commits no aparecerán en su perfil.
> - Si alguien no quiere exponer su correo: GitHub ofrece una dirección `noreply` en *Settings → Emails*.

Git necesita saber **quién eres** porque cada commit lleva tu firma.

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu-correo@ejemplo.com"
```

> [!tip] Usa el mismo correo que en tu cuenta de GitHub para que tus commits se asocien a tu perfil.

Configuraciones recomendadas:

```bash
git config --global init.defaultBranch main     
# la rama inicial se llama "main"
git config --global core.editor "code --wait"   
# usar VS Code como editor
git config --global core.autocrlf true          
# Windows: manejar saltos de línea

# (en macOS/Linux usa: core.autocrlf input)
```

Ver la configuración:

```bash
git config --list
git config user.name
```

### Niveles de configuración

| Nivel | Opción | Archivo | Alcance |
|-------|--------|---------|---------|
| Sistema | `--system` | `/etc/gitconfig` | Todos los usuarios |
| Global | `--global` | `~/.gitconfig` | Tu usuario |
| Local | `--local` (por defecto) | `.git/config` | Solo ese repositorio |

El nivel más específico gana: **local > global > sistema**.

> [!example] Analogía
> Son como las **reglas de convivencia**: están las leyes del país (sistema), las reglas de tu casa (global) y las reglas de tu cuarto (local). Si chocan, manda la regla más cercana a ti.

## 3.3 Pedir ayuda

> [!note]- Nota para el instructor
> - Muestra `git commit -h` en vivo; es más práctico que el manual completo.

```bash
git help commit        # manual completo
git commit -h          # resumen rápido de opciones
```

## 3.4 La terminal mínima que necesitas (5 min)

> [!note]- Nota para el instructor
> - Recomienda **Git Bash** a quien use Windows para que los comandos coincidan con el material (`touch`, `ls`, `cat`).

| Comando | Qué hace |
|---------|----------|
| `pwd` | Muestra en qué carpeta estás |
| `ls` / `ls -a` | Lista archivos (`-a` incluye ocultos como `.git`) |
| `cd carpeta` / `cd ..` | Entrar / salir de una carpeta |
| `mkdir nombre` | Crear carpeta |
| `touch archivo.txt` | Crear archivo vacío (Git Bash/macOS/Linux) |
| `echo "texto" > a.txt` | Escribir texto en un archivo |
| `cat archivo` | Ver contenido |

## 3.5 Ejercicio (5 min)

> [!note]- Nota para el instructor
> - Pasa por cada lugar a verificar `git config user.name` y `git config user.email` antes de seguir.

1. Instala Git y comprueba la versión (y `python --version`, lo usaremos en las prácticas).
2. Configura nombre, correo y rama por defecto `main`.
3. Ejecuta `git config --list --show-origin` y localiza en qué archivo quedó guardado tu nombre.
4. Crea tu cuenta en https://github.com si aún no la tienes.

**3.5 Ejercicio**
- Punto 3: `git config --list --show-origin` muestra una línea como `file:C:/Users/<usuario>/.gitconfig  user.name=Tu Nombre`. 
- El nombre quedó en el archivo **global** del usuario porque se usó `--global`.
---

## Guía del instructor

### Posibles preguntas de los alumnos

> [!question] Me equivoqué al escribir mi nombre, ¿qué hago?
> Vuelve a ejecutar `git config --global user.name "Nombre correcto"`. Solo afecta a los commits futuros.

> [!question] ¿Qué pasa si me equivoco con el correo?
> No se rompe nada. Git no verifica el correo: solo lo escribe tal cual en cada commit como dato del autor.
> - **Para corregirlo:** vuelve a ejecutar `git config --global user.email "correo-correcto@ejemplo.com"` y confírmalo con `git config user.email`. Igual que con el nombre, solo afecta a los commits futuros.
> - **Consecuencia en GitHub:** GitHub enlaza cada commit con tu cuenta por el correo. Si el correo no coincide con uno registrado en tu cuenta (Settings > Emails), los commits aparecen con un avatar gris y no cuentan en tu gráfica de contribuciones. Se arregla usando el mismo correo de GitHub o agregando el otro correo a la cuenta. Al agregarlo, GitHub reconoce también los commits anteriores.
> - **Commits que ya se hicieron con el correo mal:** el último se corrige con `git commit --amend --reset-author --no-edit`. Para corregir varios hay que reescribir el historial, y eso no se hace si ya están subidos y compartidos. En este curso no vale la pena: basta con corregir la configuración.
> - **Privacidad:** si no quieres que tu correo personal quede público en los commits, usa el correo `noreply` que ofrece GitHub en Settings > Emails.

> [!question] ¿Para qué sirve `core.autocrlf`?
> Windows termina las líneas con CRLF y Linux/macOS con LF. Esta opción evita que aparezcan diferencias falsas en todo el archivo solo por el fin de línea.

> [!question] ¿Puedo usar GitHub Desktop o los botones de VS Code en lugar de la terminal?
> Sí, pero conviene aprender primero la terminal: las interfaces gráficas ejecutan estos mismos comandos y entenderlos ayuda cuando algo falla.

