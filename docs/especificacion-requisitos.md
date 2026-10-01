# Especificación de requisitos

**Plantilla del curso · Ingeniería de Software I · SIS3407**

| Campo | Valor |
|---|---|
| **Sistema** | NutriGo! |
| **Autor** | Diego Valdovinos Rodríguez |
| **Versión** | 1.0 |
| **Fecha de la última actualización** | 01/10/2026 |



## 1. Propósito y alcance

**Propósito del documento:**
Este documento especifica los requisitos funcionales y no funcionales de NutriGo!, los casos de uso que los realizan y la trazabilidad entre requisitos, casos de uso y prototipo. Es la base para diseñar y construir la aplicación, y va dirigido a quienes van a diseñar, desarrollar y validar el sistema.

**Alcance del sistema:**
NutriGo! es una aplicación móvil que permite a las personas llevar un seguimiento diario de su alimentación y de sus cambios físicos, consultar su progreso y recibir un plan alimenticio personalizado que se adapta a sus resultados. Registra el peso en ayunas, las horas de sueño, el agua consumida, la alimentación y la actividad física diariamente, ya sea de forma automática o manual, y muestra el progreso del usuario mediante su peso, IMC, porcentaje de grasa y porcentaje de músculo. Permite registrar preferencias, alergias y alimentos que no le gustan al usuario para generar un plan alimenticio personalizado según sus datos, objetivos, preferencias y restricciones, y ajusta ese plan cada semana según los resultados registrados. Se conecta con dispositivos existentes compatibles, como relojes inteligentes, bandas de actividad y básculas inteligentes, para recibir datos del usuario. Proporciona los horarios disponibles del nutriólogo para atender dudas o solicitar una cita remota, y permite al nutriólogo consultar el progreso de sus pacientes y atender sus solicitudes dentro de los horarios establecidos. También permite al usuario descargar en PDF sus avances y planes de alimentación de forma semanal o mensual.

**Fuera del alcance:**
- No realiza diagnósticos médicos.
- No procesa pagos ni cobros dentro de la aplicación durante el desarrollo del semestre.
- No diseña ni fabrica dispositivos propios para medir los datos del usuario.



## 2. Usuarios y su contexto

| Usuario | Qué hace hoy sin el sistema | Qué espera del sistema |
|---|---|---|
| Paciente | Utiliza notas del teléfono, hojas de cálculo, aplicaciones diferentes o lleva sus registros de memoria. Cuando tiene muchas actividades se le olvida registrar sus datos y, al acumular registros sin completar, termina dejando el seguimiento. Acude con un nutriólogo cuando sus horarios se lo permiten. | Registrar sus datos, consultar su plan, revisar su progreso y comunicarse con un nutriólogo. Que sea rápido, sencillo y que sus datos sean confiables. Que la información se obtenga automáticamente de sus dispositivos y tener que registrar a mano solo lo que no se pueda obtener de ellos. |
| Nutriólogo | Da seguimiento a sus pacientes durante las consultas, pero el seguimiento entre una consulta y otra puede ser limitado. | Consultar el progreso de sus pacientes, revisar sus registros y atender solicitudes dentro de sus horarios establecidos. |
| Administrador | Este rol solo existe dentro del sistema. | Gestionar usuarios y mantener el funcionamiento del sistema. |

**Conflictos identificados entre usuarios:**
El paciente puede solicitar comunicarse con un nutriólogo en cualquier momento, pero este solo puede atenderlo durante sus horarios establecidos, lo que puede generar una espera para el paciente. Resolución adoptada: el paciente solo puede solicitar citas en los horarios que el nutriólogo registró, y su solicitud queda en espera hasta que el nutriólogo la acepta o la rechaza.



## 3. Requisitos funcionales

### 3.1 Resumen

