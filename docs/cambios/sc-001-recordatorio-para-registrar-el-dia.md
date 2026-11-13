# SC-001: Recordatorio para registrar el día

---

## Estado

Propuesta · 12/11/2026

## Quién la pide

<Jimena Morales>, como dupla de revisión.

## Qué se pide

Quiero que si ya es tarde y no he registrado mi día, la app me mande un recordatorio al celular. Y quiero poder elegir a qué hora me llega, o apagarlo si no lo quiero.

## Por qué

En la entrevista salió que a la persona se le olvida registrar cuando tiene muchas actividades, y que al juntarse varios días sin registro termina dejando el seguimiento. NutriGo ya deja registrar el día sin completar los anteriores, pero no hace nada para que el olvido no pase: hoy el paciente se acuerda de registrar solo si abre la app por su cuenta.

## Qué pasa si no se hace

Habrá más días "Sin registro". Eso deja huecos en la gráfica del progreso, hace que el plan de cada lunes se ajuste con menos días de información y que el nutriólogo vea pacientes de los que casi no hay datos. No rompe nada que hoy funcione, pero el problema que el sistema quería resolver sigue pasando.

## Tipo de mantenimiento

- [ ] **Correctivo** · algo está mal y hay que repararlo
- [ ] **Adaptativo** · el entorno cambió y hay que ajustarse
- [x] **Perfectivo** · funciona, pero puede ser mejor
- [ ] **Preventivo** · todavía no falla, pero va a fallar

Registrar el día ya funciona; lo que se pide es que el sistema ayude a que el paciente no se olvide de hacerlo.

## Criterio de aceptación

- Si el paciente elige las 21:00 y a esa hora su registro del día no está completo, le llega un recordatorio a su celular.
- Si a esa hora su día ya está completo, no le llega nada.
- Si apaga el recordatorio, no le vuelve a llegar ninguno hasta que lo prenda otra vez.
- Al tocar el recordatorio se abre el registro de hoy.
- El recordatorio no muestra ningún dato de salud, ni su peso ni lo que comió, porque puede aparecer en la pantalla bloqueada.

## Prioridad que sugiere el solicitante

Media. Ataca justo el problema que salió en la entrevista y que puede hacer que el paciente abandone la app, pero el sistema funciona sin él y no bloquea ninguna otra función.

---

*La solicitud describe qué se quiere, nunca cómo hacerlo. El cómo se decide en el análisis de impacto, y lo decide quien conoce el diseño.*
