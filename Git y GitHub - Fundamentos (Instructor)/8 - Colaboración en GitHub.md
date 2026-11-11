## 8.1 Anatomía de un repositorio en GitHub (10 min)

> [!note]- Nota para el instructor
> - Navega en vivo un repositorio real y popular (por ejemplo, el de VS Code o el de Python) mostrando cada pestaña.

| Pestaña / elemento | Para qué sirve |
|--------------------|----------------|
| **Code** | Archivos, ramas, commits, botón *Clone* |
| **README.md** | Portada del proyecto: qué es, cómo instalarlo, cómo usarlo |
| **Issues** | Reportar errores, proponer ideas, organizar tareas |
| **Pull requests** | Proponer y revisar cambios antes de integrarlos |
| **Actions** | Automatización (CI/CD): pruebas, despliegues |
| **Projects** | Tableros tipo Kanban |
| **Wiki** | Documentación extendida |
| **Settings** | Colaboradores, ramas protegidas, visibilidad |
| **Star / Watch / Fork** | Marcar favorito / seguir / copiar a tu cuenta |
| **LICENSE** | Qué pueden hacer otros con tu código (MIT, GPL, Apache…) |

### README en Markdown

```markdown
# Gestor de tareas CLI
Aplicación de línea de comandos en Python para administrar tareas.

## Requisitos
- Python 3.10+

## Instalación
1. `git clone https://github.com/tu-usuario/gestor-tareas.git`
2. `python -m venv .venv` y activa el entorno
3. `cp .env.example .env`

## Uso
`python app.py`

## Pruebas
`python -m pytest`
```

## 8.2 Issues (5 min)

> [!note]- Nota para el instructor
> - Crea un issue en vivo en tu repositorio de demostración, con etiqueta y responsable.

Un **issue** es una tarjeta de trabajo: un error, una mejora o una pregunta.

- Se les asignan **etiquetas** (`bug`, `enhancement`, `good first issue`), **responsables** y **hitos**.
- Se cierran automáticamente desde un commit o PR: `git commit -m "fix: corrige expiración del token JWT. Closes #12"`.

> [!example] Analogía
> Los **post-it en el tablero** del equipo: cada uno es una tarea pendiente con dueño, y cuando se termina se quita del tablero.

## 8.3 Pull Requests (15 min)

> [!note]- Nota para el instructor
> - Es el flujo más importante del módulo: haz un PR completo en vivo, desde la rama hasta el merge y el borrado de la rama.
> - Muestra cómo dejar un comentario en una línea concreta y cómo se actualiza el PR al hacer push a la misma rama.

Un **Pull Request (PR)** es una **solicitud para fusionar** una rama en otra, con espacio para discutir y revisar el código antes.

> [!example] Analogía
> Entregar un **borrador al editor** de una revista: lo revisa, deja comentarios en los márgenes, pide correcciones y, cuando está aprobado, se **publica** (merge).

### Flujo de un PR (GitHub Flow)

```
1. Crear rama        git switch -c feat/exportar-csv
2. Commits           git add . && git commit -m "feat: exporta tareas a CSV"
3. Subir rama        git push -u origin feat/exportar-csv
4. Abrir PR          (GitHub muestra el botón "Compare & pull request")
5. Revisión          comentarios, CI ejecuta los tests, más commits a la misma rama
6. Merge             botón "Merge pull request"
7. Limpiar           borrar la rama; en local: git switch main && git pull
```

```
main              ──●───────────●─────────●── (merge del PR)
                      \                  /
feat/exportar-csv      ●──●──●──────────●
                            ↑ PR abierto, code review y CI
```

### Opciones de merge en GitHub

| Opción | Resultado |
|--------|-----------|
| **Create a merge commit** | Conserva todos los commits + commit de merge |
| **Squash and merge** | Junta todos los commits del PR en uno solo |
| **Rebase and merge** | Re-aplica los commits linealmente en `main` |

### Un buen PR
- Pequeño y enfocado en **una** cosa.
- Título claro y descripción: **qué** cambia, **por qué**, **cómo probarlo**.
- Enlaza el issue (`Closes #12`).
- Capturas si hay cambios visuales.

## 8.4 Fork: contribuir a proyectos ajenos (10 min)

> [!note]- Nota para el instructor
> - Dibuja los **tres lugares** (repo original, tu fork en GitHub, tu PC) para diferenciar fork de clone.

Si **no** tienes permiso de escritura en un repositorio, haces un **fork**: una copia del repositorio **en tu cuenta de GitHub**.

```
 Repo original (upstream)  ──fork──>  Tu copia en GitHub (origin)  ──clone──>  Tu PC
          ↑                                                                    │
          └──────────────────────── Pull Request <──── push ───────────────────┘
```

