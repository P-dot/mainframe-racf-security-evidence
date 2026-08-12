# commands.md — H13 RACF Restricted User Impact & Safe Rollback Lab

## Comandos de consulta inicial

```text
LISTUSER H7USER
LISTDSD DA('IBMUSER.SECLAB.PUBLIC.*') ALL
LISTDSD DA('IBMUSER.SECLAB.GRANTED.*') ALL
LISTDSD DA('IBMUSER.SECLAB.PRIVATE.*') ALL
```

Estos comandos solo consultan. Sirven para confirmar que `H7USER` es una identidad de prueba y que los perfiles del sandbox siguen teniendo el estado esperado.

## Comando de cambio probado

```text
ALTUSER H7USER RESTRICTED
```

Este comando modifica el perfil de usuario de `H7USER` y cambia el método de comprobación de accesos. No debe aplicarse a `IBMUSER`, usuarios técnicos, started tasks ni IDs del sistema.

## Verificación

```text
LISTUSER H7USER
```

La salida debe mostrar el atributo `RESTRICTED` si el cambio se aplicó.

## Resultado observado

Tras aplicar `RESTRICTED`, el usuario de laboratorio no pudo completar la sesión interactiva normal para repetir las pruebas de datasets. Esto indica dependencia de accesos generales durante el proceso de logon/ISPF.

## Rollback seguro

```text
ALTUSER H7USER NORESTRICTED
LISTUSER H7USER
```

Este rollback devuelve a `H7USER` al modo normal de access checking.

## Comandos que NO se ejecutan en este lab

```text
ALTUSER IBMUSER RESTRICTED
ALTUSER START1 RESTRICTED
ALTUSER START2 RESTRICTED
ALTUSER TCPIP RESTRICTED
PERMIT SYS1.* ID(H7USER) ACCESS(READ)
PERMIT ADCD.Z111S.* ID(H7USER) ACCESS(READ)
PERMIT ISP.* ID(H7USER) ACCESS(READ)
PERMIT ISF.* ID(H7USER) ACCESS(READ)
```

No se compensó el impacto de `RESTRICTED` abriendo librerías del sistema.