| ID | Nombre | Prioridad | Origen |
|---|---|---|---|
| RF-001 | Creación de cuenta del paciente | Imprescindible | Caso de uso CU-01 |
| RF-002 | Inicio de sesión | Imprescindible | Caso de uso CU-02 |
| RF-003 | Acceso negado sin cuenta o con datos incorrectos | Imprescindible | Caso de uso CU-02 |
| RF-004 | Recuperación de contraseña | Importante | Caso de uso CU-02 |
| RF-005 | Cierre de sesión | Imprescindible | Caso de uso CU-03 |
| RF-006 | Actualización de datos personales | Importante | Caso de uso CU-04 |
| RF-007 | Registro diario | Imprescindible | Visión del producto |
| RF-008 | Captura manual de datos que no llegan del dispositivo | Imprescindible | Entrevista (22/09/2026) |
| RF-009 | Registro del día sin completar días anteriores | Imprescindible | Entrevista (22/09/2026) |
| RF-010 | Corrección de un registro | Importante | Caso de uso CU-08 |
| RF-011 | Vinculación de un dispositivo | Importante | Visión del producto |
| RF-012 | Integración con dispositivos | Importante | Visión del producto |
| RF-013 | Desvinculación de un dispositivo | Importante | Caso de uso CU-06 |
| RF-014 | Consulta del progreso | Imprescindible | Visión del producto |
| RF-015 | Preferencias alimenticias | Imprescindible | Visión del producto |
| RF-016 | Plan alimenticio personalizado | Imprescindible | Visión del producto |
| RF-017 | Actualización semanal del plan | Importante | Visión del producto |
| RF-018 | Registro de horarios del nutriólogo | Imprescindible | Caso de uso CU-13 |
| RF-019 | Consulta de horarios de nutriólogos | Imprescindible | Visión del producto |
| RF-020 | Solicitud de citas remotas | Imprescindible | Visión del producto |
| RF-021 | Cancelación de una cita | Importante | Caso de uso CU-15 |
| RF-022 | Atención de solicitudes | Imprescindible | Visión del producto |
| RF-023 | Autorización de acceso al nutriólogo | Imprescindible | Visión del producto |
| RF-024 | Retiro de la autorización | Importante | Caso de uso CU-17 |
| RF-025 | Seguimiento de pacientes autorizados | Imprescindible | Visión del producto |
| RF-026 | Informes | Importante | Visión del producto |
| RF-027 | Resumen mensual de registros | Importante | Visión del producto |
| RF-028 | Alta de un nutriólogo | Imprescindible | Caso de uso CU-19 |
| RF-029 | Desactivación de una cuenta | Importante | Caso de uso CU-20 |

### 3.2 Fichas

**RF-001 · Creación de cuenta del paciente**

| Campo | Contenido |
|---|---|
| Descripción | El sistema crea la cuenta de un paciente con su nombre, correo electrónico, contraseña, fecha de nacimiento, sexo y estatura. |
| Origen | Caso de uso CU-01. El paciente necesita una cuenta antes de usar cualquier función de la aplicación. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Cuando una persona llena todos los datos, se crea su cuenta y entra a la aplicación. Si el correo ya está registrado o falta algún dato, el sistema no crea la cuenta y le indica qué debe corregir. |
| Relacionado con | RF-002, RF-006, RF-014, RF-016 |

**RF-002 · Inicio de sesión**

| Campo | Contenido |
|---|---|
| Descripción | El sistema da acceso a la aplicación a un usuario registrado que ingresa su correo electrónico y su contraseña. |
| Origen | Caso de uso CU-02. El inicio de sesión es previo a todo uso de la cuenta. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Con un correo registrado y su contraseña correcta, el usuario entra a la pantalla de inicio que corresponde a su rol: paciente, nutriólogo o administrador. |
| Relacionado con | RF-001, RF-003, RF-005, RNF-SEG-001, RNF-SEG-003 |

**RF-003 · Acceso negado sin cuenta o con datos incorrectos**

| Campo | Contenido |
|---|---|
| Descripción | El sistema impide el acceso a la aplicación cuando el correo no pertenece a una cuenta registrada, cuando la contraseña no corresponde o cuando la cuenta está desactivada. |
| Origen | Caso de uso CU-02, flujos alternos. Nadie debe entrar a la aplicación sin una cuenta o con datos incorrectos. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Si una persona intenta entrar con un correo que no está registrado, el sistema no le da acceso y le muestra la opción de crear una cuenta. Si la contraseña es incorrecta, no le da acceso y le muestra la opción de recuperar su contraseña. Si la cuenta está desactivada, no le da acceso y se lo indica. |
| Relacionado con | RF-001, RF-002, RF-004, RF-029 |

**RF-004 · Recuperación de contraseña**

| Campo | Contenido |
|---|---|
| Descripción | El sistema envía al correo registrado del usuario un enlace para crear una nueva contraseña. |
| Origen | Caso de uso CU-02, flujo alterno. El usuario que olvida su contraseña necesita una forma de volver a entrar. |
| Prioridad | Importante |
| Criterio de aceptación | Al solicitar la recuperación con un correo registrado, llega un enlace a ese correo. Después de crear la nueva contraseña, el usuario entra con ella y la contraseña anterior ya no funciona. |
| Relacionado con | RF-002, RF-003 |

**RF-005 · Cierre de sesión**

| Campo | Contenido |
|---|---|
| Descripción | El sistema cierra la sesión del usuario cuando este lo solicita. |
| Origen | Caso de uso CU-03. El usuario necesita salir de su cuenta para que nadie más vea su información desde su teléfono. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Después de cerrar sesión, la aplicación regresa a la pantalla de inicio de sesión y no muestra ninguna información hasta que el usuario vuelve a entrar. |
| Relacionado con | RF-002, RNF-SEG-001 |

**RF-006 · Actualización de datos personales**

| Campo | Contenido |
|---|---|
| Descripción | El sistema guarda los cambios que el paciente hace a su nombre, fecha de nacimiento, sexo y estatura. |
| Origen | Caso de uso CU-04. Los datos personales, como la estatura, se usan para calcular el IMC y generar el plan. |
| Prioridad | Importante |
| Criterio de aceptación | Si el paciente cambia su estatura de 1.74 m a 1.75 m, su perfil muestra 1.75 m y los siguientes cálculos de IMC usan ese valor. |
| Relacionado con | RF-001, RF-014 |

