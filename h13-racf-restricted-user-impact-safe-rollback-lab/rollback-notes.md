# rollback-notes.md — H13 Rollback Notes

## Rollback obligatorio si H7USER queda afectado

Desde `IBMUSER` en ISPF opción 6:

```text
ALTUSER H7USER NORESTRICTED
LISTUSER H7USER
```

## Verificación

`LISTUSER H7USER` no debe mostrar `RESTRICTED`.

Después se puede validar logon de `H7USER` y acceso esperado a:

```text
IBMUSER.SECLAB.PUBLIC.DATA
IBMUSER.SECLAB.GRANTED.DATA
```

## No usar como rollback

```text
PERMIT SYS1.* ID(H7USER) ACCESS(READ)
PERMIT ADCD.Z111S.* ID(H7USER) ACCESS(READ)
```

Eso no es rollback; es abrir superficie de ataque.
