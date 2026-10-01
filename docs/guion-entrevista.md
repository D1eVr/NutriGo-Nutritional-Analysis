
# Guion de entrevista

> **Proyecto:** NutriGo!  
> **Autor:** Diego Valdovinos Rodríguez  
> **Fecha de la última versión:** 22/09/2026  
> **Repositorio:** https://github.com/D1eVr/NutriGo-Nutritional-Analysis.git

## 1. Guion de entrevista

### Apertura

Hola este proyecto está enfocado en el seguimiento de la alimentación y los cambios físicos. El objetivo de esta entrevista es conocer cómo llevas actualmente el control de tu alimentación, qué dificultades encuentras y cómo resuelves las situaciones que se presentan en tu rutina.

La información que compartas nos ayudará a comprender mejor tus necesidades y revisar los requerimientos del sistema. ¿Me permites tomar algunas notas durante la entrevista para no perder detalles importantes?

### Contexto

**C1.** Cuéntame cómo es un día habitual para ti y qué actividades ocupan la mayor parte de tu tiempo.

**C2.** Cuéntame qué lugar ocupa el cuidado de tu alimentación y tu condición física dentro de tus actividades cotidianas y cómo organizas actualmente estas actividades.

### El proceso actual

**P1.** Cuéntame la última vez que intentaste llevar un registro de tu alimentación y tus cambios físicos. ¿Qué hiciste desde que comenzaste hasta que terminaste?

**P2.** Cuando quieres saber si estás avanzando hacia tus objetivos, ¿qué haces para revisar tus resultados? Cuéntame un ejemplo reciente y las herramientas que utilizaste.

**P3.** Cuando necesitas orientación sobre tu alimentación, ¿cómo la buscas actualmente? Describe la última ocasión en que tuviste una duda, qué hiciste para resolverla y qué ocurrió después.

### Dificultades del proceso actual

**D1.** ¿Qué parte de llevar un seguimiento de tu alimentación y tus cambios físicos te quita más tiempo o te resulta más complicada? Cuéntame una situación en la que hayas tenido esa dificultad.

**D2.** ¿Qué ocurre cuando intentas mantener tus registros al día, pero tus actividades no te lo permiten? ¿Cómo afecta eso a la manera en que revisas tus avances?

### Excepciones y situaciones especiales

**E1.** Cuéntame qué ocurrió la última vez que olvidaste registrar tu alimentación o dejaste de hacerlo durante varios días. ¿Cómo continuaste después y qué hiciste con la información que faltaba?

**E2.** Describe alguna ocasión en la que no pudiste obtener una medición o registrar un dato como acostumbrabas. ¿Qué hiciste para continuar con tu seguimiento?

### Verificación de supuestos

Estas preguntas permiten comprobar los supuestos relacionados con los requerimientos funcionales y no funcionales de NutriGo!. Se busca conocer las necesidades reales del usuario sin dar por hecho que las funcionalidades propuestas son necesarias o suficientes.

#### Requerimientos funcionales

**V1. Registro diario de información**

**Supuesto:** El usuario necesita registrar su alimentación, peso, consumo de agua, horas de sueño y actividad física de manera práctica.

**Pregunta:** Cuéntame qué información necesitas registrar para llevar un seguimiento de tu alimentación y tus cambios físicos. ¿Cómo obtienes actualmente esos datos y qué dificultades encuentras al registrarlos?

**V2. Consulta del progreso**

**Supuesto:** Consultar indicadores como el peso, el IMC, el porcentaje de grasa y el porcentaje de músculo permite conocer la evolución de los cambios físicos.

**Pregunta:** ¿Qué información utilizas actualmente para evaluar tus avances y cómo interpretas los cambios que observas? Cuéntame un ejemplo de cómo revisaste tus resultados.

**V3. Plan alimenticio personalizado**

**Supuesto:** Un plan alimenticio debe considerar los objetivos, las preferencias, las alergias y las restricciones alimenticias del usuario.

**Pregunta:** Cuando organizas tu alimentación, ¿qué aspectos de tu situación personal necesitas tener en cuenta y cómo influyen en lo que decides comer?

**V4. Actualización semanal del plan**

**Supuesto:** Utilizar los registros de la semana anterior permite ajustar el plan alimenticio de la siguiente semana.

**Pregunta:** Cuéntame cómo ajustas actualmente tu alimentación cuando notas cambios en tus resultados o en tus actividades. ¿Qué información tomas en cuenta para decidir qué cambiar?

**V5. Solicitud de consultas con el nutriólogo**

**Supuesto:** Consultar la disponibilidad del nutriólogo y solicitar una cita remota facilita el acceso a orientación nutricional.

