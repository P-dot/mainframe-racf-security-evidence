# evidence-walkthrough.md — H13 Evidence Walkthrough

La evidencia del documento muestra el cambio de enfoque del laboratorio:

1. Se parte de `H7USER`, una identidad de laboratorio no privilegiada creada en H7.
2. Se activa `RESTRICTED` sobre esa identidad.
3. El comportamiento observado impide continuar con las pruebas normales de Browse sobre datasets.
4. La conclusión no es que el comando falle, sino que el control es demasiado restrictivo para un usuario interactivo que aún depende de accesos generales.
5. El cierre correcto es documentar el impacto y aplicar `NORESTRICTED` si no se ha aplicado ya.

La hoja de contacto `evidence/h13_contact_sheet.jpg` permite revisar visualmente la secuencia completa de capturas.
