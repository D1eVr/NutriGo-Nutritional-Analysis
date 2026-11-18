# ADR-005 · El informe en PDF lo arma la app

| Campo | Valor |
|---|---|
| **Estado** | Aceptada |
| **Fecha de la decisión** | 17/11/2026 (taller de consistencia) |
| **Registrada** | 17/11/2026 |
| **Autor** | Diego Valdovinos Rodríguez |
| **Diagramas** | [C4 niveles 2 y 3](diagramas.md), [paquetes](paquetes-y-componentes.md), [consistencia](consistencia.md) |

## Contexto

El RF-026 pide un PDF con los indicadores y los registros del periodo, que son de Seguimiento, y con el plan alimenticio de ese periodo, que es de Plan alimenticio.

En la primera versión del nivel 3 de C4 había un componente Informes, y el resumen mensual del diagrama de clases incluía los planes del mes. Al dibujar el diagrama de paquetes apareció un ciclo:
- Plan alimenticio ya depende de Seguimiento, porque ajusta el plan con los registros de la semana (RF-017).
- Con el informe dentro de Seguimiento, Seguimiento también dependía de Plan alimenticio.

## Decisión

**La app arma el PDF.** Le pide los indicadores y los registros a Seguimiento y el plan del periodo a Plan alimenticio, arma el archivo en el teléfono y lo guarda en descargas.

El resumen mensual del RF-027 se queda en Seguimiento solo con los indicadores del mes.

## Alternativas consideradas

- **Informes dentro de Seguimiento.** Es como estaba y es lo que creaba el ciclo.
- **Un quinto módulo, Informes, arriba de los dos.** No crea ciclos, pero agrega un módulo que no existe en la Actividad 2 solo para juntar dos respuestas.
- **Informes dentro de Plan alimenticio.** No crea ciclos, pero mete en el módulo del plan algo que no tiene que ver con el plan.

## Consecuencias

### Positivas

- **Los paquetes quedan sin ciclos,** y los módulos son los mismos de la Actividad 2.
- **El servidor no guarda ni controla los PDF.** Es lo que ya decían las clases candidatas: el informe se arma en el momento y queda en el teléfono.

### Negativas

- **La lógica del informe queda en la app.** Si mañana hubiera una versión web, habría que volver a escribirla ahí.
- **El trabajo lo hace el teléfono.** En un teléfono viejo, un informe mensual con muchos registros puede tardar.
- **El formato del PDF depende de la versión de la app que tenga cada paciente,** así que dos pacientes pueden recibir informes distintos para el mismo periodo.
