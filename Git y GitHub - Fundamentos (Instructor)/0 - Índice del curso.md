# Curso: Fundamentos de Git y GitHub (guía del instructor)

> [!info] Datos del curso
> - **Dirigido a:** ingenieros en sistemas (saben programar, pero no han usado Git o lo usan "de memoria")
> - **Nivel:** Fundamentos (no se requiere experiencia previa con Git)
> - **Duración total:** 8 horas (teoría + práctica guiada)
> - **Requisitos:** una computadora, una terminal (Git Bash, PowerShell o Terminal), VS Code, Python 3 y una cuenta gratuita de GitHub
> - **Proyecto que acompaña todo el curso:** `gestor-tareas`, una pequeña aplicación de línea de comandos en Python que crece módulo a módulo
> - **Metodología:** cada módulo combina teoría, analogías, comandos con ejemplos y un ejercicio práctico

## Objetivos de aprendizaje

Al terminar el curso serás capaz de:

1. Explicar **qué es el control de versiones**, por qué existe y qué problemas resuelve.
2. Distinguir entre sistemas **locales, centralizados y distribuidos**, y ubicar a Git en su historia.
3. Entender el **modelo mental de Git**: directorio de trabajo, *staging area* y repositorio.
4. Usar con confianza los comandos esenciales: `init`, `add`, `commit`, `status`, `log`, `diff`, `restore`, `reset`, `revert`, `branch`, `switch`, `merge`, `clone`, `push`, `pull`, `fetch`.
5. Trabajar con **ramas** y resolver **conflictos** de fusión.
6. Conectar un repositorio local con **GitHub** y colaborar mediante **forks, issues y pull requests**.

## Agenda (8 horas)

| #   | Módulo                                     | Tipo              | Duración | Sesion |
| --- | ------------------------------------------ | ----------------- | -------- | ------ |
| 1   | [[Cursos/Git y GitHub - Fundamentos (Instructor)/1 - Teoría del control de versiones\|1 - Teoría del control de versiones]]    | Teoría            | 60 min   | 1      |
| 2   | [[Cursos/Git y GitHub - Fundamentos (Instructor)/2 - Historia del control de versiones\|2 - Historia del control de versiones]]  | Teoría            | 30 min   | 1      |
| 3   | [[Cursos/Git y GitHub - Fundamentos (Instructor)/3 - Instalación y configuración de Git\|3 - Instalación y configuración de Git]] | Práctica          | 30 min   | 1      |
| 4   | [[Cursos/Git y GitHub - Fundamentos (Instructor)/4 - El ciclo básico de Git\|4 - El ciclo básico de Git]]             | Teoría + práctica | 90 min   | 2      |
| 5   | [[Cursos/Git y GitHub - Fundamentos (Instructor)/5 - Deshacer cambios y .gitignore\|5 - Deshacer cambios y .gitignore]]      | Práctica          | 45 min   | 2      |
| 6   | [[Cursos/Git y GitHub - Fundamentos (Instructor)/6 - Ramas, fusiones y conflictos\|6 - Ramas, fusiones y conflictos]]       | Teoría + práctica | 75 min   | 3      |
| 7   | [[Cursos/Git y GitHub - Fundamentos (Instructor)/7 - Repositorios remotos y GitHub\|7 - Repositorios remotos y GitHub]]      | Práctica          | 60 min   | 3      |
| 8   | [[Cursos/Git y GitHub - Fundamentos (Instructor)/8 - Colaboración en GitHub\|8 - Colaboración en GitHub]]             | Práctica          | 60 min   | 4      |
| 9   | [[Cursos/Git y GitHub - Fundamentos (Instructor)/9 - Proyecto final y evaluación\|9 - Proyecto final y evaluación]]        | Proyecto          | 30 min   | 4      |


# ¿Que es git?
**Git es como el sistema de guardado de un videojuego.**
- Un repositorio es una partida
- Cada **commit** es un punto de guardado a el que puedes volver.
- El **staging area** es elegir *qué* cosas incluir en ese guardado.
- Una **rama** es una *línea temporal alternativa*: pruebas algo arriesgado sin perder tu partida principal.
- **GitHub** es *la nube* donde subes tus partidas para no perderlas y compartirlas con tu equipo.

## Guía general para el instructor

> [!abstract] Cómo usar esta versión
> - Cada sección tiene una **Nota para el instructor** plegada justo debajo de su título: qué enfatizar, qué demostrar en vivo y qué errores esperar.
> - Al final de cada módulo hay una **Guía del instructor** con las preguntas que suelen hacer los alumnos (con respuesta sugerida) y las **respuestas de todas las actividades**.
> - Las respuestas que el material del alumno no incluye aquí se muestran desplegadas.
> - La versión para los alumnos está en la carpeta `Git y GitHub - Fundamentos (Alumnos)`: no tiene tiempos ni respuestas.

### Antes del curso (1 semana antes)
- Envía a los alumnos las instrucciones del [[Cursos/Git y GitHub - Fundamentos (Instructor)/3 - Instalación y configuración de Git|módulo 3]] para que lleguen con **Git, VS Code y Python 3 instalados** y una **cuenta de GitHub con el correo verificado**.
- Verifica que la red del aula permita acceder a `github.com`. Muchas redes corporativas o universitarias bloquean SSH (puerto 22): en ese caso trabaja con **HTTPS**.
- Prepara tu propio repositorio `gestor-tareas` terminado como referencia y uno vacío para hacer las demostraciones en vivo.
- Imprime o comparte la [[Cursos/Git y GitHub - Fundamentos (Instructor)/10 - Chuleta de comandos|chuleta de comandos]] desde el inicio.

### Durante el curso
- Usa una terminal con **fuente grande** y tema claro; ejecuta `git status` y `git log --oneline --graph --all` después de casi cada comando para que vean el efecto.
- Dibuja en el pizarrón las **tres áreas** (módulo 4) y el **grafo de commits** (módulo 6) y déjalos visibles todo el curso.
- Regla práctica: si más del 20 % del grupo está atorado, detente y resuelve en grupo; si son pocos, apóyate en los alumnos que van adelantados como monitores.
- Los tiempos de la agenda suman 480 min **sin descansos**: agrega 10 min de descanso por cada bloque de 2 horas.
- Plan B sin internet: los módulos 1 a 6 se pueden dar completamente en local.

### Evaluación sugerida
| Elemento | Peso |
|----------|------|
| Proyecto final (ver rúbrica en el módulo 9) | 50 % |
| Evaluación escrita del módulo 9 | 30 % |
| Prácticas de los módulos 4 a 8 | 20 % |
