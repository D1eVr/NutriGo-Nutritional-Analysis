# Visión del producto

> **Plantilla del curso · Ingeniería de Software I · SIS3407**
> Este documento es el primer entregable del semestre y la base de todo lo que viene después.
> Se entrega completo en la **semana 4** y se presenta ante el grupo.

---

> **Autor:** Diego Valdovinos Rodríguez

> **Fecha de la última versión:** 20/08/2026

> **Repositorio**: https://github.com/D1eVr/NutriGo-Nutritional-Analysis.git

---

## 1. Descripción del sistema

> **Nombre del sistema:** NutriGo!

> **Descripción:** Una aplicación móvil que permite a las personas llevar un seguimiento diario de su alimentación y de sus cambios físicos, consultar su progreso y recibir un plan alimenticio personalizado que se adapta a sus resultados. También permite utilizar información proveniente de dispositivos existentes, como relojes, bandas de actividad y básculas inteligentes, para complementar el seguimiento del usuario.

---

## 2. Problema y usuarios

> **El problema:** Hay personas que quieren regular su alimentación y llevar un seguimiento de sus cambios físicos, pero por sus actividades diarias les cuesta mantener un control constante de sus datos, alimentación y progreso.

> **Cómo se resuelve hoy sin el sistema:** Actualmente, las personas pueden utilizar notas del teléfono, hojas de cálculo, aplicaciones diferentes o llevar sus registros de memoria. También pueden acudir periódicamente con un nutriólogo, pero el seguimiento entre consultas puede ser limitado.

> **Usuarios del sistema:**

| Tipo de usuario | Qué necesita del sistema | Qué le preocupa |
|---|---|---|
| Paciente/Usuario | Registrar sus datos, consultar su plan, revisar su progreso y comunicarse con un nutriólogo. | Que sea rápido, sencillo y que sus datos sean confiables. |
| Nutriólogo | Consultar el progreso de sus pacientes, revisar sus registros y atender solicitudes dentro de sus horarios establecidos. | Contar con información suficiente y confiable para dar seguimiento a sus pacientes. |
| Administrador | Gestionar usuarios y mantener el funcionamiento del sistema. | La seguridad y el acceso correcto a la información. |

> **Un conflicto entre usuarios:** El paciente puede solicitar comunicarse con un nutriólogo en cualquier momento, pero este solo puede atenderlo durante sus horarios establecidos, lo que puede generar una espera para el paciente.

---

## 3. Alcance

### Dentro del alcance

> - Registra el peso en ayunas, las horas de sueño, el agua consumida, la alimentación y la actividad física diariamente, ya sea de forma automática o manual.
> - Muestra el progreso del usuario mediante su peso, IMC, porcentaje de grasa y porcentaje de músculo.
> - Permite registrar preferencias, alergias y alimentos que no le gustan al usuario para hacer el plan de alimentación lo más personalizado posible.
> - Proporciona al usuario los horarios disponibles del nutriólogo para atender sus dudas o solicitar una cita remota.
> - Se conecta con dispositivos existentes compatibles, como relojes inteligentes, bandas de actividad y básculas inteligentes, para recibir datos del usuario.
> - Genera un plan alimenticio personalizado según los datos, objetivos, preferencias y restricciones del usuario.
> - Ajusta el plan alimenticio semanal según los resultados registrados.
> - Permite al nutriólogo consultar el progreso de sus pacientes y atender sus solicitudes dentro de los horarios establecidos.
> - Permite al usuario descargar en PDF sus avances y planes de alimentación de forma semanal o mensual.

### Explícitamente fuera del alcance

> - No realiza diagnósticos médicos.
> - No procesa pagos ni cobros dentro de la aplicación durante el desarrollo del semestre.
> - No diseña ni fabrica dispositivos propios para medir los datos del usuario.

> **Por qué queda fuera:** El diseño y fabricación de dispositivos propios queda fuera porque requiere conocimientos, recursos y tiempo adicionales que no serían suficientes durante el periodo correspondiente a la materia. NutriGo! se enfocará en utilizar conexiones con dispositivos existentes y compatibles para obtener los datos necesarios.

---

## 4. Tipo de sistema y restricciones

> **Tipo de sistema:** De información.

> **Por qué es de ese tipo:** NutriGo! registra, consulta y utiliza información de los usuarios para mostrar su progreso, generar su plan alimenticio y facilitar el seguimiento con un nutriólogo. También puede recibir información proveniente de dispositivos compatibles.

**Atributos de calidad que impone:**

| Atributo | Por qué importa en mi caso | Qué pasa si no se cumple |
|---|---|---|
| Usabilidad | El usuario debe poder registrar sus datos diarios y consultar su información de forma rápida y sencilla. | El usuario puede dejar de usar la aplicación por ser complicada. |
| Seguridad | Se manejan datos personales y de seguimiento del usuario. | Personas no autorizadas podrían acceder a información privada. |
| Integridad de los datos | Los registros deben mantenerse correctos para que el seguimiento y los cambios del plan sean confiables. | El sistema podría mostrar un progreso incorrecto o generar un plan con información equivocada. |

**Reglas de negocio que ya identifiqué:**

> **1.** El plan alimenticio se genera tomando en cuenta los datos básicos, objetivos, preferencias y restricciones alimenticias del usuario.

> **2.** Cada semana el sistema analiza los registros de la semana anterior para generar el siguiente plan alimenticio.

> **3.** Los datos diarios se conservan durante 30 días y después se reemplazan por un resumen mensual que el usuario puede descargar.

---

## 5. Ciclo de vida elegido

> **Modelo elegido:** Ágil.

> **Por qué le conviene a este proyecto:** NutriGo! puede evolucionar conforme se conozcan mejor las necesidades de los usuarios y se prueben las funciones de la aplicación. El sistema puede comenzar con las funciones principales de registro, seguimiento y generación de planes, y posteriormente incorporar nuevas funciones de acuerdo con los resultados obtenidos y la retroalimentación de los usuarios.

> El modelo ágil permite desarrollar el sistema en ciclos cortos, validar cada parte antes de continuar y realizar cambios sin tener que replantear todo el proyecto. Esto resulta conveniente para NutriGo! porque algunas funcionalidades pueden evolucionar durante su desarrollo, especialmente las relacionadas con la conexión a dispositivos existentes y la comunicación entre usuarios y nutriólogos.

> Además, permite mantener controlado el alcance principal durante el semestre y dejar para etapas posteriores funcionalidades de mayor complejidad. Entre estas se encuentran el desarrollo de dispositivos propios para obtener datos y la incorporación de una pasarela de pagos para ofrecer servicios bajo una metodología SaaS.

### Alternativas descartadas

> **Alternativa 1:** Cascada.

> **Por qué la descarté:** El modelo en cascada supone que los requisitos pueden definirse con suficiente estabilidad desde el principio. En NutriGo! algunas funciones pueden modificarse conforme se pruebe el sistema y se reciba retroalimentación de los usuarios, por lo que realizar cambios al final de todas las etapas sería menos conveniente.

> **Alternativa 2:** Modelo V.

> **Por qué la descarté:** El Modelo V sería útil si NutriGo! tuviera requisitos completamente estables y necesitara una validación formal de cada etapa. Sin embargo, el proyecto busca evolucionar sus funciones conforme se conozcan mejor las necesidades de los usuarios, por lo que un modelo que facilite iteraciones y cambios resulta más adecuado.
