## 1.1 El problema

¿Te suena esta carpeta de proyecto?

```
sistema-ventas/
├── app.py
├── app_final.py
├── app_final_v2.py
├── app_final_v2_ESTE_SI_FUNCIONA.py
├── app_final_v2_ESTE_SI_FUNCIONA_DE_VERDAD.py
├── app_final_v2_ESTE_SI_FUNCIONA_DE_VERDAD_AHORA_SI.py <- La pasaron por memoria
```

Este "sistema" manual tiene problemas graves:

| Problema                                        | Consecuencia en un proyecto de software                                                                                    |
| ----------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| No se sabe **qué cambió** entre versiones       | Producción falla y nadie sabe qué línea se modificó desde el último deploy                                                 |
| No se sabe **quién** ni **por qué** cambió algo | Alguien cambió el `timeout` de la base de datos y nadie sabe por que hay contexto                                          |
| Dos personas editan el mismo archivo            | Juan sube su `app.py` al servidor  y borra la versión actual que habias subido ayer  asi la corrección que hiciste         |
| Borras algo por error                           | Un borrar archivo por error y adiós al módulo en el que trabajaste toda la semana                                          |
| Quieres probar una idea arriesgada              | Copias todo el proyecto a  sistema-ventas-experimento/ "por si acaso"                                                      |
| Incompatibilidad de versiones                   | No hay forma de saber qué versión exacta tiene cada quien generando el problema clásico de "En mi computadora si funciona" |

Un sistema real tiene cientos o miles de archivos (código, configuración, scripts SQL, pipelines) y decenas de desarrolladores modificándolos al mismo tiempo.

## 1.2 Definición

> [!abstract] Control de versiones
> Un **sistema de control de versiones (VCS, *Version Control System*)** es una herramienta que **registra los cambios** realizados sobre un conjunto de archivos **a lo largo del tiempo**, de modo que puedas **recuperar versiones específicas**, **comparar** cambios, saber **quién** hizo cada cambio y **por qué**, y permitir que **varias personas trabajen en paralelo**.

### Lo que un VCS te da

1. **Historial completo:** cada cambio queda registrado con autor, fecha y mensaje.
2. **Reversibilidad:** volver a cualquier punto anterior ("máquina del tiempo").
3. **Trabajo en paralelo:** ramas para desarrollar funcionalidades sin romper lo estable.
4. **Colaboración:** combinar el trabajo de varias personas de forma controlada.
5. **Trazabilidad / auditoría:** saber cuándo y por qué se introdujo un error.
6. **Respaldo:** las copias del repositorio funcionan como backup.

> [!example] Analogía: el expediente médico
> En un hospital, cada consulta queda anotada en tu **expediente**: fecha, médico, diagnóstico y tratamiento. Nadie borra una entrada anterior; si algo sale mal, se revisa el expediente para saber qué se hizo, cuándo y por qué. Un VCS es el expediente de tu proyecto.

## 1.3 Conceptos fundamentales

| Concepto                        | Definición                                                   | Analogía                                                      |
| ------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------- |
| **Repositorio**                 | Base de datos que contiene el proyecto y todo su historial   | La tarjeta de memoria con todas tus partidas                  |
| **Versión / Commit / Revisión** | Una "foto" del proyecto en un momento dado                   | Una partida guardada                                          |
| **Historial**                   | Secuencia ordenada de commits                                | La lista de partidas guardadas                                |
| **Rama (branch)**               | Línea de desarrollo independiente                            | Una línea temporal alternativa                                |
| **Fusión (merge)**              | Combinar los cambios de dos ramas                            | Unir dos líneas temporales en una                             |
| **Conflicto**                   | Dos cambios incompatibles sobre la misma parte de un archivo | Dos personas reescribieron el mismo párrafo de forma distinta |
| **Copia de trabajo**            | Los archivos que ves y editas                                | Lo que está pasando en pantalla ahora                         |
| **Diff**                        | Diferencia entre dos versiones                               | El juego de "encuentra las 7 diferencias"                     |
| **Remoto**                      | Copia del repositorio en otro lugar (servidor)               | La nube donde respaldas documentos o archivos                 |