**Pregunta:** Cuéntame cómo organizas actualmente tus consultas nutricionales y qué haces cuando necesitas orientación, pero no puedes acudir presencialmente o no encuentras un horario disponible.

#### Requerimientos no funcionales

**V6. Usabilidad**

**Supuesto:** El sistema debe ser fácil de utilizar y requerir poco esfuerzo para registrar y consultar información.

**Pregunta:** Cuéntame qué dificultades encuentras al utilizar las herramientas que tienes disponibles para llevar tu seguimiento. ¿Qué sucede cuando una tarea requiere más tiempo o pasos de los que esperabas?

**V7. Seguridad y privacidad**

**Supuesto:** La información personal y nutricional debe estar protegida y disponible únicamente para personas autorizadas.

**Pregunta:** ¿Qué aspectos te preocupan al compartir tu información personal y nutricional mediante una aplicación? ¿Qué esperas que ocurra con tus datos y quiénes deberían poder consultarlos?

**V8. Integridad de los datos**

**Supuesto:** La información registrada debe ser correcta, coherente y suficientemente completa para consultar el progreso del usuario.

**Pregunta:** Cuéntame qué haces cuando descubres que un registro o una medición es incorrecta o está incompleta. ¿Cómo afecta esa situación a la revisión de tus avances?

### Cierre

Para asegurarme de haber entendido, voy a resumir los puntos principales que hemos comentado: las actividades que realizas actualmente, las dificultades que encuentras, las situaciones excepcionales que has experimentado y las necesidades que mencionaste.

¿Hay algo de lo que hemos hablado que haya entendido incorrectamente o algún detalle importante que no te haya preguntado?

Muchas gracias por tu tiempo y por compartir tu experiencia. La información será útil para revisar los requerimientos de NutriGo! y comprobar que respondan a las necesidades identificadas.

---

## 2. Bitácora de la entrevista

### Supuestos confirmados

- **Registro diario:** Se comentó que registrar manualmente las comidas y otros datos toma tiempo y que, cuando hay muchas actividades, es fácil olvidarse de hacerlo. Por eso, se prefiere que los datos disponibles se obtengan automáticamente.
- **Consulta del progreso:** Se mencionó que revisar únicamente el peso no siempre es suficiente y que también sería útil consultar indicadores como el IMC, el porcentaje de grasa y el porcentaje de músculo.
- **Plan alimenticio personalizado:** Se dijo que un plan debe considerar los objetivos personales, las preferencias, las alergias y las restricciones alimenticias.
- **Actualización semanal:** Se expresó que sería útil ajustar el plan de la siguiente semana considerando los registros y los resultados anteriores.
- **Consultas con un nutriólogo:** Se comentó que la falta de tiempo dificulta acudir presencialmente y que sería conveniente consultar horarios y solicitar citas remotas.
- **Usabilidad, privacidad e integridad:** Se identificó que el registro debe ser sencillo, que los datos personales deben estar protegidos y que la información incompleta puede dificultar la revisión del progreso.

### Supuestos que resultaron falsos o necesitan modificarse

- **Recuperar los registros olvidados:** Se descubrió que no siempre es posible completar los registros de días anteriores, porque la persona puede no recordar qué comió ni las cantidades. Por ello, debe poder continuar con el seguimiento sin completar todos los datos faltantes.
- **Registro automático:** Se confirmó que la automatización sería útil, pero no todos los datos pueden obtenerse de dispositivos. Algunos, como los alimentos consumidos, podrían requerir registro manual.

### Hallazgos inesperados

- **Abandono del seguimiento:** Se identificó que acumular registros pendientes puede hacer que la persona deje de llevar el control.
- **Importancia de otros indicadores:** Se encontró que el peso por sí solo no cubre todas las necesidades para revisar los cambios físicos.
- **Influencia del tiempo:** Se observó que las actividades cotidianas dificultan tanto mantener los registros como organizar la alimentación y acudir a consultas.

### Supuestos que no se verificaron en la entrevista

- **Informes en PDF:** No se preguntó si la persona necesita descargar sus avances y planes de alimentación en PDF de forma semanal o mensual.
- **Conservación de los registros:** No se preguntó si conservar los registros diarios durante 30 días y después sustituirlos por un resumen mensual es suficiente para revisar el progreso.
- **Funciones del nutriólogo:** La entrevista se hizo desde el punto de vista del paciente, por lo que no se verificó cómo el nutriólogo consulta el progreso de sus pacientes ni cómo atiende las solicitudes dentro de sus horarios.

Estos supuestos se mantienen en los requisitos, pero quedan pendientes de confirmar con el cliente.

