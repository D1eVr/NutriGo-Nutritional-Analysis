# Análisis de impacto de SC-001

---

## Solicitud que se analiza

[`sc-001-recordatorio-para-registrar-el-dia.md`](sc-001-recordatorio-para-registrar-el-dia.md), pedida por Jimena Morales, dupla de revisión, el 12/11/2026.

## Traducción a términos del sistema

Lo pedido: "si ya es tarde y no he registrado mi día, que la app me mande un recordatorio al celular, y poder elegir la hora o apagarlo".

Traducido a NutriGo:

- **Un requisito funcional nuevo, el RF-034.** El sistema envía una notificación al teléfono del paciente a la hora que él eligió, solo si a esa hora su RegistroDiario de hoy no está completo.
- **Un caso de uso nuevo, el CU-21 "Configurar el recordatorio del registro diario".** Es del Paciente y se inicia desde su perfil.
- **La clase Paciente cambia:**
  - Gana dos atributos: la hora del recordatorio y si está activo.
  - Gana una operación para configurarlos.
  - Para saber si el día está completo usa `estaCompleto()`, que RegistroDiario ya tiene.
- **El contenedor Tareas programadas tiene un trabajo nuevo:** revisar qué pacientes tienen un recordatorio a esa hora y no han completado su día.
- **El aviso sale por el mismo canal que ya pide el RF-033** (notificaciones al teléfono cuando el nutriólogo responde una cita). El componente Notificador ya tiene en el diseño la salida al teléfono que pide el RF-033, pero todavía no está construida.

## Impacto directo

