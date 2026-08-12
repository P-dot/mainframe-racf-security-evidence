# hardening-notes.md — H13 Hardening Notes

## Lección de hardening

Antes de activar `RESTRICTED` en producción hay que descubrir qué necesita leer o usar el usuario para operar.

## Enfoque recomendado

1. Crear identidad de prueba.
2. Activar auditoría de fallos sobre recursos relevantes.
3. Aplicar `RESTRICTED` solo en prueba.
4. Identificar denegaciones necesarias.
5. Conceder permisos explícitos mínimos si procede.
6. Repetir prueba.
7. Documentar rollback.

## No hacer

No abrir datasets de sistema en bloque para hacer funcionar un usuario restringido.

## Conexión con labs anteriores

- H8 validó accesos reales con `H7USER`.
- H9 leyó violaciones RACF en SYSLOG.
- H10 gestionó acceso temporal.
- H12 limpió accesos residuales.
- H13 demuestra que los controles de identidad pueden afectar dependencias operativas previas.
