# attack-scenario.md — H13 Controlled Abuse Scenario

## Escenario defensivo/ofensivo controlado

Un usuario de bajo privilegio puede aprovechar permisos generales como `UACC(READ)` para leer recursos que no aparecen en su access list. El atributo `RESTRICTED` sirve para impedir ese tipo de acceso general.

## Hipótesis inicial

```text
H7USER normal       -> puede leer PUBLIC.DATA por UACC(READ)
H7USER RESTRICTED  -> no debería leer PUBLIC.DATA
H7USER RESTRICTED  -> debería conservar accesos explícitos
```

## Resultado real

El control tuvo impacto antes de la prueba de datasets: `H7USER` no pudo completar el flujo interactivo normal.

## Valor del resultado

El resultado enseña una lección realista: un control defensivo puede estar bien diseñado conceptualmente y aun así necesitar análisis operativo antes de implantarse.