**RF-007 · Registro diario**

| Campo | Contenido |
|---|---|
| Descripción | El sistema registra para cada día el peso en ayunas, las horas de sueño, el consumo de agua, la alimentación y la actividad física del paciente. |
| Origen | Visión del producto (20/08/2026). Aceptado por el cliente. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Si el paciente captura 72.4 kg, 7 horas de sueño, 1.5 litros de agua, avena en el desayuno y 30 minutos de caminata, los cinco datos aparecen en el registro de ese día con la fecha correcta. Cada alimento se registra indicando si fue parte del desayuno, la comida, la cena o una colación. |
| Relacionado con | RF-008, RF-009, RF-012, RNF-CON-002 |

**RF-008 · Captura manual de datos que no llegan del dispositivo**

| Campo | Contenido |
|---|---|
| Descripción | El sistema habilita la captura manual de cualquier dato del registro diario que no se haya obtenido de un dispositivo. |
| Origen | Entrevista (22/09/2026). Se suponía que el registro sería automático, pero en la entrevista se descubrió que no todos los datos pueden obtenerse de un dispositivo y que a veces el dispositivo no registra bien la información. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Si la báscula no envió el peso del día, el campo aparece como "Sin dato del dispositivo" y el paciente lo puede capturar a mano. Los alimentos siempre se capturan a mano porque ningún dispositivo los registra. |
| Relacionado con | RF-007, RF-012 |

**RF-009 · Registro del día sin completar días anteriores**

| Campo | Contenido |
|---|---|
| Descripción | El sistema registra la información del día actual aunque existan días anteriores sin registro. |
| Origen | Entrevista (22/09/2026). Se suponía que el paciente completaría los días que olvidó, pero en la entrevista se descubrió que muchas veces no recuerda qué comió ni las cantidades, y que acumular registros sin completar hace que deje el seguimiento. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Con tres días anteriores vacíos, el paciente guarda el registro de hoy sin que el sistema le pida completar esos días. Los tres días aparecen en el historial como "Sin registro". |
| Relacionado con | RF-007, RNF-CON-001 |

**RF-010 · Corrección de un registro**

| Campo | Contenido |
|---|---|
| Descripción | El sistema reemplaza un dato registrado por el valor que el paciente capture como corrección. |
| Origen | Caso de uso CU-08. Un dato equivocado no debe afectar la consulta del progreso. |
| Prioridad | Importante |
| Criterio de aceptación | Si el reloj registró 7 h 10 min de sueño y el paciente lo corrige a 7 h 40 min, el registro de ese día muestra 7 h 40 min en lugar del valor que envió el reloj. |
| Relacionado con | RF-007, RF-014, RNF-CON-002 |

**RF-011 · Vinculación de un dispositivo**

| Campo | Contenido |
|---|---|
| Descripción | El sistema vincula la cuenta del paciente con un reloj inteligente, una banda de actividad, una báscula inteligente u otro dispositivo compatible. |
| Origen | Visión del producto (20/08/2026). Aceptado por el cliente. |
| Prioridad | Importante |
| Criterio de aceptación | Después de vincularlo, el dispositivo aparece como conectado junto con la fecha y la hora de su última sincronización. Si el dispositivo no es compatible, el sistema lo indica y el paciente sigue registrando a mano. |
| Relacionado con | RF-012, RF-013 |

**RF-012 · Integración con dispositivos**

| Campo | Contenido |
|---|---|
| Descripción | El sistema obtiene de los dispositivos vinculados el peso, el porcentaje de grasa, el porcentaje de músculo, las horas de sueño y la actividad física que cada dispositivo mida. |
| Origen | Visión del producto (20/08/2026). Aceptado por el cliente. |
| Prioridad | Importante |
| Criterio de aceptación | Si el paciente se pesa en una báscula vinculada, el peso aparece en el registro de ese día sin que tenga que capturarlo. Los datos que el dispositivo no mide quedan por registrarse a mano. |
| Relacionado con | RF-007, RF-008, RF-011, RF-014 |

**RF-013 · Desvinculación de un dispositivo**

| Campo | Contenido |
|---|---|
| Descripción | El sistema desvincula un dispositivo de la cuenta del paciente cuando este lo solicita. |
| Origen | Caso de uso CU-06. El paciente necesita dejar de recibir datos de un dispositivo que ya no usa. |
| Prioridad | Importante |
| Criterio de aceptación | Después de desvincular una báscula, las nuevas mediciones de esa báscula ya no aparecen en NutriGo!, y los datos que ya se habían obtenido siguen en el historial. |
| Relacionado con | RF-011, RF-012 |

**RF-014 · Consulta del progreso**

