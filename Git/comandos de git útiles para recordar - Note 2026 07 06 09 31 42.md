# comandos de git útiles para recordar - Note 2026 07 06 09 31 42

- ***git merge dev -X theirs*** (fusiona cambios desde la rama dev a la rama actual ignorando los cambios locales)
- ***git log --name-only --pretty=format: --max-count=20 | Where-Object { $_ -ne '' } | Select-Object -Unique***  (ver los últimos 20 archivos modificados).
- restear la rama actual a la rama origin/main ***git fetch origin*
*git reset --hard origin/main***
- ***git log HEAD..origin/main --oneline***  Esto te muestra los commits que están en `origin/main` y todavía no en tu rama local.
- ***git diff --name-only HEAD..origin/main*** 
Esto lista solo los nombres de los archivos que cambiaron entre tu rama local y la remota después del último `fetch`.
- ***git log --oneline --graph --decorate*** Esto muestra un historial compacto y visual de tus commits. Al hacer cntrl + clic en los id de los commits se ve una detalle del mismo con la extension gitlens inspect.
- ***git diff*** Esto te muestra las diferencias entre tu directorio de trabajo y el último commit.
- ***git diff --cached*** Así ves lo que realmente se va a subir en el próximo commit.
- ***git status*** Para ver un listado de archivos modificados 
- ***git diff --stat***  Para ver un resumen de diferencias (sin mostrar el contenido línea por línea)
- ***gitk*** Abre una interfaz visual para explorar commits y diffs.
- workflow de revision: 
  - 1° git status
  - 2° git diff
  - git diff --cached
  - git diff --stat
  - git log --oneline --graph --decorate
  - gitk
     
     
     
     
     
     
     
     
     

 