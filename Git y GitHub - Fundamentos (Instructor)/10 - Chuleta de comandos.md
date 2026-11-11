> [!note]- Nota para el instructor
> Imprime esta chuleta o compártela al inicio del curso. Pide a los alumnos que la tengan abierta durante todas las prácticas.

## Configuración
| Comando | Función | Analogía |
|---------|---------|----------|
| `git config --global user.name "Nombre"` | Define tu nombre | Firmar tu trabajo |
| `git config --global user.email "correo"` | Define tu correo | Tu dirección de remitente |
| `git config --list` | Ver configuración | Revisar ajustes |

## Crear y obtener repositorios
| Comando | Función | Analogía |
|---------|---------|----------|
| `git init` | Crea un repo en la carpeta actual | Abrir una cuenta en el banco |
| `git clone <url>` | Copia un repo remoto con todo su historial | Fotocopiar el libro completo |

## Ciclo básico
| Comando | Función | Analogía |
|---------|---------|----------|
| `git status` | Estado de los archivos | Tablero del coche |
| `git add <archivo>` / `git add .` | Preparar cambios | Meter al carrito |
| `git commit -m "msg"` | Guardar instantánea | Pagar y recibir ticket / guardar partida |
| `git commit --amend` | Corregir el último commit | Reimprimir el último ticket |
| `git log --oneline --graph --all` | Ver historial | Estado de cuenta del banco |
| `git diff` / `git diff --staged` | Ver diferencias | Control de cambios de Word |
| `git show <hash>` | Ver un commit | Revisar un ticket por su folio |
| `git blame <archivo>` | Autor de cada línea | Firma en cada párrafo |

## Deshacer
| Comando                          | Función                                       | Analogía                                     |
| -------------------------------- | --------------------------------------------- | -------------------------------------------- |
| `git restore <archivo>`          | Descartar cambios no preparados (irreversible)          | Cerrar sin guardar                           |
| `git restore --staged <archivo>` | Sacar del staging                             | Devolver un producto al estante              |
| `git reset --soft HEAD~1`        | Deshacer commit, conservar cambios preparados | Rebobinar y conservar el guion               |
| `git reset --hard HEAD~1`        | Deshacer commit y borrar cambios (irreversible)         | Rebobinar y tirar el guion                   |
| `git revert <hash>`              | Commit que anula otro (seguro en público)     | Fe de erratas                                |
| `git stash` / `git stash pop`    | Guardar/recuperar cambios temporales          | Guardar el rompecabezas en su caja           |

## Ramas
| Comando | Función | Analogía |
|---------|---------|----------|
| `git branch` | Listar ramas | Ver tus slots de guardado |
| `git branch <nombre>` | Crear rama | Nuevo slot de guardado / borrador |
| `git switch <rama>` | Cambiar de rama | Cambiar de canal |
| `git switch -c <rama>` | Crear y cambiar | Nuevo slot y entrar |
| `git merge <rama>` | Fusionar en la actual | Pasar el borrador en limpio |
| `git merge --abort` | Cancelar fusión con conflictos | Dejar todo como estaba |
| `git branch -d <rama>` | Borrar rama fusionada | Tirar el borrador ya pasado en limpio |

## Remotos
| Comando | Función | Analogía |
|---------|---------|----------|
| `git remote -v` | Ver remotos | Ver la agenda de contactos |
| `git remote add origin <url>` | Conectar con un remoto | Guardar un contacto |
| `git push -u origin main` | Subir y enlazar rama | Enviar el paquete |
| `git push` | Subir commits | Enviar el paquete |
| `git fetch` | Descargar sin mezclar | Revisar el buzón |
| `git pull` | Descargar y mezclar | Revisar el buzón y archivar las cartas |

## Referencias rápidas
- `HEAD` → dónde estás ahora · `HEAD~1` → un commit atrás · `HEAD~3` → tres atrás
- `origin` → remoto por defecto · `upstream` → repo original (en forks)
- `main` → rama principal

## Flujo diario
```bash
git pull
git switch -c feat/mi-cambio
# ... editar ...
git status
git add .
git commit -m "feat: descripción clara"
git push -u origin feat/mi-cambio
# → abrir Pull Request en GitHub
```