| Campo | Contenido |
|---|---|
| Descripción | El sistema muestra el peso, el IMC, el porcentaje de grasa corporal y el porcentaje de músculo del paciente, junto con su evolución durante la última semana, el último mes o el último año. |
| Origen | Visión del producto (20/08/2026). Aceptado por el cliente. En la entrevista (22/09/2026) se mencionó que revisar solo el peso no es suficiente. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Con una estatura de 1.75 m y un peso de 70.0 kg, el sistema muestra un IMC de 22.9. Al elegir el último mes, la gráfica muestra un punto por cada día que tiene ese indicador registrado. Si no existe ninguna medición de grasa o de músculo, esos indicadores aparecen como "Sin medición". |
| Relacionado con | RF-006, RF-007, RF-012, RF-027, RNF-CON-001 |

**RF-015 · Preferencias alimenticias**

| Campo | Contenido |
|---|---|
| Descripción | El sistema registra los objetivos, las preferencias, las alergias, las restricciones y los alimentos que no le gustan al paciente. |
| Origen | Visión del producto (20/08/2026). Aceptado por el cliente. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Cuando el paciente guarda su objetivo y agrega "cacahuate" como alergia, ambos datos aparecen en su perfil. Si después modifica cualquiera de estos datos, el cambio se toma en cuenta en el siguiente plan que se genere. |
| Relacionado con | RF-016 |

**RF-016 · Plan alimenticio personalizado**

| Campo | Contenido |
|---|---|
| Descripción | El sistema genera un plan alimenticio semanal considerando los datos básicos, los objetivos, las preferencias y las restricciones del paciente. |
| Origen | Visión del producto (20/08/2026). Aceptado por el cliente. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Para un paciente con alergia al cacahuate, el plan contiene siete días con desayuno, comida y cena, y ninguna comida incluye cacahuate. El paciente consulta el plan por día y por tiempo de comida. Si el paciente todavía no registra sus preferencias alimenticias, el sistema se las pide antes de generar el primer plan. |
| Relacionado con | RF-001, RF-015, RF-017 |

**RF-017 · Actualización semanal del plan**

| Campo | Contenido |
|---|---|
| Descripción | El sistema genera el plan de la siguiente semana utilizando los registros y resultados de la semana anterior. |
| Origen | Visión del producto (20/08/2026). Aceptado por el cliente. |
| Prioridad | Importante |
| Criterio de aceptación | Cada lunes el paciente tiene un plan nuevo que indica cuántos días de la semana anterior tenían registro. Dos pacientes con el mismo perfil, pero con registros distintos, reciben planes distintos. |
| Relacionado con | RF-007, RF-016, RNF-CON-001 |

**RF-018 · Registro de horarios del nutriólogo**

| Campo | Contenido |
|---|---|
| Descripción | El sistema registra los horarios de atención del nutriólogo por día de la semana y la duración de cada cita. |
| Origen | Caso de uso CU-13. Los pacientes solo pueden solicitar citas si el nutriólogo registró antes sus horarios. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Si el nutriólogo registra los martes de 16:00 a 18:00 con citas de 30 minutos, los pacientes ven cuatro espacios disponibles ese día. Si después modifica sus horarios, las citas ya confirmadas se conservan. |
| Relacionado con | RF-019, RF-022 |

**RF-019 · Consulta de horarios de nutriólogos**

| Campo | Contenido |
|---|---|
| Descripción | El sistema muestra al paciente los horarios disponibles de los nutriólogos para las próximas dos semanas. |
| Origen | Visión del producto (20/08/2026). Aceptado por el cliente. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Los espacios que ya solicitó otro paciente no aparecen como disponibles. Si no hay espacios disponibles, el sistema lo indica. |
| Relacionado con | RF-018, RF-020 |

**RF-020 · Solicitud de citas remotas**

| Campo | Contenido |
|---|---|
| Descripción | El sistema registra la solicitud de cita remota del paciente en un horario disponible, junto con el motivo de la consulta. |
| Origen | Visión del producto (20/08/2026). Aceptado por el cliente. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Al solicitar el martes a las 16:00, la solicitud aparece en espera de respuesta para el paciente y para el nutriólogo, y ese espacio deja de estar disponible para otros pacientes. |
| Relacionado con | RF-019, RF-021, RF-022, RF-023 |

**RF-021 · Cancelación de una cita**

| Campo | Contenido |
|---|---|
| Descripción | El sistema cancela una cita en espera o confirmada cuando el paciente lo solicita. |
| Origen | Caso de uso CU-15. El paciente necesita liberar el horario de una cita a la que ya no puede asistir. |
| Prioridad | Importante |
| Criterio de aceptación | Después de cancelar, la cita aparece como "Cancelada" para el paciente y para el nutriólogo, y el espacio vuelve a estar disponible para otros pacientes. |
| Relacionado con | RF-019, RF-020 |

**RF-022 · Atención de solicitudes**