### Snapshots vs. diferencias

Hay dos formas de guardar versiones:

- **Basado en diferencias (deltas):** guarda el archivo original y luego solo los cambios. (CVS, Subversion)
- **Basado en instantáneas (snapshots):** en cada versión guarda una "foto" completa del proyecto; los archivos que no cambiaron se guardan como un **enlace** a la versión anterior idéntica. **Así funciona Git.**

```
Deltas:     v1 ──Δ1──> Δ2 ──> Δ3        (reconstruir v4 = v1+Δ1+Δ2+Δ3)
Snapshots:  [A B C] → [A' B C] → [A' B C'] (cada commit es el proyecto completo)
```

### Los tres tipos de VCS

**1. Locales** — Una base de datos de versiones en tu propia computadora.
- **Ventajas:** Simple 
- **Desventajas:** Sin colaboración, si se daña el disco lo pierdes todo.
- *Analogía:* un **diario personal guardado en un cajón**: funciona, pero si se inunda la casa, se pierde todo y nadie más puede leerlo.

**2. Centralizados (CVCS)** — Un único servidor tiene el historial; los usuarios descargan solo la versión actual.
- **Ventajas:** Control central, fácil de administrar 
- **Desventajas:** Si el servidor cae nadie puede guardar versiones; punto único de falla; requieren conexión.
- *Analogía:* una **biblioteca con un único ejemplar** de cada libro: todos dependen de ella; si la biblioteca cierra, nadie puede consultar ni registrar nada.

```
      [ Servidor central (historial) ]
         /          |          \
     [Dev A]     [Dev B]     [Dev C]   (solo copia de trabajo)
```

**3. Distribuidos (DVCS)** — Cada persona tiene una **copia completa** del repositorio, con todo el historial.
- **Ventajas:** Trabajo sin conexión, muy rápido, cada copia es un respaldo, ramas baratas 
- **Desventajas:** Curva de aprendizaje mayor.
- *Analogía:* cada miembro de la familia tiene **una copia completa del álbum familiar**, con todas las fotos desde el principio. Se van intercambiando las fotos nuevas, y si alguien pierde su álbum, cualquier otro puede dárselo completo.

```
  [Dev A: repo completo] ⇄ [Servidor: repo completo] ⇄ [Dev B: repo completo]
            ⇅_____________________________________________⇅
```

**Git es un sistema distribuido.**

## 1.4 Git vs. GitHub

> [!warning] Confusión muy común
> **Git ≠ GitHub**

| Git                                      | GitHub                                                |
| ---------------------------------------- | ----------------------------------------------------- |
| **Software** de control de versiones     | **Plataforma web** que aloja repositorios Git         |
| Funciona en tu computadora, sin internet | Funciona en internet                                  |
| Creado por Linus Torvalds (2005)         | Fundado en 2008, adquirido por Microsoft (2018)       |
| Libre y de código abierto                | Servicio comercial con plan gratuito                  |
| Comandos en la terminal                  | Interfaz web + issues, pull requests, Actions, Pages… |

*Analogía:* **Git es la cámara; GitHub es Google Fotos o Instagram.** Con la cámara tomas y guardas las fotos tú solo; en la plataforma las subes, las respaldas, las compartes y otros las comentan. 
Alternativas a GitHub: GitLab, Bitbucket, Gitea, Azure DevOps.

## 1.5 Reflexión y autoevaluación

1. ¿Qué diferencia hay entre un VCS centralizado y uno distribuido?
2. ¿Por qué se dice que en Git "cada clon es un respaldo"?
3. ¿Git guarda diferencias o instantáneas?
4. Explica con tus palabras la diferencia entre Git y GitHub.