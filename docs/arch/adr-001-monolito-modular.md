# ADR-001 · Monolito modular con cuatro módulos

| Campo | Valor |
|---|---|
| **Estado** | Aceptada |
| **Fecha de la decisión** | 20/10/2026 (Actividad 2) |
| **Registrada** | 17/11/2026 |
| **Autor** | Diego Valdovinos Rodríguez |
| **Diagramas** | [C4 nivel 2 y 3](diagramas.md), [paquetes y componentes](paquetes-y-componentes.md) |

## Contexto

NutriGo! lo construye una sola persona durante el semestre. No hay equipos que coordinar, y por la ley de Conway lo natural es un solo programa.

Los requisitos todavía se mueven. Después del primer parcial se agregaron el RF-031, el RF-032 y el RF-033 a partir del prototipo, y el RF-016 se partió en dos (generar el plan y consultarlo, RF-030). Una frontera entre servicios que se dibuja hoy probablemente habría que moverla mañana.

Al mismo tiempo, las reglas de negocio necesitan fronteras de verdad para no repartirse, porque cada una tiene un solo dueño:
- Solo el horario decide si un espacio está libre (RF-020).
- Solo el registro decide si un valor está en rango (RNF-CON-002).
- Solo el paciente decide qué nutriólogo lo puede ver (RNF-SEG-002).

## Decisión

Un solo programa desplegable, la API de NutriGo! en Node.js con Express, partido por dentro en cuatro módulos y un paquete común:

| Módulo | Qué guarda |
|---|---|
| Cuentas | Cuentas, pacientes, autorizaciones y administración |
| Seguimiento | Registro diario, dispositivos, progreso y resumen mensual |
| Plan alimenticio | Preferencias y plan semanal |
| Consultas | Horarios y citas |
| Común | El Notificador, sin reglas de negocio |

Hay dos reglas para que la partición sea real:
- **Las dependencias bajan y no regresan.** Plan alimenticio y Consultas dependen de Seguimiento y de Cuentas; Cuentas no depende de ningún módulo. No hay ciclos.
- **Un módulo nunca lee las tablas de otro.** Le pregunta por su interfaz: ICuentas, ISeguimiento o INotificador.

## Alternativas consideradas

- **Monolito sin módulos.** Es lo más rápido de empezar, pero las reglas terminarían repetidas: la validación del rango en la pantalla, en la sincronización y en la corrección. Un cambio en una regla obligaría a tocar varios lugares.
- **Microservicios, uno por módulo.** Serían cuatro servicios que operar, desplegar y vigilar para una sola persona. Cada pregunta entre módulos viajaría por red, y se perdería la transacción en operaciones que hoy tocan dos módulos a la vez, como solicitar una cita: crear la cita, apartar el espacio y registrar la autorización. Con fronteras que todavía se mueven, el resultado sería un monolito distribuido.

## Consecuencias

### Positivas

- **Un solo despliegue y una sola base de datos.** Lo puede operar una persona.
- **Las operaciones que cruzan módulos siguen siendo una transacción.** Si falla apartar el espacio, no queda una cita a medias.
- **Las fronteras ya están listas para separarse.** Cada módulo habla por su interfaz, así que sacarlo después es mover un módulo, no rehacer el sistema.

### Negativas

- **Si una parte se cae o se satura, se cae todo.** El momento de riesgo son los lunes: generar el plan de todos los pacientes compite con los pacientes que registran su día. Si eso pasa de los 2 minutos del RNF-USA-001, la decisión se rompe.
- **Las fronteras no las cuida la red, las cuida la disciplina.** Un módulo que lee directo la tabla de otro compila y funciona igual, y nadie se entera hasta que algo cambia. Hay que revisarlo en cada cambio.
- **Cualquier cambio, por pequeño que sea, obliga a volver a desplegar toda la API.** Tampoco se puede escalar un solo módulo: se escala todo o nada.

## Cuándo revisarla

Si los lunes, mientras se generan los planes, registrar el día tarda más de lo que permite el RNF-USA-001, el primer módulo que se saca es Plan alimenticio, y los demás se quedan igual.
