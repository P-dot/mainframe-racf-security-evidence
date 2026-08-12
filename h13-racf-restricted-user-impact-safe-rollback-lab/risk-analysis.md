# risk-analysis.md — H13 Risk Analysis

## Riesgo que se quería controlar

`H7USER` podía leer `PUBLIC.DATA` por `UACC(READ)`. Un usuario `RESTRICTED` no debería poder apoyarse en accesos generales como `UACC` o `ID(*)`.

## Riesgo observado

La activación de `RESTRICTED` puede cortar accesos necesarios para operar una sesión interactiva completa. Esto puede producir indisponibilidad funcional para el usuario antes incluso de llegar al recurso de negocio que se quería probar.

## Impacto en producción

Aplicar `RESTRICTED` sin análisis previo puede romper:

- logon TSO,
- inicialización ISPF,
- acceso a procedimientos de logon,
- lectura de librerías compartidas,
- paneles o clists/rexx de sesión,
- herramientas de operación que dependen de accesos generales.

## Severidad en laboratorio

Media. Se aplicó a un usuario de prueba y con rollback disponible.

## Severidad potencial en producción

Alta si se aplica sobre usuarios interactivos o técnicos sin análisis de dependencias.
