# Curso: Fundamentos de Git y GitHub (material del alumno)

> [!info] Datos del curso
> - **Dirigido a:** ingenieros en sistemas (saben programar, pero no han usado Git o lo usan "de memoria")
> - **Nivel:** Fundamentos (no se requiere experiencia previa con Git)
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

## Agenda

| #   | Módulo                                     | Tipo              | Sesion |
| --- | ------------------------------------------ | ----------------- | ------ |
| 1   | [[Cursos/Git y GitHub - Fundamentos (Alumnos)/1 - Teoría del control de versiones\|1 - Teoría del control de versiones]]    | Teoría            | 1      |
| 2   | [[Cursos/Git y GitHub - Fundamentos (Alumnos)/2 - Historia del control de versiones\|2 - Historia del control de versiones]]  | Teoría            | 1      |
| 3   | [[Cursos/Git y GitHub - Fundamentos (Alumnos)/3 - Instalación y configuración de Git\|3 - Instalación y configuración de Git]] | Práctica          | 1      |
| 4   | [[Cursos/Git y GitHub - Fundamentos (Alumnos)/4 - El ciclo básico de Git\|4 - El ciclo básico de Git]]             | Teoría + práctica | 2      |
| 5   | [[Cursos/Git y GitHub - Fundamentos (Alumnos)/5 - Deshacer cambios y .gitignore\|5 - Deshacer cambios y .gitignore]]      | Práctica          | 2      |
| 6   | [[Cursos/Git y GitHub - Fundamentos (Alumnos)/6 - Ramas, fusiones y conflictos\|6 - Ramas, fusiones y conflictos]]       | Teoría + práctica | 3      |
| 7   | [[Cursos/Git y GitHub - Fundamentos (Alumnos)/7 - Repositorios remotos y GitHub\|7 - Repositorios remotos y GitHub]]      | Práctica          | 3      |
| 8   | [[Cursos/Git y GitHub - Fundamentos (Alumnos)/8 - Colaboración en GitHub\|8 - Colaboración en GitHub]]             | Práctica          | 4      |
| 9   | [[Cursos/Git y GitHub - Fundamentos (Alumnos)/9 - Proyecto final y evaluación\|9 - Proyecto final y evaluación]]        | Proyecto          | 4      |

# ¿Que es git?
**Git es como el sistema de guardado de un videojuego.**
- Un repositorio es una partida
- Cada **commit** es un punto de guardado a el que puedes volver.
- El **staging area** es elegir *qué* cosas incluir en ese guardado.
- Una **rama** es una *línea temporal alternativa*: pruebas algo arriesgado sin perder tu partida principal.
- **GitHub** es *la nube* donde subes tus partidas para no perderlas y compartirlas con tu equipo.
