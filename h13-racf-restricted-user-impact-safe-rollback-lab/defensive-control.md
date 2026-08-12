# defensive-control.md — H13 Defensive Control

## Control estudiado

`RESTRICTED` en un perfil de usuario RACF.

## Para qué sirve

Evita que un usuario se beneficie de accesos generales. Esto es especialmente útil para identidades compartidas, públicas, anónimas o asignadas por mecanismos de filtrado, donde no queremos que el usuario herede permisos amplios.

## Qué no hace

No convierte al usuario en más privilegiado. Al contrario: limita accesos.

No sustituye una access list explícita bien diseñada.

No debe aplicarse sin probar dependencias de sesión, aplicaciones y procedimientos.

## Buen uso

- Usuarios compartidos o anónimos.
- Identidades de entrada externa.
- Usuarios que solo deben acceder a recursos explícitamente permitidos.

## Mal uso

- Aplicarlo a usuarios interactivos sin inventario.
- Aplicarlo a usuarios técnicos críticos.
- Compensar la rotura abriendo librerías de sistema.