| Campo | Contenido |
|---|---|
| Descripción | El sistema registra si el nutriólogo acepta o rechaza cada solicitud de cita que recibe dentro de sus horarios. |
| Origen | Visión del producto (20/08/2026). |
| Prioridad | Imprescindible |
| Criterio de aceptación | Si el nutriólogo acepta, el paciente ve su cita como "Confirmada". Si la rechaza, el paciente la ve como "Rechazada" y el espacio vuelve a estar disponible. |
| Relacionado con | RF-018, RF-020 |

**RF-023 · Autorización de acceso al nutriólogo**

| Campo | Contenido |
|---|---|
| Descripción | El sistema registra la autorización que el paciente da a un nutriólogo para consultar su progreso y sus registros. |
| Origen | Visión del producto (20/08/2026). Aceptado por el cliente. En la entrevista (22/09/2026) se confirmó que solo las personas autorizadas deben consultar la información del paciente. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Al solicitar una cita, el paciente elige si autoriza al nutriólogo. Si lo autoriza, el paciente aparece en la lista de ese nutriólogo. Si no lo autoriza, la solicitud se registra igual, pero el nutriólogo no ve su información. |
| Relacionado con | RF-020, RF-024, RF-025, RNF-SEG-002 |

**RF-024 · Retiro de la autorización**

| Campo | Contenido |
|---|---|
| Descripción | El sistema retira el acceso de un nutriólogo a la información del paciente cuando el paciente quita su autorización. |
| Origen | Caso de uso CU-17. El paciente necesita dejar de compartir su información con un nutriólogo. |
| Prioridad | Importante |
| Criterio de aceptación | Después de que el paciente retira la autorización, el paciente ya no aparece en la lista del nutriólogo y este no puede abrir su información. |
| Relacionado con | RF-023, RF-025, RNF-SEG-002 |

**RF-025 · Seguimiento de pacientes autorizados**

| Campo | Contenido |
|---|---|
| Descripción | El sistema muestra al nutriólogo el progreso y los registros de los pacientes que lo autorizaron. |
| Origen | Visión del producto (20/08/2026). |
| Prioridad | Imprescindible |
| Criterio de aceptación | Al abrir a un paciente autorizado, el nutriólogo ve la misma información de progreso que ve el paciente, incluidos los días sin registro, pero no puede modificarla. |
| Relacionado con | RF-014, RF-023, RF-024, RNF-SEG-002 |

**RF-026 · Informes**

| Campo | Contenido |
|---|---|
| Descripción | El sistema genera informes semanales y mensuales en PDF con los avances y el plan alimenticio del paciente. |
| Origen | Visión del producto (20/08/2026). |
| Prioridad | Importante |
| Criterio de aceptación | Al elegir una semana, el PDF contiene los siete días con sus registros y el plan de esa semana. Al elegir un mes, contiene el resumen de los indicadores y los planes de ese mes. Los días sin registro aparecen como "Sin registro". |
| Relacionado con | RF-014, RF-027, RNF-CON-001 |

**RF-027 · Resumen mensual de registros**

| Campo | Contenido |
|---|---|
| Descripción | El sistema sustituye los registros diarios con más de 30 días de antigüedad por un resumen mensual que el paciente puede descargar. |
| Origen | Visión del producto (20/08/2026). |
| Prioridad | Importante |
| Criterio de aceptación | Cuando un mes queda fuera de los últimos 30 días, sus registros diarios ya no aparecen en el historial y ese mes aparece como un resumen mensual descargable. Ningún registro se elimina antes de que exista el resumen de su mes. |
| Relacionado con | RF-014, RF-026 |

**RF-028 · Alta de un nutriólogo**

| Campo | Contenido |
|---|---|
| Descripción | El sistema crea la cuenta de un nutriólogo con su nombre y correo electrónico cuando el administrador lo da de alta. |
| Origen | Caso de uso CU-19. Los nutriólogos no crean su cuenta por sí mismos, sino que el administrador los da de alta. |
| Prioridad | Imprescindible |
| Criterio de aceptación | El nutriólogo recibe en su correo un enlace para crear su contraseña y, al entrar, ve las funciones de nutriólogo. Una persona no puede crear por su cuenta una cuenta de nutriólogo desde la aplicación. |
| Relacionado con | RF-002, RF-018, RNF-SEG-003 |

**RF-029 · Desactivación de una cuenta**

| Campo | Contenido |
|---|---|
| Descripción | El sistema desactiva la cuenta de un usuario cuando el administrador lo indica. |
| Origen | Caso de uso CU-20. El administrador necesita impedir el acceso de un usuario sin borrar su información. |
| Prioridad | Importante |
| Criterio de aceptación | El usuario con la cuenta desactivada ya no puede iniciar sesión y su información se conserva. Si es un nutriólogo, sus horarios dejan de mostrarse y sus citas en espera o confirmadas aparecen como "Cancelada" para los pacientes. |
| Relacionado con | RF-003, RF-028 |



## 4. Requisitos no funcionales

### 4.1 Resumen

