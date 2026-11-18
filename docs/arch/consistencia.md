# Taller de consistencia · NutriGo!

| Campo | Valor |
|---|---|
| **Sistema** | NutriGo! |
| **Autor** | Diego Valdovinos Rodríguez |
| **Versión** | 1.0 |
| **Fecha** | 17/11/2026 |
| **Fuentes** | Todos los diagramas de `docs/arch/` y `docs/diagramas/`, `docs/especificacion-requisitos.md`, `docs/casos de uso/`, Actividad 2 |

Cada diagrama suelto se veía bien. Este documento revisa que todos cuenten la misma historia, con las tres reglas del taller y la cuarta que amarra con la Unidad 3. Para cada regla está lo que se revisó, lo que se encontró y cómo quedó corregido.

**Resumen:**

| Regla | Inconsistencias encontradas | Corregidas |
|---|---|---|
| 1 · Clases contra secuencia | 5 | 5 |
| 2 · Clases contra estados | 3 | 3 |
| 3 · Todo contra la Unidad 2 | 3 diagramas y 2 requisitos que faltan | 3 diagramas. Los 2 requisitos se dejaron como pendientes, porque agregarlos es un cambio que hay que pedir. |
| 4 · C4 contra paquetes contra Actividad 2 | 4 | 4 |

---

## Regla 1 · Clases contra secuencia

> Cada mensaje que entra en una pieza tiene que existir como operación en la clase que lo recibe.

Se revisaron los 11 mensajes que reciben objetos en [diagrama-de-secuencia.md](diagrama-de-secuencia.md). La tabla mensaje por mensaje está en ese archivo. Faltaban cinco cosas:

| # | Inconsistencia | Corrección en el diagrama de clases |
|---|---|---|
| 1 | La app le pide al registro qué datos faltan y RegistroDiario no tenía esa operación. | Se agregó `+datosFaltantes() List~string~`. |
| 2 | La app le manda un alimento al registro y no había operación para recibirlo. | Se agregó `+agregarAlimento(nombre, tiempoComida)`. |
| 3 | `recibirDato()` devolvía `void`, pero el `alt` necesita saber si el valor se aceptó. | Ahora devuelve `bool`. |
| 4 | `guardar()` devolvía `void`, pero la app muestra en qué estado quedó el día. | Ahora devuelve `EstadoRegistro`. |
| 5 | El estado que regresa `guardar()` era un `string` libre. | Se volvió la enumeración `EstadoRegistro`. |

---

## Regla 2 · Clases contra estados

> Si dibujaste una máquina de estados para una clase, esa clase tiene que tener un atributo que guarde en qué estado está, y sus valores son exactamente los estados del diagrama.

Las tres clases con atributo de estado tienen su máquina en [estados-y-actividad.md](estados-y-actividad.md):

| Clase | Atributo | Valores de la enumeración | Estados del diagrama | ¿Coinciden? |
|---|---|---|---|---|
| RegistroDiario | `estado: EstadoRegistro` | SinRegistro, Incompleto, Completo | SinRegistro, Incompleto, Completo | Sí |
| Cita | `estado: EstadoCita` | Pendiente, Confirmada, Rechazada, Cancelada | Pendiente, Confirmada, Rechazada, Cancelada | Sí |
| Cuenta | `estado: EstadoCuenta` | Activa, Desactivada | Activa, Desactivada | Sí |

Antes de la revisión había tres problemas:
- **Los tres atributos eran `string`.** Admitían cualquier palabra, así que no había forma de que coincidieran exactamente con un diagrama. Se volvieron enumeraciones.
- **La máquina del registro de la Unidad 3 tenía un estado de más,** EnResumenMensual, que no era ningún valor del atributo. Cuando el registro se suma al resumen deja de existir, así que se volvió la salida al estado final.
- **Cita y Cuenta tenían atributo de estado y ninguna máquina.** Se dibujaron las dos.

Además, cada evento de las máquinas es una operación de la clase, y cada guarda y acción usa algo que ya existe:

| Máquina | Evento, guarda o acción | Operación |
|---|---|---|
| Registro | recibirDato, guardar, corregir | `RegistroDiario.recibirDato()`, `guardar()`, `corregir()` |
| Registro | [ya no falta ningún dato] | `RegistroDiario.estaCompleto()` |
| Registro | / recalcular el IMC del día | `RegistroDiario.imcDelDia()` |
| Registro | / sumarse al resumen mensual | `ResumenMensual.generar(registros)` |
| Cita | solicitar, aceptar, rechazar, cancelar | `Cita.solicitar()`, `aceptar()`, `rechazar()`, `cancelar()` |
| Cita | [el espacio sigue libre] / apartar / liberar el espacio | `HorarioDeAtencion.estaLibre()`, `apartar()`, `liberar()` |
| Cita | / avisar al paciente | `Cita.anunciarCambio()` y el Notificador |
| Cuenta | desactivar / anunciar la desactivación | `Cuenta.desactivar()` |

---

## Regla 3 · Todo contra la Unidad 2

> Cada diagrama tiene que poder rastrearse hasta un caso de uso o un requisito, en los dos sentidos.

### De cada diagrama a su requisito

| Diagrama | Archivo | Se rastrea hasta |
|---|---|---|
| Casos de uso | `docs/diagramas/casos-de-uso.drawio` | CU-01 a CU-20, que realizan RF-001 a RF-033 |
| C4 niveles 1 a 3 | `diagramas.md` | Visión del producto; RF-012 (fabricantes), RF-004 y RF-028 (correo), RF-033 (notificaciones) |
| Clases | `diagrama-de-clases.md` | `clases-candidatas.md`, que saca cada clase de los RF |
| Objetos | `diagrama-de-clases.md` | El caso de Ana del prototipo: RF-007, RF-010, RF-012, RF-015, RF-016, RF-020, RF-023 |
| Paquetes y componentes | `paquetes-y-componentes.md` | Cada módulo con su rango de RF, en la tabla del inicio de ese archivo |
| Secuencia | `diagrama-de-secuencia.md` | CU-07 (RF-007, RF-008, RF-012, RNF-CON-002) |
| Estados del registro | `estados-y-actividad.md` | CU-07 y CU-08 (RF-007, RF-009, RF-010, RF-027, RF-031, RNF-CON-001) |
| Estados de la cita | `estados-y-actividad.md` | CU-14, CU-15, CU-16 y CU-20 (RF-020, RF-021, RF-022, RF-029, RF-033) |
| Estados de la cuenta | `estados-y-actividad.md` | CU-01, CU-19 y CU-20 (RF-001, RF-028, RF-029) |
| Actividad | `estados-y-actividad.md` | CU-07 (RF-007, RF-008, RF-009, RF-031, RNF-USA-001) |
| Modelo de datos | `diagramas.md` | RF-007, RF-027, RNF-CON-001 y las clases del diagrama de clases |

Ningún diagrama quedó sin requisito.

### Lo que se encontró

- **El RF-033 avisa al teléfono y C4 no lo mostraba.** Los tres niveles solo tenían el servicio de correo. Se agregó el servicio de notificaciones del teléfono al contexto, a los contenedores y a los componentes, como salida del Notificador.
- **El modelo de datos seguía el modelo de la Unidad 3.** Tenía la estatura en metros, la autorización como un campo de la cita y la cita sin su horario. Se alineó con el diagrama de clases: `estatura_cm`, una tabla AUTORIZACION y `horario_id` en CITA.
- **Las secciones 4, 5 y 7 de `diagramas.md` eran versiones anteriores.** Se movieron a sus archivos de la Unidad 4 y en su lugar quedó una nota que apunta al diagrama vigente.

### Al revés: lo que los diagramas piden y los requisitos no tienen

Dos máquinas de estados encontraron un requisito que nunca se escribió. **No se agregaron a los diagramas**, porque un estado sin requisito sería justo la inconsistencia que esta regla busca. Quedan como candidatos a una solicitud de cambio:

| Hallazgo | Diagrama | Qué falta |
|---|---|---|
| Una cita Confirmada nunca termina. Después de la consulta sigue Confirmada y se puede cancelar. | Estados de la cita | Un requisito que diga qué pasa después de la consulta, por ejemplo un estado Atendida. |
| Una cuenta Desactivada no se puede reactivar. Si el administrador se equivoca, la persona no vuelve a entrar. | Estados de la cuenta | Un requisito de reactivación de cuentas, o confirmar con el cliente que no hace falta. |

### Clases que no aparecen en ningún diagrama de comportamiento