| Artefacto | Unidad | Qué hay que cambiar |
|---|---|---|
| Requisito RF-034 (nuevo) en `especificacion-requisitos.md` | 2 | **Ficha nueva:**<ul><li>Nombre: "Recordatorio para registrar el día".</li><li>Prioridad: importante.</li><li>Origen: la solicitud SC-001 y el hallazgo de la entrevista (22/09/2026) de que el olvido lleva al abandono.</li><li>Criterio de aceptación: el de la solicitud, con las 21:00 como ejemplo.</li></ul>Agregar el renglón en la tabla resumen. |
| Caso de uso CU-21 (nuevo) en `docs/casos de uso/` | 2 | **Escenario:** el paciente abre su perfil, elige "Recordatorio", lo activa, elige la hora y guarda.<br>**Flujos alternos:**<ul><li>Lo desactiva.</li><li>El teléfono tiene bloqueadas las notificaciones de NutriGo: la app lo avisa y el caso regresa a la configuración.</li></ul>**Realiza:** RF-034. |
| Diagrama de casos de uso (`casos-de-uso.drawio` y sus PNG) | 2 | Agregar CU-21 asociado al Paciente en la vista general y en la vista "Registro diario y dispositivos", y volver a exportar los PNG desde el mismo `.drawio`. |
| Prototipo en Figma, pantalla P07 Perfil | 2 | **P07 Perfil:** agregar la opción "Recordatorio".<br>**Pantalla nueva:** interruptor de activado y selector de hora. |
| Diagrama de clases (`diagrama-de-clases.md`) | 4 | **En Paciente:**<ul><li>Agregar `-horaRecordatorio: string`.</li><li>Agregar `-recordatorioActivo: bool`.</li><li>Agregar `+configurarRecordatorio(hora: string, activo: bool) void`.</li></ul>No hay relaciones nuevas: la revisión usa la asociación Paciente–RegistroDiario que ya existe. |
| Diagrama de secuencia | 4 | Agregar uno nuevo del envío del recordatorio: Tareas programadas busca a quién le toca, pregunta si el registro de hoy está completo, le pide al Notificador el envío y el servicio de notificaciones lo entrega al teléfono. Hoy no existe ninguno que cubra este flujo. |
| Modelo de datos (#8 de `diagramas.md`) | 3 | Agregar `hora_recordatorio` y `recordatorio_activo` a la tabla USUARIO. |

## Impacto indirecto

| Artefacto | Unidad | Por qué se ve afectado |
|---|---|---|
| Registro de decisión [ADR-003](../arch/adr-003-tareas-programadas.md), tareas programadas en un proceso aparte | 3 | Las tareas programadas ganan un cuarto trabajo, revisar los recordatorios. La decisión no cambia, pero hay que agregar el trabajo a su contexto. Una consecuencia negativa también pesa más: si el proceso se detiene sin avisar, ahora tampoco llegan los recordatorios. |
| Diagrama de contexto C4, nivel 1 (#1) | 3 | **Sin impacto.** El servicio de notificaciones del teléfono ya aparece desde la revisión de consistencia del 17/11/2026, porque el RF-033 lo necesitaba. Este cambio lo usa, pero no lo agrega. |
| Diagrama de contenedores C4, nivel 2 (#2) | 3 | Tareas programadas gana un cuarto trabajo: revisar los recordatorios, además de descargar mediciones, generar el plan de los lunes y armar el resumen mensual. Gana una flecha hacia el servicio de notificaciones, que hoy solo recibe avisos de la API. |
| Diagrama de componentes C4, nivel 3 (#3) | 3 | **Sin impacto.** El Notificador ya envía avisos al teléfono por el RF-033. El recordatorio es un aviso más por el mismo camino. |
| Diagramas de paquetes y de componentes UML (`paquetes-y-componentes.md`) | 4 | **Sin impacto.** Los atributos nuevos van en Paciente, dentro de Cuentas, y el aviso usa `notificar(paciente, mensaje)` de la interfaz INotificador, que ya existe. No aparece ninguna dependencia nueva entre paquetes. |
| Máquina de estados del registro diario (`estados-y-actividad.md`, sección 1) | 4 | **Sin impacto.** El recordatorio no cambia el estado del día: si el paciente no registra, el día sigue en SinRegistro, que es la regla del RNF-CON-001. |
| Diagrama de actividad de registrar el seguimiento diario (`estados-y-actividad.md`, sección 4) | 4 | **Sin impacto.** El recordatorio ocurre antes de que el paciente abra el registro, y el flujo de registrar no cambia. |
| Diagrama de secuencia del registro del día (`diagrama-de-secuencia.md`) | 4 | **Sin impacto.** Ese flujo es el mismo con o sin recordatorio. |
| Diagrama de objetos (`diagrama-de-clases.md`) | 4 | El objeto `ana : Paciente` debe mostrar los dos atributos nuevos, por ejemplo `horaRecordatorio = "21:00"` y `recordatorioActivo = true`, para que el caso siga representando a la clase completa. |
| Clases candidatas (`clases-candidatas.md`) | 4 | El sustantivo nuevo "recordatorio" se descarta como atributo de Paciente: guarda una hora y si está activo, y no tiene comportamiento propio. Hay que anotarlo en la tabla "Es atributo". |
| RNF-SEG-001, acceso solo con sesión iniciada | 2 | **Sigue siendo cierto con una condición.** El recordatorio aparece en la pantalla bloqueada sin sesión, así que no puede incluir ningún dato de salud. Hay que agregarlo a su métrica como caso de prueba. |
| RNF-USA-001, tiempo para registrar el día | 2 | **Sin impacto.** Abrir el registro desde el recordatorio no cambia los pasos para capturar el día. |
| Trazabilidad y registro de cambios de la especificación | 2 | **Trazabilidad:** agregar el renglón de RF-034 hacia CU-21 y hacia la pantalla nueva del prototipo.<br>**Registro de cambios:** agregar la entrada que cite SC-001. |
| Plan de pruebas (`calidad/Plan de Pruebas NutriGo - Diego Valdovinos.docx`) | 5 | Agregar casos de prueba:<ul><li>Día incompleto a la hora elegida: llega el recordatorio.</li><li>Día completo: no llega nada.</li><li>Recordatorio apagado: no llega nada.</li><li>Notificaciones bloqueadas en el teléfono.</li><li>Pantalla bloqueada sin datos de salud.</li></ul> |
| Visión del producto | 1 | **Sin impacto.** El alcance ya dice que el sistema registra los datos diariamente; el recordatorio apoya eso y no lo amplía. |

## Lo que queda sin tocar

- Las reglas del registro diario: rangos, guardar incompleto, corrección y los 30 días. Tampoco cambia la clase RegistroDiario, porque `estaCompleto()` ya existe.
- El plan alimenticio, el perfil alimenticio, las citas, los horarios de atención, las autorizaciones, los dispositivos y el resumen mensual.
- Las funciones del nutriólogo y del administrador. El recordatorio es solo del paciente y no cambia ningún rol.
- Los casos de uso CU-01 a CU-20 y sus flujos.
- No aparece ningún actor nuevo en el diagrama de casos de uso. El servicio de notificaciones es un sistema externo del contexto C4, no un usuario.
- No se agregan otros canales como SMS, WhatsApp o correo para el recordatorio.

## Riesgos

- **El teléfono tiene bloqueadas las notificaciones de NutriGo.** Probabilidad alta, porque mucha gente las niega la primera vez. El recordatorio no llega y el paciente cree que no funciona. Se mitiga con el flujo alterno del CU-21, que lo avisa en la configuración.
- **El dispositivo sincroniza después del recordatorio.** Probabilidad media. Si la báscula manda el peso a las 22:00 y el recordatorio salió a las 21:00, el paciente recibe un aviso de algo que su báscula iba a llenar sola. Se mitiga aclarando en el RF que el recordatorio considera solo lo que ya llegó a esa hora.
- **El paciente lo siente molesto y lo apaga o desinstala la app.** Probabilidad media. Por eso el criterio pide que se pueda apagar y que solo llegue un recordatorio por día.
- **Muchos pacientes eligen la misma hora.** Probabilidad baja hoy y alta si la app crece. Si miles eligen las 21:00, Tareas programadas tiene un pico como el de los lunes con el plan, que es la señal que anoté en la Actividad 2 para separar módulos.
- **No hacerlo.** El olvido que salió en la entrevista sigue sin atenderse, y los días sin registro afectan el progreso, el plan semanal y lo que ve el nutriólogo.

## Alternativas consideradas

1. **Notificación desde el servidor (Tareas programadas y el Notificador).** El servidor revisa a la hora elegida si el registro está completo y envía el aviso por el servicio de notificaciones del teléfono.
   - **Ventaja:** decide con los datos reales del servidor, incluidos los que ya mandó la báscula, y reutiliza el canal que de todos modos hace falta para el RF-033.
   - **Costo:** el adaptador de notificaciones y un trabajo programado nuevo.
   - **Pendiente:** depende de que el teléfono tenga conexión a esa hora.
2. **Notificación local programada en el teléfono.** La app agenda una alarma diaria en el propio celular y, al sonar, revisa lo que tiene guardado.
   - **Ventaja:** no toca el servidor ni los diagramas C4, y funciona sin conexión.
   - **Costo:** la app no sabe lo que llegó de la báscula al servidor después de la última vez que se abrió, así que puede avisar de algo que ya está completo, lo que rompe el criterio de aceptación. Además, la configuración vive en el teléfono y se pierde si el paciente cambia de celular.
3. **Recordatorio por correo.** Reutiliza el servicio de correo que ya existe.
   - **Ventaja:** es lo más barato.
   - **Costo:** un correo a las 21:00 casi nadie lo ve a tiempo, y la solicitud pide explícitamente que llegue al celular.

Me inclino por la **opción 1**. Es la única que cumple el criterio de aceptación con los datos reales del paciente, y su costo se comparte con el RF-033, que ya está en la especificación.

## Estimación de esfuerzo

| Concepto | Esfuerzo |
|---|---|
| Cambios de diseño y documentación | 5 horas |
| Implementación | 8 horas |
| Pruebas | 4 horas |
| **Total** | **17 horas** |

**De dónde sale este número.**

- **Diseño y documentación.** Son unos doce artefactos de las tablas de arriba. Casi todos son agregar un renglón o una línea, y calculé de 15 a 20 minutos por cada uno. Lo que más pesa es el caso de uso nuevo con sus flujos, volver a exportar los diagramas de casos de uso desde draw.io y la pantalla nueva en Figma.
- **Implementación.** Lo comparé con lo más parecido que ya está diseñado, la notificación del RF-033:
  - El adaptador del Notificador para el teléfono: unas 3 horas, que **solo se cuentan una vez**; si el RF-033 se construye primero, este cambio cuesta 3 horas menos.
  - El trabajo programado y su consulta: 2 horas.
  - La pantalla y el guardado de la configuración: 2 horas.
  - Abrir el registro de hoy al tocar el aviso: 1 hora.
- **Pruebas.** Son los cinco casos que se agregan al plan de pruebas, más probar en un teléfono real con las notificaciones bloqueadas y con la pantalla bloqueada.
- **Lo que no incluí.** Configurar la cuenta del servicio de notificaciones del teléfono, que es una sola vez para todo el proyecto, y la revisión de la dupla.

Supuse que lo hace una sola persona, que soy yo, con la arquitectura de monolito modular que ya está decidida.

## Recomendación

**Aceptar, con prioridad media**, y programarlo **después del RF-033**:
- Ataca directamente el olvido y el abandono que salieron en la entrevista.
- No toca ninguna regla que ya funcione.
- Su riesgo más alto, las notificaciones bloqueadas, se puede manejar con un flujo alterno.
- Hacerlo después del RF-033 lo deja en unas 14 horas, porque el canal de notificaciones al teléfono ya estará construido.

Antes de cerrarlo hay que confirmar con la dupla una sola cosa: si le parece bien que el recordatorio solo considere lo que ya llegó al servidor a esa hora.

---

*El objetivo de este documento no es dar un número exacto. Es que cualquiera pueda seguir tu razonamiento y señalarte dónde te equivocaste.*