| ID | Atributo | Nombre | Prioridad | Origen |
|---|---|---|---|---|
| RNF-USA-001 | Usabilidad | Tiempo para registrar el día | Imprescindible | Visión del producto |
| RNF-USA-002 | Usabilidad | Acceso rápido al progreso | Importante | Visión del producto |
| RNF-SEG-001 | Seguridad | Acceso solo con sesión iniciada | Imprescindible | Visión del producto |
| RNF-SEG-002 | Seguridad | Acceso del nutriólogo a pacientes autorizados | Imprescindible | Visión del producto |
| RNF-SEG-003 | Seguridad | Funciones según el rol | Imprescindible | Visión del producto |
| RNF-CON-001 | Confiabilidad | Registros incompletos sin rellenar | Imprescindible | Visión del producto |
| RNF-CON-002 | Confiabilidad | Valores dentro de rango | Imprescindible | Visión del producto |

### 4.2 Fichas

**Usabilidad**

**RNF-USA-001 · Tiempo para registrar el día**

| Campo | Contenido |
|---|---|
| Atributo de calidad | Usabilidad |
| Descripción | El paciente registra los datos manuales de su día en menos de 2 minutos. |
| Métrica | Tiempo promedio desde que abre la aplicación hasta que guarda el agua, los alimentos de una comida y una actividad física, medido con cinco personas que ya usaron la aplicación una vez. |
| Origen | Visión del producto (20/08/2026). Aceptado por el cliente. |
| Prioridad | Imprescindible |
| Por qué importa | En la entrevista se identificó que registrar a mano toma tiempo y que, cuando cuesta trabajo, la persona termina dejando el seguimiento. |
| Afecta a | RF-007, RF-008, RF-009 |

**RNF-USA-002 · Acceso rápido al progreso**

| Campo | Contenido |
|---|---|
| Atributo de calidad | Usabilidad |
| Descripción | Desde la pantalla de inicio, el paciente llega a la consulta de su progreso con un máximo de dos toques. |
| Métrica | Número de toques desde la pantalla de inicio hasta que se muestran sus indicadores, revisado en un teléfono. |
| Origen | Visión del producto (20/08/2026). Aceptado por el cliente. |
| Prioridad | Importante |
| Por qué importa | Consultar el progreso es lo que motiva al paciente a seguir registrando. Si le cuesta encontrarlo, pierde el interés en el seguimiento. |
| Afecta a | RF-014 |

**Seguridad**

**RNF-SEG-001 · Acceso solo con sesión iniciada**

| Campo | Contenido |
|---|---|
| Atributo de calidad | Seguridad |
| Descripción | La aplicación no muestra ningún dato personal ni de salud a una persona que no haya iniciado sesión. |
| Métrica | En el 100 % de las pantallas probadas sin sesión iniciada, el sistema pide iniciar sesión y no muestra información. |
| Origen | Visión del producto (20/08/2026). Aceptado por el cliente. |
| Prioridad | Imprescindible |
| Por qué importa | NutriGo! guarda información personal y de salud. Si alguien sin cuenta pudiera verla, se expondría la privacidad de los pacientes. |
| Afecta a | RF-002, RF-003, RF-005 |

**RNF-SEG-002 · Acceso del nutriólogo a pacientes autorizados**

| Campo | Contenido |
|---|---|
| Atributo de calidad | Seguridad |
| Descripción | Un nutriólogo solo consulta la información de los pacientes que lo autorizaron. |
| Métrica | El 100 % de los intentos de un nutriólogo por abrir la información de un paciente que no lo autorizó, o que retiró su autorización, son rechazados. |
| Origen | Visión del producto (20/08/2026). Aceptado por el cliente. |
| Prioridad | Imprescindible |
| Por qué importa | En la entrevista se confirmó que el paciente considera privada su información y que solo las personas que él autorice deben poder verla. |
| Afecta a | RF-023, RF-024, RF-025 |

**RNF-SEG-003 · Funciones según el rol**

| Campo | Contenido |
|---|---|
| Atributo de calidad | Seguridad |
| Descripción | Cada usuario solo accede a las funciones de su rol: paciente, nutriólogo o administrador. |
| Métrica | El 100 % de los intentos de un usuario por entrar a funciones de otro rol son rechazados. Por ejemplo, un paciente no puede abrir la lista de solicitudes de un nutriólogo ni dar de alta a un nutriólogo. |
| Origen | Visión del producto (20/08/2026). Aceptado por el cliente. |
| Prioridad | Imprescindible |
| Por qué importa | Cada rol maneja información y acciones distintas. Si un paciente pudiera usar funciones de nutriólogo o de administrador, podría ver información de otros pacientes o modificar cuentas. |
| Afecta a | RF-002, RF-018, RF-022, RF-025, RF-028, RF-029 |

**Confiabilidad**

**RNF-CON-001 · Registros incompletos sin rellenar**

