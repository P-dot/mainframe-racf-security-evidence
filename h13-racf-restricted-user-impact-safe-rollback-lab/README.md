# H13 — RACF Restricted User Impact & Safe Rollback Lab

## Objetivo

Validar de forma controlada el impacto del atributo `RESTRICTED` sobre una identidad de prueba RACF (`H7USER`) en el entorno ADCD z/OS 1.11 sobre Hercules.

El objetivo inicial era comprobar que un usuario `RESTRICTED` no podía aprovechar accesos generales como `UACC(READ)`, pero que sí podría seguir usando permisos explícitos. La práctica real mostró un resultado más importante: al aplicar `RESTRICTED`, `H7USER` no pudo completar el flujo interactivo normal de TSO/ISPF, por lo que el laboratorio se cerró como análisis de impacto y rollback seguro.

## Resultado técnico

- `H7USER` funcionaba previamente como usuario no privilegiado de laboratorio.
- Se aplicó el atributo `RESTRICTED` únicamente a `H7USER`.
- El acceso interactivo TSO/ISPF quedó afectado antes de poder repetir las pruebas de datasets.
- La conclusión operativa es que `H7USER` dependía de accesos generales para recursos necesarios durante el logon o arranque de ISPF.
- No se concedieron permisos masivos a librerías de sistema para “arreglar” el acceso.
- El control se documenta como útil, pero de implantación delicada.

## Conclusión principal

`RESTRICTED` no debe activarse sobre usuarios interactivos sin inventario previo de dependencias. Puede cortar accesos heredados por `UACC`, `ID(*)` o Global Access Checking, y eso puede afectar no solo al dataset que queremos probar, sino también a recursos de sesión como TSO, ISPF, procedimientos de logon o librerías necesarias para trabajar.

## Estado final esperado

El laboratorio debe quedar con `H7USER` sin `RESTRICTED` si se aplicó rollback:

```text
ALTUSER H7USER NORESTRICTED
LISTUSER H7USER
```

No se deben tocar usuarios técnicos, started tasks ni datasets críticos.
