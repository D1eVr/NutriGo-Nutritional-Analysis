# De los requisitos a las clases

**Sistema:** NutriGo!
**Autor:** Diego Valdovinos Rodríguez
**Fuente:** Especificación de requisitos, versión 1.1

## 1. Sustantivos subrayados

Subrayé los sustantivos del alcance del sistema y de la descripción de cada requisito funcional, que es donde está dicho qué hace el sistema. Cuando dos palabras forman un solo nombre, como "plan alimenticio" o "peso en ayunas", las subrayé juntas. En este paso no filtré nada.

**Alcance del sistema**

NutriGo! es una <ins>aplicación</ins> móvil que permite a las <ins>personas</ins> llevar un <ins>seguimiento</ins> diario de su <ins>alimentación</ins> y de sus <ins>cambios físicos</ins>, consultar su <ins>progreso</ins> y recibir un <ins>plan alimenticio</ins> personalizado que se adapta a sus <ins>resultados</ins>. Registra el <ins>peso en ayunas</ins>, las <ins>horas de sueño</ins>, el <ins>agua</ins> consumida, la <ins>alimentación</ins> y la <ins>actividad física</ins> diariamente, ya sea de <ins>forma</ins> automática o manual, y muestra el <ins>progreso</ins> del <ins>usuario</ins> mediante su <ins>peso</ins>, <ins>IMC</ins>, <ins>porcentaje de grasa</ins> y <ins>porcentaje de músculo</ins>. Permite registrar <ins>preferencias</ins>, <ins>alergias</ins> y <ins>alimentos</ins> que no le gustan al <ins>usuario</ins> para generar un <ins>plan alimenticio</ins> personalizado según sus <ins>datos</ins>, <ins>objetivos</ins>, <ins>preferencias</ins> y <ins>restricciones</ins>, y ajusta ese <ins>plan</ins> cada <ins>semana</ins> según los <ins>resultados</ins> registrados. Se conecta con <ins>dispositivos</ins> existentes compatibles, como <ins>relojes inteligentes</ins>, <ins>bandas de actividad</ins> y <ins>básculas inteligentes</ins>, para recibir <ins>datos</ins> del <ins>usuario</ins>. Proporciona los <ins>horarios</ins> disponibles del <ins>nutriólogo</ins> para atender <ins>dudas</ins> o solicitar una <ins>cita</ins> remota, y permite al <ins>nutriólogo</ins> consultar el <ins>progreso</ins> de sus <ins>pacientes</ins> y atender sus <ins>solicitudes</ins> dentro de los <ins>horarios</ins> establecidos. También permite al <ins>usuario</ins> descargar en <ins>PDF</ins> sus <ins>avances</ins> y <ins>planes de alimentación</ins> de <ins>forma</ins> semanal o mensual.

**Requisitos funcionales**