| Campo | Contenido |
|---|---|
| Atributo de calidad | Confiabilidad |
| Descripción | Los días y los datos sin registro se muestran como "Sin registro" y nunca se reemplazan por cero ni por valores estimados. |
| Métrica | En una semana de prueba con dos días sin peso, la gráfica muestra cinco puntos y el promedio semanal se calcula con cinco valores. |
| Origen | Visión del producto (20/08/2026). Aceptado por el cliente. |
| Prioridad | Imprescindible |
| Por qué importa | Si los huecos se llenaran con ceros o con estimaciones, el paciente vería un progreso que no ocurrió y el plan siguiente se calcularía con datos falsos. |
| Afecta a | RF-009, RF-014, RF-017, RF-026 |

**RNF-CON-002 · Valores dentro de rango**

| Campo | Contenido |
|---|---|
| Atributo de calidad | Confiabilidad |
| Descripción | El sistema rechaza los valores fuera de estos rangos: peso de 20 a 300 kg, estatura de 0.50 a 2.50 m, sueño de 0 a 24 horas, agua de 0 a 10 litros por día, actividad física de 0 a 1,440 minutos por día, grasa corporal de 2 a 70 % y músculo de 10 a 80 %. |
| Métrica | El 100 % de los valores de prueba fuera de rango se rechazan con un mensaje que indica el rango permitido, tanto si se capturan a mano como si llegan de un dispositivo. |
| Origen | Visión del producto (20/08/2026). Aceptado por el cliente. |
| Prioridad | Imprescindible |
| Por qué importa | Un error al capturar, como escribir 724 en lugar de 72.4, cambia la gráfica, el IMC y el plan de la siguiente semana. |
| Afecta a | RF-001, RF-006, RF-007, RF-008, RF-010, RF-012 |



## 5. Casos de uso

El detalle de cada caso de uso (actor, objetivo, precondición, escenario principal, flujos alternos y postcondición) está en `docs/casos de uso/cu-01.md` a `cu-20.md`. Cada caso de uso se relaciona con los requisitos funcionales que realiza:

| ID | Nombre | Requisitos que realiza |
|---|---|---|
| CU-01 | Crear una cuenta | RF-001 |
| CU-02 | Iniciar sesión | RF-002, RF-003, RF-004 |
| CU-03 | Cerrar sesión | RF-005 |
| CU-04 | Actualizar los datos personales | RF-006 |
| CU-05 | Vincular un dispositivo | RF-011 |
| CU-06 | Desvincular un dispositivo | RF-013 |
| CU-07 | Registrar el seguimiento diario | RF-007, RF-008, RF-009, RF-012 |
| CU-08 | Corregir un registro | RF-010 |
| CU-09 | Consultar el progreso físico | RF-014 |
| CU-10 | Descargar un informe | RF-026, RF-027 |
| CU-11 | Configurar las preferencias alimenticias | RF-015 |
| CU-12 | Consultar el plan alimenticio semanal | RF-016, RF-017 |
| CU-13 | Registrar los horarios de atención | RF-018 |
| CU-14 | Solicitar una cita remota | RF-019, RF-020, RF-023 |
| CU-15 | Cancelar una cita | RF-021 |
| CU-16 | Atender una solicitud de cita | RF-022 |
| CU-17 | Retirar la autorización a un nutriólogo | RF-024 |
| CU-18 | Revisar el progreso de un paciente | RF-025 |
| CU-19 | Dar de alta a un nutriólogo | RF-028 |
| CU-20 | Desactivar una cuenta | RF-029 |



## 6. Trazabilidad

