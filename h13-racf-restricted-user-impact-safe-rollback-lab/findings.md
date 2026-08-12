# findings.md — H13 Findings

## Finding 1 — RESTRICTED afecta antes de la prueba de datasets

El resultado relevante del laboratorio no fue una simple denegación sobre `PUBLIC.DATA`, sino que `H7USER` no pudo completar el uso interactivo normal después de aplicar `RESTRICTED`.

## Finding 2 — Dependencia de accesos generales

La hipótesis técnica más razonable es que el logon TSO/ISPF o su inicialización depende de recursos a los que `H7USER` accedía por mecanismos generales. Al activar `RESTRICTED`, esos accesos generales dejan de servir para conceder acceso.

## Finding 3 — No se debe corregir con permisos masivos

La respuesta incorrecta habría sido conceder `READ` amplio sobre `SYS1.*`, `ADCD.Z111S.*`, librerías ISPF o datasets de sistema. Eso habría convertido una prueba de hardening en una apertura peligrosa.

## Finding 4 — El lab se cierra como impacto y rollback

El aprendizaje profesional es que `RESTRICTED` es útil para identidades compartidas, anónimas, filtradas por certificado o de bajo privilegio, pero exige inventario previo si se pretende usar en identidades interactivas.