- **RF-001.** El <ins>sistema</ins> crea la <ins>cuenta</ins> de un <ins>paciente</ins> con su <ins>nombre</ins>, <ins>correo electrónico</ins>, <ins>contraseña</ins>, <ins>fecha de nacimiento</ins>, <ins>sexo</ins> y <ins>estatura</ins> en <ins>centímetros</ins>.
- **RF-002.** El <ins>sistema</ins> da <ins>acceso</ins> a la <ins>aplicación</ins> a un <ins>usuario</ins> registrado que ingresa su <ins>correo electrónico</ins> y su <ins>contraseña</ins>.
- **RF-003.** El <ins>sistema</ins> impide el <ins>acceso</ins> a la <ins>aplicación</ins> cuando el <ins>correo</ins> no pertenece a una <ins>cuenta</ins> registrada, cuando la <ins>contraseña</ins> no corresponde o cuando la <ins>cuenta</ins> está desactivada.
- **RF-004.** El <ins>sistema</ins> envía al <ins>correo</ins> registrado del <ins>usuario</ins> un <ins>enlace</ins> para crear una nueva <ins>contraseña</ins>.
- **RF-005.** El <ins>sistema</ins> cierra la <ins>sesión</ins> del <ins>usuario</ins> cuando este lo solicita.
- **RF-006.** El <ins>sistema</ins> guarda los <ins>cambios</ins> que el <ins>paciente</ins> hace a su <ins>nombre</ins>, <ins>fecha de nacimiento</ins>, <ins>sexo</ins> y <ins>estatura</ins>.
- **RF-007.** El <ins>sistema</ins> registra para cada <ins>día</ins> el <ins>peso en ayunas</ins>, las <ins>horas de sueño</ins>, el <ins>consumo de agua</ins>, la <ins>alimentación</ins> y la <ins>actividad física</ins> del <ins>paciente</ins>.
- **RF-008.** El <ins>sistema</ins> habilita la <ins>captura</ins> manual de cualquier <ins>dato</ins> del <ins>registro diario</ins> que no se haya obtenido de un <ins>dispositivo</ins>.
- **RF-009.** El <ins>sistema</ins> registra la <ins>información</ins> del <ins>día</ins> actual aunque existan <ins>días</ins> anteriores sin <ins>registro</ins>.
- **RF-010.** El <ins>sistema</ins> reemplaza un <ins>dato</ins> registrado por el <ins>valor</ins> que el <ins>paciente</ins> capture como <ins>corrección</ins>.
- **RF-011.** El <ins>sistema</ins> vincula la <ins>cuenta</ins> del <ins>paciente</ins> con un <ins>reloj inteligente</ins>, una <ins>banda de actividad</ins>, una <ins>báscula inteligente</ins> u otro <ins>dispositivo</ins> compatible.
- **RF-012.** El <ins>sistema</ins> obtiene de los <ins>dispositivos</ins> vinculados el <ins>peso</ins>, el <ins>porcentaje de grasa</ins>, el <ins>porcentaje de músculo</ins>, las <ins>horas de sueño</ins> y la <ins>actividad física</ins> que cada <ins>dispositivo</ins> mida.
- **RF-013.** El <ins>sistema</ins> desvincula un <ins>dispositivo</ins> de la <ins>cuenta</ins> del <ins>paciente</ins> cuando este lo solicita.
- **RF-014.** El <ins>sistema</ins> muestra el <ins>peso</ins>, el <ins>IMC</ins>, el <ins>porcentaje de grasa corporal</ins> y el <ins>porcentaje de músculo</ins> del <ins>paciente</ins>, junto con su <ins>evolución</ins> durante la última <ins>semana</ins>, el último <ins>mes</ins> o el último <ins>año</ins>.
- **RF-015.** El <ins>sistema</ins> registra el <ins>objetivo</ins> del <ins>paciente</ins>, sus <ins>alergias</ins>, sus <ins>restricciones alimenticias</ins> y los <ins>alimentos</ins> que no le gustan.
- **RF-016.** El <ins>sistema</ins> genera un <ins>plan alimenticio semanal</ins> considerando los <ins>datos personales</ins>, el <ins>objetivo</ins>, las <ins>alergias</ins>, las <ins>restricciones</ins> y los <ins>alimentos</ins> que no le gustan al <ins>paciente</ins>.
- **RF-017.** El <ins>sistema</ins> genera el <ins>plan</ins> de la siguiente <ins>semana</ins> utilizando los <ins>registros</ins> y <ins>resultados</ins> de la <ins>semana</ins> anterior.
- **RF-018.** El <ins>sistema</ins> registra los <ins>horarios de atención</ins> del <ins>nutriólogo</ins> por <ins>día de la semana</ins> y la <ins>duración</ins> de cada <ins>cita</ins>.
- **RF-019.** El <ins>sistema</ins> muestra al <ins>paciente</ins> los <ins>horarios</ins> disponibles de los <ins>nutriólogos</ins> para las próximas dos <ins>semanas</ins>.
- **RF-020.** El <ins>sistema</ins> registra la <ins>solicitud de cita</ins> remota del <ins>paciente</ins> en un <ins>horario</ins> disponible, junto con el <ins>motivo</ins> de la <ins>consulta</ins>.
- **RF-021.** El <ins>sistema</ins> cancela una <ins>cita</ins> pendiente o confirmada cuando el <ins>paciente</ins> lo solicita.
- **RF-022.** El <ins>sistema</ins> registra si el <ins>nutriólogo</ins> acepta o rechaza cada <ins>solicitud de cita</ins> que recibe dentro de sus <ins>horarios</ins>.
- **RF-023.** El <ins>sistema</ins> registra la <ins>autorización</ins> que el <ins>paciente</ins> da a un <ins>nutriólogo</ins> para consultar su <ins>progreso</ins> y sus <ins>registros</ins>.
- **RF-024.** El <ins>sistema</ins> retira el <ins>acceso</ins> de un <ins>nutriólogo</ins> a la <ins>información</ins> del <ins>paciente</ins> cuando el <ins>paciente</ins> quita su <ins>autorización</ins>.
- **RF-025.** El <ins>sistema</ins> muestra al <ins>nutriólogo</ins> el <ins>progreso</ins> y los <ins>registros</ins> de los <ins>pacientes</ins> que lo autorizaron.
- **RF-026.** El <ins>sistema</ins> genera un <ins>archivo PDF</ins> con los <ins>indicadores</ins>, los <ins>registros diarios</ins> y el <ins>plan alimenticio</ins> de la <ins>semana</ins> o del <ins>mes</ins> que elija el <ins>paciente</ins>, y lo guarda en su <ins>teléfono</ins>.
- **RF-027.** El <ins>sistema</ins> sustituye los <ins>registros diarios</ins> con más de 30 <ins>días</ins> de <ins>antigüedad</ins> por un <ins>resumen mensual</ins> que el <ins>paciente</ins> puede descargar.
- **RF-028.** El <ins>sistema</ins> crea la <ins>cuenta</ins> de un <ins>nutriólogo</ins> con su <ins>nombre</ins> y <ins>correo electrónico</ins> cuando el <ins>administrador</ins> lo da de <ins>alta</ins>.
- **RF-029.** El <ins>sistema</ins> desactiva la <ins>cuenta</ins> de un <ins>usuario</ins> cuando el <ins>administrador</ins> lo indica.
- **RF-030.** El <ins>sistema</ins> muestra al <ins>paciente</ins> las <ins>comidas</ins> de su <ins>plan alimenticio</ins> vigente por <ins>día de la semana</ins> y por <ins>tiempo de comida</ins>.
- **RF-031.** El <ins>sistema</ins> guarda los <ins>datos</ins> del <ins>día</ins> que el <ins>paciente</ins> lleva capturados cuando decide guardar sin haber completado todos.
- **RF-032.** El <ins>sistema</ins> registra los <ins>datos</ins> de un <ins>día</ins> anterior que quedó sin <ins>registro</ins>, dentro de los últimos 30 <ins>días</ins>, cuando el <ins>paciente</ins> lo elige.
- **RF-033.** El <ins>sistema</ins> envía una <ins>notificación</ins> al <ins>teléfono</ins> del <ins>paciente</ins> cuando el <ins>nutriólogo</ins> acepta o rechaza su <ins>solicitud de cita</ins>.

