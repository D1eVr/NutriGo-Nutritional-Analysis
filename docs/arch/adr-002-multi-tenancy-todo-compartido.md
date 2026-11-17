# ADR-002 · Software como servicio con multi-tenancy todo compartido

| Campo | Valor |
|---|---|
| **Estado** | Aceptada |
| **Fecha de la decisión** | 13/10/2026 (Actividad 1) |
| **Registrada** | 17/11/2026 |
| **Autor** | Diego Valdovinos Rodríguez |
| **Diagramas** | [C4 nivel 2](diagramas.md), [modelo de datos](diagramas.md), [diagrama de clases](diagrama-de-clases.md) |

## Contexto

NutriGo! es software como servicio: hay una sola instalación que se opera desde el servidor y todos la usan por internet. El paciente solo descarga la app.

No hay clínicas. Cada paciente crea su propia cuenta, y sus registros, su progreso y su plan solo son suyos, así que **cada paciente es un inquilino**. No es un número fijo: la idea es llegar a cientos o miles.

El nutriólogo no es otro inquilino. Es alguien a quien el paciente le presta la llave, y su lista "Mis pacientes" junta a muchos inquilinos a la vez.

El requisito que manda es el **RNF-SEG-002**: un nutriólogo solo puede ver a los pacientes que lo autorizaron, y el 100 % de los intentos de ver a otro paciente se rechazan. Ningún requisito pide separar físicamente los datos.

## Decisión

**Todo compartido:** una sola base de datos, las mismas tablas para todos los pacientes y cada dato marcado con el paciente al que pertenece.
- En la base, cada tabla de datos del paciente lleva su `usuario_id` o `paciente_id`.
- En el diagrama de clases, cada RegistroDiario, Cita, PlanAlimenticio, Dispositivo y ResumenMensual sabe de qué paciente es. Por eso las asociaciones van de esas clases hacia Paciente.
- La regla de quién puede ver a quién vive en un solo lugar: `Autorizacion.permite(nutriologo)`, dentro del módulo Cuentas.

## Alternativas consideradas

- **Una base de datos por paciente.** Serían miles de bases que respaldar y migrar para una app que hoy ni siquiera cobra. Además, "Mis pacientes" tendría que buscar en muchas bases para armar una sola lista.
- **Un esquema por paciente.** Tiene el mismo problema de la lista del nutriólogo, y cada cambio a una tabla se aplicaría miles de veces.

## Consecuencias

### Positivas

- **Una sola migración y un solo respaldo** para todos los pacientes.
- **"Mis pacientes" es una sola consulta,** filtrada por las autorizaciones del nutriólogo (RF-025).
- **Es el modelo más barato de operar,** que es lo que conviene mientras la app no cobra.

### Negativas

- **El riesgo más grande del sistema.** Si una sola consulta olvida filtrar por paciente, un nutriólogo o un paciente puede ver datos de salud de todos los demás. La base de datos no lo impide: lo impide el código. Por eso la regla vive en un solo lugar y el RNF-SEG-002 se prueba con el 100 % de los intentos.
- **No se puede aislar a un paciente.** Si alguien pidiera que sus datos estén separados físicamente o restaurar solo su información, este modelo no lo permite sin rehacer la base.
- **Todos comparten la misma base,** así que una consulta pesada de un paciente le quita rendimiento a los demás.
