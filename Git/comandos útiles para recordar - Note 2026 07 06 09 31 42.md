# comandos útiles para recordar - Note 2026 07 06 09 31 42

- ***git merge dev -X theirs*** (fusiona cambios desde la rama dev a la rama actual ignorando los cambios locales)
- git log --name-only --pretty=format: --max-count=20 | Where-Object { $_ -ne '' } | Select-Object -Unique  (ver los últimos 20 archivos modificados).

 