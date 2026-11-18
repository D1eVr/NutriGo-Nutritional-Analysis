# Foro · Control de cambio y análisis de impacto · SC-001

Versión corta del análisis de impacto publicada en el foro del curso el 17/11/2026. El documento completo es [ai-001-recordatorio-para-registrar-el-dia.md](ai-001-recordatorio-para-registrar-el-dia.md).

---

**SC-001 · Recordatorio para registrar el día · NutriGo! (Diego Valdovinos)**

**Qué me pidieron.** Jimena Morales, mi dupla, escribió: "Quiero que si ya es tarde y no he registrado mi día, la app me mande un recordatorio al celular. Y quiero poder elegir a qué hora me llega, o apagarlo si no lo quiero."

**Cómo lo traduje.** Es un requisito nuevo, el RF-034, y el caso de uso CU-21 "Configurar el recordatorio del registro diario". La clase Paciente gana `horaRecordatorio`, `recordatorioActivo` y `configurarRecordatorio(hora, activo)`, y para saber si el día está completo uso `estaCompleto()`, que RegistroDiario ya tiene. Las Tareas programadas ganan el trabajo de revisar a quién le toca, y el aviso sale por el Notificador, el mismo canal al teléfono que ya pide el RF-033. También cambian el diagrama de casos de uso, la pantalla de Perfil en Figma, la tabla USUARIO y se agrega un diagrama de secuencia.

**El impacto indirecto más caro.** Mis Tareas programadas (ADR-003) traen los datos de los dispositivos cada cierto tiempo, no en tiempo real. Si a las 21:00 el reloj todavía no sincroniza el sueño, el servidor ve el día incompleto y el aviso le llega a alguien que ya había cumplido. Sincronizar a cada paciente antes del aviso lo evitaría, pero multiplicaría las llamadas a los fabricantes. Y si ese proceso se detiene, ahora tampoco llegan los recordatorios.

**Estimación: 17 horas.** 5 de diseño y documentación, 8 de implementación y 4 de pruebas. La implementación la comparé con la notificación del RF-033, lo más parecido que tengo diseñado: 3 horas del adaptador al teléfono, 2 del trabajo programado, 2 de la pantalla y 1 para abrir el registro desde el aviso. Supuse que lo hago yo solo, sobre el monolito modular actual.

**Recomendación: aceptar, con prioridad media, después del RF-033.** Ataca el olvido que en mi entrevista salió como causa de abandono, y no toca ninguna regla que ya funcione. Después del RF-033 baja a 14 horas, porque el canal al teléfono ya estaría hecho. Antes confirmaría con Jimena que el aviso considere solo lo que ya llegó al servidor.