```bash
git clone https://github.com/TU-USUARIO/proyecto.git
cd proyecto
git remote add upstream https://github.com/AUTOR-ORIGINAL/proyecto.git
git switch -c fix/docs-instalacion
# ... cambios, add, commit ...
git push -u origin fix/docs-instalacion
# En GitHub: abrir PR desde tu fork hacia el repo original

# Mantener tu fork actualizado
git switch main
git fetch upstream
git merge upstream/main
git push origin main
```

> [!example] Analogía
> **Fork** = copiar una **receta familiar** en tu propio cuaderno. Puedes modificarla libremente y, si mejoras algo, se lo propones a la abuela (PR). Ella decide si lo incorpora al recetario original.

**Clone vs Fork:** *clone* copia un repo a tu **computadora**; *fork* copia un repo a tu **cuenta de GitHub**.

## 8.5 Buenas prácticas de colaboración (5 min)

> [!note]- Nota para el instructor
> - Las reglas de protección de ramas en repositorios **privados** requieren un plan de pago de GitHub; en públicos son gratuitas.

- **Protege `main`**: *Settings → Branches → Branch protection rules* (exigir PR y revisión).
- Nombra ramas con prefijos: `feat/`, `fix/`, `docs/`, `hotfix/`.
- `git pull` antes de empezar a trabajar.
- **Nunca** subas secretos (contraseñas, tokens, `.env`). Si ocurre, **revoca** el secreto: borrarlo del repo no basta porque queda en el historial.
- Revisa código con respeto: comenta el código, no a la persona.

## 8.6 Práctica en parejas (15 min)

> [!note]- Nota para el instructor
> - Forma parejas de nivel mixto. Si el número es impar, haz un trío con dos colaboradores.
> - Error común: la invitación de colaborador llega por correo y no la aceptan. También aparece en `github.com/notifications`.

**Persona A (propietaria):**
1. Sube (o comparte) su repo `gestor-tareas`.
2. *Settings → Collaborators* → invita a la persona B.
3. Crea un issue: "Agregar función para marcar una tarea como completada".

**Persona B (colaboradora):**
1. Acepta la invitación y clona el repo.
2. `git switch -c feat/completar-tarea`, agrega en `tareas.py` una función `completar_tarea(indice)`, commit con `Closes #1`.
3. `git push -u origin feat/completar-tarea` y abre el PR describiendo qué hace y cómo probarlo.

**Persona A:**
1. Revisa el PR y deja un comentario en una línea pidiendo un cambio (por ejemplo, "valida que el índice exista y lanza `IndexError` con un mensaje claro").

**Persona B:**
1. Haz el cambio, commit y push en la misma rama (el PR se actualiza solo).

**Persona A:**
1. Aprueba y hace *Merge*. Verifica que el issue se cerró solo.

**Ambas:** `git switch main && git pull`.

> [!tip] Si trabajas solo, haz la práctica con un **fork** de un repositorio de práctica como https://github.com/firstcontributions/first-contributions

---

## Guía del instructor

### Posibles preguntas de los alumnos

> [!question] ¿Qué diferencia hay entre ser colaborador y hacer fork?
> El colaborador tiene permiso de escritura y crea ramas en el mismo repositorio; el fork es para cuando **no** tienes permisos.

> [!question] ¿Puedo seguir agregando commits a un PR abierto?
> Sí: cada push a la misma rama actualiza el PR automáticamente.

> [!question] ¿Qué opción de merge elijo?
> Depende de la convención del equipo. *Squash and merge* deja un historial limpio con un commit por PR y es muy común.

> [!question] ¿Quién debe aprobar un PR?
> Alguien distinto del autor. En equipos se configura un número mínimo de aprobaciones en las reglas de la rama.

> [!question] ¿Qué diferencia hay entre un issue y un PR?
> El issue describe **qué** hay que hacer o qué falla; el PR propone **cómo** resolverlo con código.

> [!question] ¿Cómo empiezo a contribuir en open source?
> Busca issues con la etiqueta `good first issue` y lee el archivo `CONTRIBUTING.md` del proyecto antes de empezar.

### Respuestas de las actividades

**8.6 Práctica en parejas**
- El issue se cierra solo **al hacer merge** del PR en la rama por defecto (`main`), gracias a `Closes #1` en el commit o en la descripción del PR.
- Una posible versión final de la función:
  ```python
  def completar_tarea(indice):
      if indice < 0 or indice >= len(tareas):
          raise IndexError(f"No existe la tarea {indice}")
      tareas[indice] = "[x] " + tareas[indice]
  ```
- Al final, ambos ejecutan `git switch main && git pull` y deben ver el mismo historial.