| Requisito | Origen | Caso de uso | Elemento del prototipo |
|---|---|---|---|
| RF-001 | Caso de uso CU-01 | CU-01 | Pantallas Bienvenida y Crear cuenta |
| RF-002 | Caso de uso CU-02 | CU-02 | Pantalla Inicio de sesión |
| RF-003 | Caso de uso CU-02 | CU-02 | Pantalla Datos de acceso incorrectos |
| RF-004 | Caso de uso CU-02 | CU-02 | Pantallas Recuperar contraseña y Revisa tu correo |
| RF-005 | Caso de uso CU-03 | CU-03 | Opción Cerrar sesión en la pantalla Perfil |
| RF-006 | Caso de uso CU-04 | CU-04 | Pantalla Datos personales |
| RF-007 | Visión del producto | CU-07 | Pantalla Registro del día |
| RF-008 | Entrevista (22/09/2026) | CU-07 | Pantalla Dato sin llegar del dispositivo |
| RF-009 | Entrevista (22/09/2026) | CU-07 | Pantallas Panel con días sin registro y Día guardado incompleto |
| RF-010 | Caso de uso CU-08 | CU-08 | Pantalla Corregir un dato |
| RF-011 | Visión del producto | CU-05 | Pantallas Vincular un dispositivo y Dispositivo vinculado |
| RF-012 | Visión del producto | CU-07 | Pantallas Registro del día y Mis dispositivos |
| RF-013 | Caso de uso CU-06 | CU-06 | Pantallas Desvincular dispositivo y Dispositivo desvinculado |
| RF-014 | Visión del producto | CU-09 | Pantalla Mi progreso |
| RF-015 | Visión del producto | CU-11 | Pantalla Preferencias alimenticias |
| RF-016 | Visión del producto | CU-12 | Pantalla Plan semanal |
| RF-017 | Visión del producto | CU-12 | Aviso de ajuste semanal en la pantalla Plan semanal |
| RF-018 | Caso de uso CU-13 | CU-13 | Pantalla Mis horarios |
| RF-019 | Visión del producto | CU-14 | Pantalla Consultas |
| RF-020 | Visión del producto | CU-14 | Pantallas Solicitar cita y Mis citas |
| RF-021 | Caso de uso CU-15 | CU-15 | Pantalla Cita cancelada |
| RF-022 | Visión del producto | CU-16 | Pantallas Solicitudes de cita, Solicitud aceptada y Solicitud rechazada |
| RF-023 | Visión del producto | CU-14 | Opción Compartir mi progreso en la pantalla Solicitar cita |
| RF-024 | Caso de uso CU-17 | CU-17 | Pantallas Nutriólogos con acceso y Acceso retirado |
| RF-025 | Visión del producto | CU-18 | Pantallas Mis pacientes y Progreso del paciente |
| RF-026 | Visión del producto | CU-10 | Pantallas Descargar informe e Informe descargado |
| RF-027 | Visión del producto | CU-10 | Opción Mensual en la pantalla Descargar informe |
| RF-028 | Caso de uso CU-19 | CU-19 | Pantallas Agregar nutriólogo y Nutriólogo dado de alta |
| RF-029 | Caso de uso CU-20 | CU-20 | Pantalla Cuenta desactivada |
| RNF-USA-001 | Visión del producto | CU-07 | Pantalla Registro del día |
| RNF-USA-002 | Visión del producto | CU-09 | Tarjeta Mi progreso del Panel principal |
| RNF-SEG-001 | Visión del producto | CU-02, CU-03 | Pantallas Bienvenida e Inicio de sesión |
| RNF-SEG-002 | Visión del producto | CU-14, CU-17, CU-18 | Pantallas Mis pacientes y Progreso del paciente |
| RNF-SEG-003 | Visión del producto | CU-02, CU-13, CU-16, CU-19, CU-20 | Pantallas Panel del nutriólogo y Usuarios |
| RNF-CON-001 | Visión del producto | CU-07, CU-09, CU-10, CU-18 | Pantallas Mi progreso y Día guardado incompleto |
| RNF-CON-002 | Visión del producto | CU-01, CU-04, CU-07, CU-08 | Pantalla Valor fuera de rango |



## 7. Registro de cambios

| Fecha | Requisito | Qué cambió | Por qué |
|---|---|---|---|
| 22/09/2026 | RF-008 | Se agregó la captura manual de los datos que no llegan del dispositivo. | En la entrevista se descubrió que no todos los datos se obtienen de un dispositivo, como los alimentos, y que a veces el dispositivo falla. |
| 22/09/2026 | RF-009 | Se agregó que el paciente puede registrar el día actual sin completar los días anteriores. | En la entrevista se descubrió que la persona no siempre recuerda los días olvidados y que acumular registros sin completar la lleva a dejar el seguimiento. |
| 22/09/2026 | RF-014 | Se confirmó que el progreso debe incluir el IMC, el porcentaje de grasa y el porcentaje de músculo además del peso. | En la entrevista se mencionó que revisar solo el peso no es suficiente. |
| 30/09/2026 | RF-019, RF-020 | El requisito "Solicitud de consultas" de la visión se dividió en la consulta de horarios y la solicitud de citas remotas. | Unía dos comportamientos distintos en un solo requisito. |
| 30/09/2026 | RF-022, RF-025 | El requisito "Seguimiento de pacientes" de la visión se dividió en la atención de solicitudes y el seguimiento de pacientes autorizados. | Unía dos comportamientos distintos en un solo requisito. |
| 30/09/2026 | Consentimiento para datos de salud y cancelación de cuenta | Se eliminaron. | Correspondían a obligaciones legales y no a lo que el sistema necesita para funcionar dentro del alcance del proyecto. |
| 30/09/2026 | Contraseña de 15 caracteres y segundo factor de autenticación | Se eliminaron. | Imponían una solución técnica en lugar de describir una necesidad. La protección del acceso queda cubierta por RNF-SEG-001 y RNF-SEG-003. |
| 30/09/2026 | Plan con registros insuficientes | Se eliminó. | Dependía de un umbral de tres días que no salió del cliente ni de la visión. RF-017 y RNF-CON-001 ya cubren los días sin registro. |
| 30/09/2026 | Todos | Se propuso cambiar la prioridad a Alta, Media y Baja. Se rechazó. | La guía de redacción del curso establece la prioridad como imprescindible, importante o deseable. |
| 01/10/2026 | RF-010, RF-011 | Se ajustaron los criterios de aceptación a lo que muestra el prototipo: la corrección de un dato del reloj y la opción de vincular otro dispositivo compatible. | El prototipo en Figma mostró estos casos durante su diseño. |
