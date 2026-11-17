# ADR-004 · Datos de los dispositivos por el servicio de cada fabricante, con un conector por fabricante

| Campo | Valor |
|---|---|
| **Estado** | Aceptada |
| **Fecha de la decisión** | 20/10/2026 (Actividad 2) |
| **Registrada** | 17/11/2026 |
| **Autor** | Diego Valdovinos Rodríguez |
| **Diagramas** | [C4 niveles 1 a 3](diagramas.md), [componentes](paquetes-y-componentes.md), [diagrama de secuencia](diagrama-de-secuencia.md) |

## Contexto

El paciente vincula un reloj inteligente, una banda de actividad, una báscula inteligente u otro dispositivo compatible (RF-011), y el sistema obtiene de ellos lo que cada uno mide (RF-012):
- La báscula da peso, grasa y músculo.
- El reloj y la banda dan sueño y actividad.

En la entrevista se confirmó que el paciente quiere registrar a mano solo lo que no se pueda obtener de sus dispositivos. También se descubrió que a veces el dispositivo no registra bien, así que el paciente debe poder capturar y corregir (RF-008, RF-010).

Cada fabricante tiene su propio servicio y responde a su manera. En la Actividad 1 este fue el candidato a polimorfismo: si el registro preguntara qué tipo de dispositivo es para saber qué leer, cada dispositivo nuevo sería una condición más.

## Decisión

- **La app nunca habla con los dispositivos.** Los datos llegan del servicio en la nube de cada fabricante, al que NutriGo! se conecta con OAuth cuando el paciente vincula el dispositivo.
- **Solo el módulo Seguimiento habla con los fabricantes,** a través de un conector por fabricante. Todos los conectores contestan la misma pregunta, "dame los datos del día", y cada uno la traduce a su servicio. Es el patrón **Adapter**, y del lado del dominio, cada Dispositivo entrega sus datos con la misma operación (**Strategy**).
- **La validación vive en el registro, no en el dispositivo.** `RegistroDiario.validarRango()` revisa igual el dato que llega de la báscula y el que captura el paciente (RNF-CON-002).

## Alternativas consideradas

- **Leer los dispositivos por Bluetooth desde la app.** El paciente tendría que tener la báscula cerca para registrar, y la app tendría que conocer el protocolo de cada modelo.
- **Un solo componente con una condición por fabricante.** Funciona con dos fabricantes, pero cada uno nuevo obliga a abrir el registro diario y probar todo otra vez.

## Consecuencias

### Positivas

- **Un fabricante nuevo es un conector nuevo.** El registro diario no se toca, como el anillo inteligente de la Actividad 1.
- **El registro funciona aunque el paciente no tenga el dispositivo cerca.**
- **Una sola validación** para lo que llega del dispositivo y lo que se captura a mano.

### Negativas

- **NutriGo! depende de servicios que no controla.** Si un fabricante cambia su servicio o se cae, sus datos dejan de llegar sin aviso. El respaldo es la captura manual del RF-008, que el paciente hace con más esfuerzo.
- **Hay que guardar y proteger un token de acceso por dispositivo** (`token_acceso` en DISPOSITIVO). Si se filtra, alguien podría leer los datos del paciente en el servicio del fabricante.
- **Solo se obtiene lo que el fabricante expone, y cuando lo expone.** Un dispositivo sin servicio en la nube no se puede vincular.
