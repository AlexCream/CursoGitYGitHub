## 2.1 Línea de tiempo

```
1972  SCCS ──── 1982  RCS ──── 1990  CVS ──── 2000  Subversion ──── 2005  Git / Mercurial ──── 2008  GitHub
 (local)          (local)       (centralizado)  (centralizado)        (distribuido)              (nube + colaboración)
```

| Año           | Sistema                                                            | Tipo                      | Aportación clave                                                                                                            |
| ------------- | ------------------------------------------------------------------ | ------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **1972**      | **SCCS** (Source Code Control System) — Marc Rochkind, Bell Labs   | Local                     | Primer VCS. Guardaba versiones de archivos individuales.                                                                    |
| **1982**      | **RCS** (Revision Control System) — Walter Tichy                   | Local                     | Guardaba *deltas inversos* (la última versión completa + diferencias hacia atrás). Más eficiente. Solo un archivo a la vez. |
| **1986–1990** | **CVS** (Concurrent Versions System) — Dick Grune / Brian Berliner | Centralizado              | Introduce el **modelo cliente-servidor** y permite que **varias personas trabajen simultáneamente** en el mismo proyecto.   |
| **2000**      | **Subversion (SVN)** — CollabNet                                   | Centralizado              | "CVS bien hecho": commits **atómicos**, versiona directorios, renombrado de archivos. Dominó la década de 2000.             |
| **2000**      | **BitKeeper** — BitMover                                           | Distribuido (propietario) | DVCS comercial; el kernel de Linux lo usó gratis desde 2002.                                                                |
| **2005**      | **Git** — Linus Torvalds                                           | Distribuido               | Velocidad, integridad (hashes SHA-1), ramas ligeras.                                                                        |
| **2005**      | **Mercurial (hg)** — Matt Mackall                                  | Distribuido               | Nació el mismo mes que Git por el mismo motivo. Más simple; usado por Mozilla y Facebook.                                   |
| **2008**      | **GitHub**                                                         | Plataforma                | Convierte Git en una red social de código: forks, pull requests, perfiles.                                                  |
| **2011–hoy**  | GitLab, Bitbucket, Azure DevOps…                                   | Plataformas               | Integración continua, DevOps, revisión de código.                                                                           |

## 2.2 La historia del nacimiento de Git

1. Desde **1991 hasta 2002**, el kernel de Linux se mantenía mediante parches enviados por correo y archivos comprimidos.
2. En **2002** el proyecto empezó a usar **BitKeeper**, un DVCS propietario cuya empresa concedió licencias gratuitas a la comunidad.
3. En **2005** la relación se rompió: un desarrollador (Andrew Tridgell) hizo ingeniería inversa del protocolo de BitKeeper y la empresa retiró la licencia gratuita.
4. **Linus Torvalds** decidió escribir su propio sistema. Tenía requisitos claros:
   - **Velocidad** (el kernel tenía miles de archivos y desarrolladores).
   - **Diseño simple.**
   - **Fuerte soporte para desarrollo no lineal** (miles de ramas en paralelo).
   - **Completamente distribuido.**
   - **Integridad**: que fuera imposible corromper el historial sin que se notara.
5. Comenzó a escribirlo el **3 de abril de 2005**; en unos **pocos días** Git ya se gestionaba a sí mismo, y en **junio de 2005** se usó para publicar la versión 2.6.12 del kernel.
6. En **julio de 2005** Linus cedió el mantenimiento a **Junio Hamano**, quien sigue siendo el mantenedor principal.

> [!quote] ¿Por qué "Git"?
> En el inglés británico, *git* es un insulto coloquial ("persona desagradable"). Torvalds bromeó diciendo que nombra sus proyectos en su honor: primero Linux y luego Git. El README original también lo describe como "the stupid content tracker".

## 2.3 GitHub y la popularización

- **2008:** Tom Preston-Werner, Chris Wanstrath, PJ Hyett y Scott Chacon lanzan GitHub.
- Popularizó el **fork + pull request**: cualquiera puede proponer cambios a cualquier proyecto abierto.
- **2018:** Microsoft compra GitHub por 7,500 millones de dólares.
- Hoy GitHub aloja **más de cien millones de desarrolladores** y cientos de millones de repositorios; Git es el VCS usado por la gran mayoría de los desarrolladores profesionales (encuestas como la de Stack Overflow lo sitúan por encima del 90 %).

## 2.4 ¿Por qué ganó Git?

| Factor | Explicación |
|--------|-------------|
| Velocidad | Casi todo ocurre localmente, sin red |
| Ramas baratas | Crear una rama es crear un archivo de 41 bytes |
| Integridad | Todo se identifica con un hash; cualquier cambio se detecta |
| Distribuido | Trabajo offline, sin punto único de falla |
| Efecto red | Linux + GitHub crearon un ecosistema imparable |

## 2.5 Actividad

Ordena cronológicamente y clasifica (local / centralizado / distribuido): *Subversion, Git, SCCS, CVS, RCS, Mercurial*.

