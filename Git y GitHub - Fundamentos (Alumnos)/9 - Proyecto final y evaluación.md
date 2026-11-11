## 9.1 Proyecto final: tu portafolio en GitHub Pages

**Objetivo:** aplicar el ciclo completo — repo, commits, ramas, merge, push, PR y publicación.

### Pasos

1. Crea en GitHub un repositorio llamado **`TU-USUARIO.github.io`** (público, con README).
2. Clónalo:
   ```bash
   git clone https://github.com/TU-USUARIO/TU-USUARIO.github.io.git
   cd TU-USUARIO.github.io
   ```
3. Crea `index.html` y haz commit en `main`:
   ```html
   <!doctype html>
   <html lang="es">
   <head><meta charset="utf-8"><title>Mi portafolio</title></head>
   <body>
     <h1>Hola, soy TU NOMBRE</h1>
     <p>Ingeniero(a) en Sistemas. Tecnologías: Python, SQL, Git.</p>
   </body>
   </html>
   ```
   ```bash
   git add index.html
   git commit -m "feat: agrega página inicial"
   git push
   ```
4. Crea una rama `feat/proyectos`, agrega una sección "Mis proyectos" con un enlace a tu repo `gestor-tareas` (y a cualquier otro proyecto de la carrera), haz commit y push.
5. Abre un **Pull Request**, revísalo tú mismo y haz **merge**.
6. Agrega un `.gitignore` y mejora el `README.md`.
7. Espera 1–2 minutos y visita `https://TU-USUARIO.github.io`
   (si no aparece: *Settings → Pages → Source: Deploy from branch → main*).

### Lista de verificación

- [ ] Al menos 5 commits con mensajes descriptivos
- [ ] Al menos 1 rama creada y fusionada mediante PR
- [ ] README con descripción del proyecto
- [ ] `.gitignore` presente
- [ ] Sitio publicado y accesible

## 9.2 Evaluación

### Preguntas de opción múltiple

1. ¿Qué comando mueve cambios al staging area?
   a) `git commit` b) `git add` c) `git push` d) `git stage-all`
2. ¿Qué tipo de VCS es Git?
   a) Local b) Centralizado c) Distribuido d) En la nube
3. ¿Quién creó Git y en qué año?
   a) Bill Gates, 2008 b) Linus Torvalds, 2005 c) Junio Hamano, 2000 d) GitHub Inc., 2008
4. ¿Qué hace `git fetch`?
   a) Descarga y fusiona b) Sube commits c) Descarga sin fusionar d) Borra ramas remotas
5. Quieres deshacer un commit que **ya subiste** a GitHub. ¿Qué usas?
   a) `git reset --hard` b) `git revert` c) `git restore` d) Borrar `.git`
6. ¿Qué es `origin`?
   a) La primera rama b) El primer commit c) El nombre por defecto del remoto d) Un comando
7. ¿Qué es un fork?
   a) Una copia del repo en tu cuenta de GitHub b) Una rama local c) Un conflicto d) Un tipo de commit
8. Los marcadores `<<<<<<<`, `=======`, `>>>>>>>` indican…
   a) Un error de sintaxis b) Un conflicto de fusión c) Un archivo ignorado d) Un commit vacío

### Preguntas abiertas

1. Explica con una analogía propia las tres áreas de Git.
2. ¿Cuándo usarías `git stash`?
3. Encontraste un bug en una librería open source que usa tu sistema. Describe el flujo completo para enviar la corrección.
4. Un compañero subió el `.env` con la contraseña de la base de datos de producción. ¿Por qué es grave y qué pasos sigues?
5. Un deploy de ayer rompió producción y el commit ya está en `main` compartido. ¿Usas `reset` o `revert`? ¿Por qué?

### Ejercicio práctico de diagnóstico

Ordena correctamente esta secuencia para publicar un cambio nuevo:

`git push` · `git commit -m "..."` · `git pull` · `git add .` · `git switch -c feat/x` · editar archivos

## 9.3 ¿Qué sigue? (temas intermedios)

- `git rebase` e interactive rebase (`git rebase -i`)
- `git cherry-pick`, `git reflog` (recuperar commits "perdidos")
- `git tag` y versionado semántico (SemVer)
- Flujos de trabajo: GitHub Flow, Git Flow, trunk-based development
- GitHub Actions (CI/CD)
- Firmar commits, ramas protegidas, CODEOWNERS

## Recursos

- Libro oficial gratuito en español: **Pro Git** — https://git-scm.com/book/es/v2
- Documentación de GitHub: https://docs.github.com/es
- Práctica visual interactiva: https://learngitbranching.js.org/?locale=es_ES
- Curso interactivo de GitHub: https://skills.github.com
