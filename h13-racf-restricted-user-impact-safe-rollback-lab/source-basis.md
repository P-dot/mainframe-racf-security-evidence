# source-basis.md — H13 Source Basis

## RACF Security Administrator's Guide

La guía RACF indica que los usuarios restringidos no pueden usar accesos generales para acceder a recursos protegidos. La autorización de IDs restringidos omite Global Access Checking, y no usa `UACC` ni `ID(*)` para conceder acceso.

También documenta que `ADDUSER` y `ALTUSER` permiten definir o cambiar access checking con `RESTRICTED` y `NORESTRICTED`.

## Dattani — IBM Mainframe Security: Beyond the Basics

Dattani explica `RESTRICTED` como un atributo que no concede poder, sino que evita que el usuario use:

- Universal Access (`UACC`),
- acceso general `ID(*)`,
- Global Access Checking.

La única vía válida para el usuario restringido es permiso explícito por usuario o por grupo.

## Evidencia local

La evidencia local del lab muestra que, en este entorno ADCD/Hercules, aplicar `RESTRICTED` a un usuario interactivo puede afectar el logon/uso de TSO/ISPF antes de poder probar datasets individuales.