---

## 3. Ficha de Dominio

*Para quien hace de cliente durante la entrevista*

### QUIÉN ERES

Eres una persona que busca cuidar su salud, mejorar sus hábitos alimenticios y llevar un mejor control de sus cambios físicos. Te interesa recibir orientación nutricional y conocer tus avances, pero no tienes suficiente tiempo para acudir presencialmente con un nutriólogo debido a tus actividades diarias.

Has intentado utilizar aplicaciones de control nutricional para registrar tus comidas y monitorear tu progreso. Sin embargo, se te suele olvidar registrar manualmente tus datos, por lo que la información queda incompleta y tu control alimenticio no siempre es acertado.

### CÓMO ES TU DÍA

Durante el día tienes diferentes actividades y responsabilidades que hacen difícil dedicar tiempo a registrar todo lo que comes, cuánta agua consumes, cuánto duermes y qué actividad física realizas.

Te gustaría llevar un seguimiento más preciso de tu alimentación y tus cambios físicos, pero registrar cada dato manualmente resulta poco práctico. Preferirías que la aplicación obtuviera automáticamente toda la información posible de dispositivos compatibles, como una báscula inteligente, un reloj o una banda de actividad, y tener que anotar únicamente los datos que no puedan obtenerse mediante esos instrumentos.

### REGLAS QUE CONOCES Y NO VAS A DECIR SI NO TE PREGUNTAN

- Buscas mejorar tus hábitos alimenticios y cuidar tu salud, pero necesitas que el seguimiento se adapte a tus actividades diarias.
- Se te olvida registrar manualmente tus comidas y otros datos, por lo que no siempre cuentas con información completa para conocer tu progreso.
- Prefieres que los datos se obtengan automáticamente de los dispositivos compatibles y registrar manualmente solo la información que no pueda obtenerse de ellos.
- Te interesa consultar la evolución de tu peso, IMC, porcentaje de grasa y porcentaje de músculo para conocer tus cambios físicos.
- Consideras importante que un plan alimenticio tome en cuenta tus objetivos, preferencias, alergias y restricciones alimenticias.
- Te gustaría recibir un plan alimenticio personalizado que se ajuste semanalmente según tus registros y resultados.
- Consideras que tu información personal y nutricional es privada y que solo las personas autorizadas deberían poder consultarla.
- Debido a tu falta de tiempo, prefieres poder consultar la disponibilidad de un nutriólogo y solicitar una cita remota en lugar de tener que acudir presencialmente.

### UNA EXCEPCIÓN QUE OCURRE A VECES

Hay días en los que estás ocupado y olvidas registrar si cumpliste con tu alimentación o alguna medición que no se puede registrar automáticamente. Cuando intentas revisar tu progreso, descubres que faltan registros.

Puede suceder que alguno de tus dispositivos no registre correctamente la información o que no sea compatible con la aplicación. En ese caso, tendrías que registrar manualmente los datos que no se puedan obtener automáticamente.

Si necesitas orientación nutricional, pero el nutriólogo no está disponible, preferirías consultar sus horarios y solicitar una cita remota para recibir atención cuando sea posible.

### LO QUE TE MOLESTA DE CÓMO LO HACES HOY

Te molesta tener que registrar manualmente cada comida y los demás datos relacionados con tu salud, porque requiere tiempo y constancia. Aunque intentas utilizar aplicaciones de control nutricional, terminas olvidando algunos registros y la información acumulada no representa con precisión tus hábitos.

Esto dificulta que conozcas tu progreso real y que puedas identificar cómo ha cambiado tu alimentación con el paso de las semanas. Además, acudir presencialmente con un nutriólogo no siempre es viable debido a tus horarios.

Por eso, buscas una forma de llevar un seguimiento más práctico y preciso, en el que la mayor cantidad posible de información se registre automáticamente y solo tengas que introducir manualmente los datos que no puedan obtenerse de tus dispositivos.

### CÓMO RESPONDER

Contesta solo lo que te pregunten. No reveles toda la información de esta ficha desde el principio.

Responde de manera natural, como una persona que intenta cuidar su salud, pero tiene dificultades para mantener un registro constante. Si te preguntan por una experiencia concreta, explica qué ocurrió y cómo lo resolviste.

Si te preguntan qué información te gustaría registrar automáticamente, menciona que prefieres evitar el registro manual siempre que existan dispositivos compatibles que puedan proporcionar esos datos.

No inventes funciones que no estén contempladas en NutriGo! ni propongas detalles técnicos de implementación. Si te preguntan por una situación que no has vivido, explica qué harías en ese caso.
