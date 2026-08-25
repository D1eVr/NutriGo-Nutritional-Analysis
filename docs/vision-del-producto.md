# Visión del producto

> **Plantilla del curso · Ingeniería de Software I · SIS3407**
> Este documento es el primer entregable del semestre y la base de todo lo que viene después.
> Se entrega completo en la **semana 4** y se presenta ante el grupo.
>
> **Cómo usarla:** copia este archivo a tu repositorio como `docs/vision-del-producto.md`, borra las instrucciones en gris de cada apartado y escribe tu contenido en su lugar. Conserva los títulos.

---

> **Autor:** Diego Valdovinos Rodríguez

> **Fecha de la última versión:** 20/08/2026

> **Repositorio**: https://github.com/D1eVr/NutriGo-Nutritional-Analysis.git

---

## 1. Descripción del sistema

*Instrucción: nombre del sistema y qué hace, en un párrafo que cualquier persona entienda sin ser del área. Si necesitas usar una palabra técnica para explicarlo, todavía no está listo.*

> **Nombre del sistema:** NutriGo!

> **Descripción:** Una aplicación que permite a las personas llevar un seguimiento diario de su alimentación y de sus cambios físicos, consultar su progreso y recibir un plan alimenticio personalizado que se adapta a sus resultados.

---

## 2. Problema y usuarios

*Instrucción: qué problema resuelve, a quién le sirve y, muy importante, qué hace esa gente hoy para arreglárselas sin el sistema. Esa última parte es la que revela el problema real.*

> **El problema:** Hay personas que quieren regular su alimentación y llevar un seguimiento de sus cambios físicos, pero por sus actividades diarias les cuesta mantener un control constante de sus datos, alimentación y progreso.

> **Cómo se resuelve hoy sin el sistema:** Actualmente, las personas pueden utilizar notas del teléfono, hojas de cálculo, aplicaciones diferentes o llevar sus registros de memoria. También pueden acudir periódicamente con un nutriólogo, pero el seguimiento entre consultas puede ser limitado.

> **Usuarios del sistema:**

| Tipo de usuario | Qué necesita del sistema | Qué le preocupa |
|---|---|---|
| Paciente/Usuario | Registrar sus datos y consultar su plan y progreso. | Que sea rápido y fácil de usar. |
| Nutriologo | Revisar el progreso y atender dudas o solicitudes de sus pacientes. | Contar con información suficiente y confiable. |
| Administrador | Gestionar usuarios y mantener el sistema. | La seguridad y el acceso correcto a la información. |

*Instrucción: necesitas al menos dos tipos de usuario con necesidades distintas. Si los dos quieren exactamente lo mismo, probablemente sean el mismo usuario.*

> **Un conflicto entre usuarios:** El paciente puede solicitar una cita en cualquier momento, mientras que el nutriólogo debe atender las solicitudes de acuerdo con su disponibilidad y horario de trabajo.

*Instrucción: describe algo que un usuario quiera y que a otro le estorbe. Ahí está tu primera decisión de diseño real.*

---

## 3. Alcance

*Instrucción: lo que escribes en "fuera del alcance" es lo que después evita que el proyecto crezca sin control. Sé específico: "reportes" no dice nada, "reportes de ventas mensuales exportables a PDF" sí.*

### Dentro del alcance

- Registra el peso en ayunas, las horas de sueño, el agua consumida y la alimentación.
- Muestra el progreso del usuario mediante su peso, IMC, porcentaje de grasa y porcentaje de músculo.
- Recibe datos de dispositivos compatibles, como relojes, bandas o básculas inteligentes.
- Genera un plan alimenticio personalizado según los datos, objetivos, preferencias y restricciones del usuario.
- Ajusta el plan alimenticio semanal según los resultados registrados.

### Explícitamente fuera del alcance

- No realiza diagnósticos médicos.
- No procesa pagos ni cobros dentro de la aplicación.
- No desarrolla dispositivos propios para medir los datos del usuario.

> **Por qué queda fuera:** El desarrollo de dispositivos propios queda fuera porque requiere conocimientos, recursos y tiempo adicionales que no son necesarios para desarrollar la función principal de NutriGo! durante el semestre.

---

## 4. Tipo de sistema y restricciones

---

## 4. Tipo de sistema y restricciones

*Instrucción: identifica de qué tipo es tu sistema y qué te obliga a garantizar ese tipo. Un sistema de información y un sistema crítico no se diseñan igual.*

> **Tipo de sistema:** De información

> **Por qué es de ese tipo:** NutriGo! registra, consulta y utiliza información de los usuarios para mostrar su progreso y generar su plan alimenticio. También puede recibir información proveniente de dispositivos compatibles.

**Atributos de calidad que impone:**

| Atributo | Por qué importa en mi caso | Qué pasa si no se cumple |
|---|---|---|
| Usabilidad | El usuario debe poder registrar sus datos diarios y consultar su información de forma rápida y sencilla. | El usuario puede dejar de usar la aplicación por ser complicada. |
| Seguridad | Se manejan datos personales y de seguimiento del usuario. | Personas no autorizadas podrían acceder a información privada. |
| Integridad de los datos | Los registros deben mantenerse correctos para que el seguimiento y los cambios del plan sean confiables. | El sistema podría mostrar un progreso incorrecto o generar un plan con información equivocada. |

**Reglas de negocio que ya identifiqué:**

*Instrucción: reglas que no son obvias desde fuera y que alguien que conoce el dominio tendría que explicarte. Si no encuentras ninguna, tu caso puede ser demasiado simple.*

> **1.** El plan alimenticio se genera tomando en cuenta los datos básicos, objetivos, preferencias y restricciones alimenticias del usuario.

> **2.** Cada semana el sistema analiza los registros de la semana anterior para generar el siguiente plan alimenticio.

> **3.** Los datos diarios se conservan durante 30 días y después se reemplazan por un resumen mensual que el usuario puede descargar.

---

## 5. Ciclo de vida elegido

*Instrucción: este apartado se trabaja en la semana 3, después de ver los modelos de desarrollo. La justificación pesa más que la elección: no hay un modelo correcto, hay uno defendible para tu caso.*

**Modelo elegido:**

**Por qué le conviene a este proyecto:**

*Instrucción: argumenta con las características reales de tu caso. Estabilidad de los requisitos, disponibilidad del cliente, nivel de riesgo, tamaño del equipo, frecuencia de entregas esperada.*

### Alternativas descartadas

**Alternativa 1:**

*Por qué la descarté:*

**Alternativa 2:**

*Por qué la descarté:*

---

## Antes de entregar

Reviso que el documento cumpla lo siguiente:

- [ ] La descripción del apartado 1 se entiende sin ser del área
- [ ] Hay al menos dos tipos de usuario con necesidades distintas
- [ ] Identifiqué un conflicto real entre usuarios
- [ ] El alcance dice qué queda fuera, no solo qué queda dentro
- [ ] Las exclusiones son específicas, no genéricas
- [ ] Identifiqué el tipo de sistema y al menos dos atributos de calidad
- [ ] Anoté al menos tres reglas de negocio no obvias
- [ ] Justifiqué el ciclo de vida contra dos alternativas descartadas
- [ ] El documento está en mi repositorio y se puede leer desde el navegador
- [ ] Borré todas las instrucciones en cursiva de la plantilla