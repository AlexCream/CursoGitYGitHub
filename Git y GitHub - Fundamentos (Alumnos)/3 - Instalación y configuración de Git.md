## 3.1 Instalación

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

## 3.2 Configuración inicial

Git necesita saber **quién eres** porque cada commit lleva tu firma.

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu-correo@ejemplo.com"
```

> [!tip] Usa el mismo correo que en tu cuenta de GitHub para que tus commits se asocien a tu perfil.

Configuraciones recomendadas:

```bash
git config --global init.defaultBranch main     # la rama inicial se llama "main"
git config --global core.editor "code --wait"   # usar VS Code como editor
git config --global core.autocrlf true          # Windows: manejar saltos de línea
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

```bash
git help commit        # manual completo
git commit -h          # resumen rápido de opciones
```

## 3.4 La terminal mínima que necesitas

| Comando | Qué hace |
|---------|----------|
| `pwd` | Muestra en qué carpeta estás |
| `ls` / `ls -a` | Lista archivos (`-a` incluye ocultos como `.git`) |
| `cd carpeta` / `cd ..` | Entrar / salir de una carpeta |
| `mkdir nombre` | Crear carpeta |
| `touch archivo.txt` | Crear archivo vacío (Git Bash/macOS/Linux) |
| `echo "texto" > a.txt` | Escribir texto en un archivo |
| `cat archivo` | Ver contenido |

## 3.5 Ejercicio

1. Instala Git y comprueba la versión (y `python --version`, lo usaremos en las prácticas).
2. Configura nombre, correo y rama por defecto `main`.
3. Ejecuta `git config --list --show-origin` y localiza en qué archivo quedó guardado tu nombre.
4. Crea tu cuenta en https://github.com si aún no la tienes.
