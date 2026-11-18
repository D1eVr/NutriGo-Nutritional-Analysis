# ADR-003 · Tareas programadas en un proceso aparte

| Campo | Valor |
|---|---|
| **Estado** | Aceptada |
| **Fecha de la decisión** | 28/10/2026 (diagramas C4) |
| **Registrada** | 17/11/2026 |
| **Autor** | Diego Valdovinos Rodríguez |
| **Diagramas** | [C4 nivel 2](diagramas.md), [diagrama de secuencia](diagrama-de-secuencia.md) |

## Contexto

Hay tres trabajos que nadie pide en el momento y que el sistema tiene que hacer solo:
- **Descargar las mediciones** de los servicios de los fabricantes (RF-012). En la entrevista, el paciente pidió capturar a mano solo lo que ningún dispositivo mide, así que el peso tiene que estar guardado antes de que abra la app.
- **Generar el plan de cada lunes** para todos los pacientes con los registros de la semana anterior (RF-016, RF-017). Es el momento más pesado de toda la semana.
- **Sustituir los registros de más de 30 días** por el resumen mensual (RF-027).

Mientras tanto, la API tiene que contestar rápido a los pacientes, porque registrar el día debe tomar menos de 2 minutos (RNF-USA-001).

## Decisión

Un contenedor aparte, **Tareas programadas** (Node.js con cron), que corre los tres trabajos a su hora. Usa el mismo código de los módulos que la API, porque es parte del mismo monolito modular (ADR-001), pero se ejecuta como otro proceso y escribe en la misma base de datos.

## Alternativas consideradas

- **Hacer el trabajo cuando el paciente lo pide.** Por ejemplo, generar el plan la primera vez que el paciente lo abre el lunes. El paciente esperaría a que se genere, y el peso de la báscula no estaría listo al abrir el registro.
- **Correr el cron dentro del proceso de la API.** El pico de los lunes quedaría dentro del mismo proceso que atiende a los pacientes, que es justo lo que se quiere evitar.
- **Una cola de mensajes con varios trabajadores.** Resuelve el mismo problema, pero agrega una pieza más que operar, y una sola persona no lo necesita con este volumen.

## Consecuencias

### Positivas

- **El trabajo pesado de los lunes no frena a la API** mientras los pacientes registran su día.
- **El peso ya está guardado cuando el paciente abre la app,** como se ve en el diagrama de secuencia.
- **Si un trabajo falla, se puede volver a correr** sin afectar a los pacientes que están usando la app.

### Negativas

- **Es otro proceso que desplegar y vigilar.** Si se detiene sin avisar, el lunes no hay planes nuevos y los dispositivos dejan de mandar datos, y nadie se entera hasta que un paciente se queja.
- **Los datos de los dispositivos no son en tiempo real.** Llegan cuando corre la tarea, no cuando el paciente se pesa.
- **Dos procesos escriben en las mismas tablas.** Si una sincronización llega mientras el paciente corrige un dato, tiene que ganar la corrección (RF-010). Esa regla se cuida en `RegistroDiario.recibirDato()`, que no pisa un dato corregido a mano.
- **La base de datos sigue siendo una sola.** El pico de los lunes ya no está en la API, pero sigue cargando la misma base que usan los pacientes.

## Cambios pendientes

La solicitud de cambio [SC-001](../cambios/sc-001-recordatorio-para-registrar-el-dia.md) agregaría un cuarto trabajo, revisar los recordatorios, con las mismas consecuencias de este registro.