Es el caso 3 del ejercicio del taller. PerfilAlimenticio, PlanAlimenticio y Autorizacion no reciben ningún mensaje en la secuencia ni tienen máquina de estados. **No sobran:** las tres se rastrean a requisitos imprescindibles (RF-015, RF-016, RF-017, RF-023, RF-025) y aparecen en el diagrama de objetos. Lo que falta es un escenario. Los siguientes diagramas de secuencia por dibujar son:
- **La generación del plan de cada lunes (RF-016 y RF-017).** Al dibujarlo habrá que contestar algo que hoy ningún diagrama dice: de dónde salen los alimentos candidatos que `PerfilAlimenticio.admite()` revisa.
- **La consulta del progreso de un paciente por su nutriólogo (RF-025),** donde Autorizacion decide con `permite()`.

---

## Regla 4 · C4 contra paquetes contra la Actividad 2

> Los componentes del nivel 3 de C4 deberían ser los mismos paquetes del diagrama de paquetes y los mismos módulos de la Actividad 2.

### Cómo estaba

El nivel 3 de C4 tenía diez componentes (Cuentas y sesión, Registro diario, Dispositivos, Progreso, Plan alimenticio, Citas, Informes, Administración, Conector de dispositivos y Notificador). La Actividad 2 tenía cuatro módulos. No había diagrama de paquetes. Se cambió de opinión en algún punto y no se actualizó lo anterior.

### Cómo quedó

| Actividad 2 | C4 nivel 3 | Paquete | Componente UML | Lo que absorbió del C4 anterior |
|---|---|---|---|---|
| Cuentas | Cuentas | Cuentas | Cuentas | Cuentas y sesión, Administración |
| Seguimiento | Seguimiento | Seguimiento | Seguimiento | Registro diario, Dispositivos, Progreso, Conector de dispositivos |
| Plan alimenticio | Plan alimenticio | Plan alimenticio | Plan alimenticio | Plan alimenticio |
| Consultas | Consultas | Consultas | Consultas | Citas |
| — | Notificador | Común | Notificador | Notificador |

Los cuatro módulos son los mismos en los cuatro lugares. El Notificador es la única pieza extra, y no es un módulo de negocio sino infraestructura compartida. La Actividad 2 no lo mencionaba porque agrupaba las clases del dominio.

Informes fue el único componente que no cupo en ningún módulo. Junta indicadores de Seguimiento con planes de Plan alimenticio, así que dentro de Seguimiento cerraba un ciclo. El PDF del RF-026 lo arma la app pidiéndole cada parte a su módulo, y el resumen mensual del RF-027 se quedó en Seguimiento solo con los indicadores.

### Las dependencias tampoco coincidían

Con las flechas del diagrama de clases 1.0, los paquetes tenían tres ciclos con Cuentas. La Actividad 2 decía que Cuentas no le habla a nadie. Se corrigió la navegabilidad en el diagrama de clases y ahora las dependencias son exactamente las de la Actividad 2:

| La Actividad 2 decía | Diagrama de paquetes |
|---|---|
| Seguimiento habla con Cuentas para la estatura y los permisos | Seguimiento → Cuentas |
| Plan alimenticio habla con Seguimiento para los días con registro | Plan alimenticio → Seguimiento |
| Plan alimenticio habla con Cuentas para los datos personales | Plan alimenticio → Cuentas |
| Consultas habla con Cuentas para los nutriólogos activos y el permiso | Consultas → Cuentas |
| Cuentas no le habla a nadie | Cuentas no depende de ningún módulo; solo usa el Notificador de Común |

El detalle de cada ciclo y cómo se rompió está en [paquetes-y-componentes.md](paquetes-y-componentes.md).

---

## Cómo afecta esto a la solicitud de cambio SC-001

El análisis de impacto [ai-001](../cambios/ai-001-recordatorio-para-registrar-el-dia.md) se escribió el 12/11/2026, antes de este taller. Se revisó contra los diagramas nuevos:
- Su recomendación y su estimación no cambian.
- El servicio de notificaciones del teléfono ya aparece en C4 por el RF-033, así que el recordatorio ya no agrega ese sistema externo; solo lo usa.
- Las referencias a los diagramas 4, 5 y 7 de `diagramas.md` ahora apuntan a sus archivos nuevos.