En los criterios de aceptación aparecen además estos sustantivos que no salen en las descripciones: <ins>estado</ins> (completo, incompleto, sin registro, pendiente, confirmada, rechazada, cancelada, activa, desactivada), <ins>desayuno</ins>, <ins>colación</ins>, <ins>cena</ins>, <ins>historial</ins>, <ins>última sincronización</ins>, <ins>espacio</ins>, <ins>hora de inicio</ins>, <ins>hora de fin</ins>, <ins>gráfica</ins>, <ins>periodo</ins>, <ins>informe</ins>, <ins>medición</ins>, <ins>rango</ins>, <ins>pantalla</ins>, <ins>rol</ins> y <ins>mensaje</ins>.

## 2. Clases candidatas

Después de pasar cada sustantivo por las cuatro razones me quedaron diez clases. Para cada una anoté en una línea qué sabe y qué hace.

| Clase | Qué sabe | Qué hace |
|---|---|---|
| Cuenta | Nombre, correo, contraseña, rol, estado (activa o desactivada), fecha de nacimiento, sexo y estatura. | Inicia y cierra sesión, rechaza el acceso con datos incorrectos o si está desactivada, recupera la contraseña, actualiza sus datos personales y se desactiva. |
| RegistroDiario | Fecha, peso en ayunas, horas de sueño, agua, actividad física, porcentaje de grasa y de músculo, de dónde salió cada dato (dispositivo o captura manual) y su estado. | Se guarda completo o incompleto, valida que los valores estén dentro de rango, corrige un dato y calcula el IMC del día con la estatura de la cuenta. |
| Alimento | Nombre del alimento y el tiempo de comida en que se registró (desayuno, colación, comida o cena). | En blanco. |
| Dispositivo | Tipo (reloj, banda, báscula u otro), los datos que mide, si está conectado y su última sincronización. | Se vincula a una cuenta, se sincroniza y entrega sus datos al registro del día, y se desvincula. |
| PerfilAlimenticio | Objetivo, alergias, restricciones y alimentos que no le gustan al paciente. | Valida que tenga un objetivo antes de guardarse. Fuera de eso casi no hace nada, sus datos los usa el plan. |
| PlanAlimenticio | Semana que cubre, comidas por día y por tiempo de comida, y si es el plan vigente. | Se genera con el perfil y los datos de la cuenta, se ajusta con los registros de la semana anterior y muestra las comidas de un día. |
| HorarioDeAtencion | Día de la semana, hora de inicio, hora de fin y duración de cada cita. | Valida que la hora de fin sea posterior a la de inicio y se divide en los espacios que ven los pacientes. |
| Cita | Fecha, hora, motivo, estado, paciente, nutriólogo y si el paciente comparte su progreso. | Se solicita, se acepta, se rechaza o se cancela, libera su espacio cuando se rechaza o se cancela y avisa al paciente de la respuesta. |
| Autorizacion | Paciente, nutriólogo y fecha desde la que tiene acceso. | Se otorga, se retira y responde si un nutriólogo puede ver la información de un paciente. |
| ResumenMensual | Mes, resumen de los indicadores y planes de ese mes. | Se genera con los registros que pasan de 30 días y se descarga en PDF. |

