# Lab 09 y 10 — zFS Backing Dataset Protection + SAF Controls Review

Este laboratorio une dos revisiones que van juntas en una auditoría RACF de z/OS ADCD:

- **Lab 09:** revisión de protección RACF sobre datasets que respaldan filesystems zFS.
- **Lab 10:** revisión de controles SAF no-DATASET: `FACILITY`, `UNIXPRIV`, `SERVAUTH` y `STARTED`.

El objetivo no es endurecer todavía el sistema, sino **documentar el estado base** con evidencias de TSO/ISPF/SDSF.

## Contexto

Los labs anteriores mostraron tres hechos relevantes:

1. Muchos usuarios técnicos tienen `UID(0)` en OMVS.
2. Varias librerías sensibles `SYS1.*` y `ADCD.Z111S.*` no mostraron perfil RACF DATASET visible.
3. Hay zFS activos montados `RDWR`.

Por eso este lab revisa dos capas:

- la protección de los **backing datasets zFS**;
- la existencia o ausencia de controles granulares SAF, como `BPX.*`, `UNIXPRIV` y `EZB.*`.

## Evidencias incluidas

```text
lab-09-y-10-zfs-saf-controls-review/
├── evidence/
│   ├── screenshots/
│   │   ├── lab09/
│   │   └── lab10/
│   └── source-documents/
│       ├── lab9.docx
│       └── LAB10.docx
```

## Resultado ejecutivo

El entorno ADCD funciona correctamente como laboratorio, pero las evidencias muestran un patrón no endurecido:

```text
zFS activos RDWR
+
backing datasets zFS sin perfil RACF visible
+
ausencia de perfiles BPX.* / UNIXPRIV / EZB.* en las búsquedas realizadas
+
uso de usuarios técnicos potentes y UID(0) visto en labs previos
```

En producción, esto sería una línea clara de revisión de mínimo privilegio, separación de funciones, auditoría y controles SAF granulares.