**Casos para comentar en clase**

- **Alimento** se quedó con el "qué hace" en blanco. El sistema sí necesita guardar cada alimento por separado, con su tiempo de comida, porque aparece en el registro, en el plan y en el informe, pero no se me ocurre ninguna regla propia. Puede que sea una clase de puros datos o que en realidad sea un atributo de RegistroDiario y de PlanAlimenticio.
- **PerfilAlimenticio** casi también sale en blanco. Lo dejé como clase porque el sistema lo guarda, el paciente lo modifica por su cuenta y el plan lo consulta cada semana, pero la regla importante, que el plan no incluya alergias ni alimentos que no le gustan, vive en PlanAlimenticio.

## 3. Sustantivos descartados

**Es atributo**

| Sustantivo | De qué clase es atributo |
|---|---|
| nombre, correo electrónico, contraseña, fecha de nacimiento, sexo, estatura, centímetros, rol | Cuenta |
| sesión | Cuenta (si tiene o no la sesión iniciada) |
| enlace | Cuenta (es el medio para crear la nueva contraseña) |
| alta | Cuenta (es el momento en que se crea con el rol de nutriólogo) |
| peso, peso en ayunas, horas de sueño, agua, consumo de agua, actividad física, porcentaje de grasa, porcentaje de músculo, IMC | RegistroDiario |
| fecha, día, valor, corrección, captura, antigüedad | RegistroDiario |
| estado | RegistroDiario, Cita y Cuenta, cada una con sus propios estados |
| objetivo, alergias, restricciones, restricciones alimenticias | PerfilAlimenticio |
| comida, desayuno, colación, cena, tiempo de comida | Alimento y PlanAlimenticio |
| semana, día de la semana | PlanAlimenticio y HorarioDeAtencion |
| hora de inicio, hora de fin, duración | HorarioDeAtencion |
| espacio | Cita (es la fecha y la hora que ocupa la cita dentro de un horario) |
| motivo, consulta | Cita |
| última sincronización | Dispositivo |
| mes, año, periodo | ResumenMensual, o el filtro con el que se consulta el progreso |
| rango | RegistroDiario y Cuenta (son los límites con los que validan sus valores) |

**Es sinónimo**

| Sustantivo | Es sinónimo de |
|---|---|
| usuario | Cuenta |
| plan, plan semanal, plan alimenticio semanal, planes de alimentación | PlanAlimenticio |
| preferencias, preferencias alimenticias | PerfilAlimenticio |
| registro, registros diarios, dato registrado, medición | RegistroDiario |
| historial | RegistroDiario (es el conjunto de registros de un paciente) |
| solicitud, solicitud de cita | Cita (es una cita en estado pendiente) |
| horario, horarios disponibles | HorarioDeAtencion |
| acceso | Autorizacion |
| reloj inteligente, banda de actividad, báscula inteligente | Dispositivo (son sus tipos) |
| persona | paciente |

**Está fuera del sistema**

| Sustantivo | Por qué |
|---|---|
| paciente, nutriólogo, administrador | Son los actores. Existen fuera de la aplicación, y lo que el sistema guarda de ellos está en Cuenta, PerfilAlimenticio y HorarioDeAtencion. |
| teléfono | Es donde corre la aplicación y donde se guarda el PDF, pero el sistema no lo gestiona. |
| notificación | La entrega el servicio de notificaciones del teléfono. Para el sistema es solo una operación de Cita que avisa al paciente. |
| correo (como servicio de envío) | El sistema solo manda el enlace, quien lo entrega es el servicio de correo. |
| archivo PDF | Una vez descargado queda en el teléfono y el sistema ya no lo controla. |

**Es demasiado vago**

| Sustantivo | Por qué |
|---|---|
| sistema, aplicación | Es NutriGo! completo, no una pieza de él. |
| información, datos, datos personales | No dicen qué guardan. Lo que hay detrás ya está en Cuenta, RegistroDiario y PerfilAlimenticio. |
| progreso, avances, resultados, evolución, cambios físicos, indicadores | Se calculan con los registros diarios y el resumen mensual, no guardan nada propio. |
| seguimiento, alimentación | Describen la actividad del paciente, no una cosa concreta que el sistema guarde. |
| informe | Solo junta lo que ya está en RegistroDiario, PlanAlimenticio y ResumenMensual, y se genera cuando el paciente lo pide. |
| forma, dudas, cambios | No nombran nada concreto del sistema. |
| gráfica, pantalla, mensaje | Son de la interfaz, no del dominio. No tienen datos propios. |
