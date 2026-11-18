# Diagramas de arquitectura · NutriGo!

| Campo | Valor |
|---|---|
| **Sistema** | NutriGo!  |
| **Autor** | Diego Valdovinos Rodríguez |
| **Versión** | 1.1 |
| **Fecha** | 17/11/2026 |
| **Fuentes** | `docs/vision-del-producto.md`, `docs/especificacion-requisitos.md` |

Los diagramas van como texto de Mermaid, igual que el código: se comparan entre versiones y se revisan en el mismo diff. Para verlos, pega cada bloque en [mermaid.live](https://mermaid.live).

## Dónde está cada diagrama

| Diagrama | Tipo | Archivo |
|---|---|---|
| Casos de uso | UML, comportamiento | `docs/diagramas/casos-de-uso.drawio` y sus PNG |
| C4 nivel 1 · Contexto | C4 | [Sección 1](#1--nivel-1--diagrama-de-contexto) de este archivo |
| C4 nivel 2 · Contenedores | C4 | [Sección 2](#2--nivel-2--diagrama-de-contenedores) de este archivo |
| C4 nivel 3 · Componentes | C4 | [Sección 3](#3--nivel-3--diagrama-de-componentes) de este archivo |
| Clases | UML, estructural | [diagrama-de-clases.md](diagrama-de-clases.md), sección 1 |
| Objetos | UML, estructural | [diagrama-de-clases.md](diagrama-de-clases.md), sección 2 |
| Paquetes | UML, estructural | [paquetes-y-componentes.md](paquetes-y-componentes.md), sección 1 |
| Componentes | UML, estructural | [paquetes-y-componentes.md](paquetes-y-componentes.md), sección 2 |
| Secuencia · Registrar el seguimiento diario | UML, comportamiento | [diagrama-de-secuencia.md](diagrama-de-secuencia.md) |
| Estados · Registro diario, Cita y Cuenta | UML, comportamiento | [estados-y-actividad.md](estados-y-actividad.md), secciones 1 a 3 |
| Actividad · Registrar el seguimiento diario | UML, comportamiento | [estados-y-actividad.md](estados-y-actividad.md), sección 4 |
| Modelo de datos | Entidad-relación | [Sección 8](#8--modelo-de-datos) de este archivo |
| Revisión de consistencia entre todos | Taller | [consistencia.md](consistencia.md) |

## Registros de decisión

| # | Decisión | Archivo |
|---|---|---|
| ADR-001 | Monolito modular con cuatro módulos | [adr-001-monolito-modular.md](adr-001-monolito-modular.md) |
| ADR-002 | Software como servicio con multi-tenancy todo compartido | [adr-002-multi-tenancy-todo-compartido.md](adr-002-multi-tenancy-todo-compartido.md) |
| ADR-003 | Tareas programadas en un proceso aparte | [adr-003-tareas-programadas.md](adr-003-tareas-programadas.md) |
| ADR-004 | Datos de los dispositivos por el servicio de cada fabricante, con un conector por fabricante | [adr-004-datos-de-dispositivos.md](adr-004-datos-de-dispositivos.md) |
| ADR-005 | El informe en PDF lo arma la app | [adr-005-informe-pdf-en-la-app.md](adr-005-informe-pdf-en-la-app.md) |

---

## 1 · Nivel 1 · Diagrama de contexto

El sistema completo visto desde afuera: quién lo usa y con quién habla. No aparece ninguna tecnología a propósito, porque es el diagrama que se le puede enseñar al paciente sin explicarle nada. Los tres usuarios son los de la visión del producto, y cada flecha dice lo que hace cada uno con el sistema. Los dispositivos quedan fuera de la frontera porque NutriGo! no los fabrica, solo recibe lo que miden. Los dos servicios de envío también quedan afuera: NutriGo! los usa para mandar los enlaces de contraseña por correo y los avisos al teléfono del RF-033, pero no son parte del sistema.

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#fff8f4','primaryBorderColor':'#ff5d00'}}}%%
flowchart TD
    pac([Paciente])
    nut([Nutriólogo])
    adm([Administrador])
    sis[Sistema NutriGo!]
    disp[Dispositivos compatibles]
    correo[Servicio de correo]
    avisos[Servicio de notificaciones del teléfono]

    pac -->|registra su día y consulta su progreso y su plan| sis
    nut -->|atiende citas y revisa a sus pacientes| sis
    adm -->|da de alta nutriólogos y desactiva cuentas| sis
    disp -->|envía peso, sueño y actividad física| sis
    sis -->|envía enlaces para crear contraseñas| correo
    sis -->|avisa al paciente la respuesta a su cita| avisos

    style sis fill:#ff5d00,color:#fff
```

---

## 2 · Nivel 2 · Diagrama de contenedores

Las piezas que se despliegan y se ejecutan por separado, con la tecnología de cada una. El subgraph dibuja la frontera del sistema. Los tres usuarios entran por la misma app móvil y la API decide qué puede hacer cada uno según su rol. Las tareas programadas son un proceso aparte porque hacen el trabajo que nadie pide en el momento: descargar las mediciones de los dispositivos, generar el plan de cada lunes y sustituir los registros de más de 30 días por el resumen mensual. La app nunca habla con los dispositivos: los datos llegan por el servicio de cada fabricante, así que el registro funciona aunque el paciente no tenga la báscula cerca. La app también arma el PDF del informe de avances (RF-026) con lo que le contesta la API, y lo guarda en el teléfono.

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#fff8f4','primaryBorderColor':'#ff5d00'}}}%%
flowchart TD
    pac([Paciente])
    nut([Nutriólogo])
    adm([Administrador])

    subgraph nutrigo[NutriGo!]
        app[App móvil<br/>React Native]
        api[API de NutriGo!<br/>Node.js + Express]
        tar[Tareas programadas<br/>Node.js + cron]
        db[(Base de datos<br/>PostgreSQL)]
    end

    fab[Servicios de los fabricantes<br/>reloj, banda y báscula]
    correo[Servicio de correo<br/>SMTP]
    avisos[Servicio de notificaciones<br/>push del teléfono]

    pac -->|usa| app
    nut -->|usa| app
    adm -->|usa| app
    app -->|HTTPS/JSON · registros, planes y citas| api
    api -->|lee y escribe| db
    api -->|OAuth · vincular| fab
    api -->|SMTP · enlaces| correo
    api -->|HTTPS · avisos de citas| avisos
    avisos -->|entrega el aviso| app
    tar -->|HTTPS · mediciones| fab
    tar -->|guarda mediciones, planes y resúmenes| db

    style api fill:#ff5d00,color:#fff
```

---

## 3 · Nivel 3 · Diagrama de componentes

Lo que hay dentro de la API, que es donde vive la lógica del sistema. Los componentes son los cuatro módulos de la Actividad 2, que también son los paquetes del diagrama de paquetes ([paquetes-y-componentes.md](paquetes-y-componentes.md)), más el Notificador, que es compartido y no tiene reglas de negocio.

Hay dos cosas que el dibujo deja ver. Ningún componente le habla directo a un fabricante: solo Seguimiento, que tiene adentro el conector con el servicio de cada uno. Es el patrón Adapter, y es lo que permite agregar un fabricante nuevo sin tocar el registro diario. La otra es que las flechas entre módulos bajan y no regresan: Cuentas contesta, pero no le pide nada a ningún otro módulo.

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#fff8f4','primaryBorderColor':'#ff5d00'}}}%%
flowchart LR
    app[App móvil]

    subgraph api[API de NutriGo!]
        cue[Cuentas<br/>registro, sesión, roles,<br/>autorizaciones y administración]
        seg[Seguimiento<br/>registro diario, dispositivos,<br/>progreso y resumen mensual]
        pla[Plan alimenticio<br/>preferencias y plan semanal]
        con[Consultas<br/>horarios y citas]
        noti[Notificador<br/>correo y avisos al teléfono]
    end

    db[(Base de datos)]
    fab[Servicios de los fabricantes]
    correo[Servicio de correo]
    avisos[Servicio de notificaciones]

    app --> cue
    app --> seg
    app --> pla
    app --> con
    seg -->|estatura y autorización del nutriólogo| cue
    pla -->|registros de la semana| seg
    pla -->|datos personales| cue
    con -->|paciente, nutriólogo y autorización| cue
    cue -->|enviarEnlace| noti
    con -->|avisa la respuesta a la cita| noti
    seg -->|HTTPS · mediciones, un conector por fabricante| fab
    noti --> correo
    noti --> avisos
    cue --> db
    seg --> db
    pla --> db
    con --> db

    style seg fill:#ff5d00,color:#fff
```

---

## 4 · Registrar el seguimiento diario

> **Movido a [estados-y-actividad.md](estados-y-actividad.md), sección 4.** En la Unidad 4 este flujo se volvió un diagrama de actividad con calles para el paciente y el sistema, y con las acciones en infinitivo.

---

## 5 · Registro del día con datos del dispositivo

> **Reemplazado por [diagrama-de-secuencia.md](diagrama-de-secuencia.md).** La versión anterior era una secuencia entre contenedores, con llamadas HTTP, y no se podía comparar con el diagrama de clases. La nueva cuenta el mismo escenario entre objetos, con barras de activación y un `alt`, y cada mensaje es una operación de la clase que lo recibe.

---

## 6 · Modelo de dominio

> **Reemplazado por [diagrama-de-clases.md](diagrama-de-clases.md).** Este fue el primer modelo de dominio de la Unidad 3. En la Unidad 4 las clases se volvieron a sacar de los requisitos con la técnica de los sustantivos (`docs/clases-candidatas.md`), y cambiaron tres cosas: Usuario con herencia pasó a ser Cuenta con su rol más una clase Paciente, la autorización dejó de ser un atributo de la cita y es una clase propia, y la estatura va en centímetros. Se conserva aquí solo como historial; el diagrama vigente es el del archivo nuevo.

Clases, atributos, métodos, visibilidad y multiplicidad: el signo menos es privado y el más es público. Paciente, Nutriólogo y Administrador heredan de Usuario porque comparten el inicio y el cierre de sesión, pero cada uno tiene funciones propias de su rol. Hay dos requisitos que se volvieron métodos: admite(alimento) en el perfil alimenticio es la regla de no incluir alergias, restricciones ni alimentos que no le gustan al paciente, y calcularIMC(pesoKg) vive en Paciente porque es quien tiene la estatura. La autorización para que el nutriólogo vea el progreso es un atributo de la cita, porque el paciente la da al solicitarla.

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#fff8f4','primaryBorderColor':'#ff5d00'}}}%%
classDiagram
    class Usuario {
        -id: string
        -nombre: string
        -correo: string
        -activo: bool
        +iniciarSesion() bool
        +cerrarSesion() void
    }
    class Paciente {
        -fechaNacimiento: Date
        -sexo: string
        -estaturaM: double
        +calcularIMC(pesoKg) double
    }
    class Nutriologo {
        +registrarHorario(horario) void
        +responder(cita, acepta) void
    }
    class Administrador {
        +darDeAlta(nutriologo) void
        +desactivar(usuario) void
    }
    class RegistroDiario {
        -fecha: Date
        -pesoKg: double
        -horasSueno: double
        -aguaLitros: double
        -actividadMin: int
        -estado: string
        +corregir(dato, valor) void
        +estaCompleto() bool
    }
    class Alimento {
        -tiempoComida: string
        -nombre: string
        -porciones: double
    }
    class PerfilAlimenticio {
        -objetivo: string
        -alergias: List~string~
        -restricciones: List~string~
        -noDeseados: List~string~
        +admite(alimento) bool
    }
    class PlanSemanal {
        -inicioSemana: Date
        -diasConRegistro: int
        +generar(perfil, registros) void
    }
    class ComidaDelPlan {
        -dia: string
        -tiempoComida: string
        -platillo: string
    }
    class Dispositivo {
        -tipo: string
        -fabricante: string
        -conectado: bool
        -ultimaSincronizacion: Date
        +sincronizar() void
        +desvincular() void
    }
    class HorarioDeAtencion {
        -diaSemana: string
        -horaInicio: string
        -horaFin: string
        -duracionMin: int
        +espacios() List~Date~
    }
    class Cita {
        -fechaHora: Date
        -motivo: string
        -estado: string
        -autorizaProgreso: bool
        +cancelar() void
    }
    class ResumenMensual {
        -mes: string
        -diasConRegistro: int
        +descargarPDF() File
    }

    Usuario <|-- Paciente
    Usuario <|-- Nutriologo
    Usuario <|-- Administrador
    Paciente "1" --> "*" RegistroDiario : registra
    RegistroDiario "1" --> "*" Alimento : incluye
    Paciente "1" --> "1" PerfilAlimenticio : tiene
    Paciente "1" --> "*" Dispositivo : vincula
    Paciente "1" --> "*" PlanSemanal : recibe
    PlanSemanal "1" --> "*" ComidaDelPlan : contiene
    Paciente "1" --> "*" ResumenMensual : conserva
    Paciente "1" --> "*" Cita : solicita
    Cita "*" --> "1" Nutriologo : atiende
    Nutriologo "1" --> "*" HorarioDeAtencion : registra
```

---

## 7 · Ciclo de vida de un registro diario

> **Movido a [estados-y-actividad.md](estados-y-actividad.md), sección 1.** La máquina ahora tiene guardas y acciones en sus transiciones, y sus estados son los valores de la enumeración `EstadoRegistro` del diagrama de clases. El estado EnResumenMensual se quitó: cuando el registro se suma al resumen deja de existir, así que es el final de su vida y no un estado más. En el mismo archivo están las máquinas de Cita y de Cuenta.

---

## 8 · Modelo de datos

Las tablas de la base de datos y cómo se relacionan, con la cardinalidad en los extremos de cada línea: || es uno y exactamente uno, o{ es cero o muchos, o| es cero o uno. Pacientes, nutriólogos y administradores están en la misma tabla USUARIO y se distinguen por el rol, igual que la clase Cuenta. CITA tiene dos relaciones con USUARIO porque guarda quién la solicita y quién la atiende, y AUTORIZACION también, porque guarda qué paciente le da permiso a qué nutriólogo. En REGISTRO_DIARIO la pareja usuario y fecha lleva restricción UNIQUE, para que exista un solo registro por día, y los datos que faltan se guardan como vacíos y no como cero: así la base de datos misma impide inventar un progreso que no ocurrió.

La clase Alimento se guarda en dos tablas, ALIMENTO_REGISTRADO y COMIDA_PLAN, una para lo que se comió y otra para lo que se planeó. Es la duda que quedó abierta en el diagrama de clases, resuelta del lado de la base de datos.

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#fff8f4','primaryBorderColor':'#ff5d00'}}}%%
erDiagram
    USUARIO ||--o{ REGISTRO_DIARIO : registra
    REGISTRO_DIARIO ||--o{ ALIMENTO_REGISTRADO : incluye
    USUARIO ||--o| PERFIL_ALIMENTICIO : tiene
    USUARIO ||--o{ DISPOSITIVO : vincula
    USUARIO ||--o{ PLAN_SEMANAL : recibe
    PLAN_SEMANAL ||--o{ COMIDA_PLAN : contiene
    USUARIO ||--o{ HORARIO_ATENCION : registra
    HORARIO_ATENCION ||--o{ CITA : ocupa
    USUARIO ||--o{ CITA : solicita
    USUARIO ||--o{ CITA : atiende
    USUARIO ||--o{ AUTORIZACION : otorga
    USUARIO ||--o{ AUTORIZACION : recibe
    USUARIO ||--o{ RESUMEN_MENSUAL : conserva

    USUARIO {
        int id
        string nombre
        string correo
        string hash
        string rol
        string estado
        date fecha_nacimiento
        string sexo
        int estatura_cm
    }
    REGISTRO_DIARIO {
        int id
        int usuario_id
        date fecha
        decimal peso_kg
        string origen_peso
        decimal horas_sueno
        decimal agua_litros
        int actividad_min
        decimal grasa_pct
        decimal musculo_pct
        string estado
    }
    ALIMENTO_REGISTRADO {
        int id
        string tiempo_comida
        string nombre
    }
    PERFIL_ALIMENTICIO {
        int id
        string objetivo
        string alergias
        string restricciones
        string no_le_gustan
    }
    DISPOSITIVO {
        int id
        string tipo
        string fabricante
        string token_acceso
        datetime ultima_sincronizacion
        bool conectado
    }
    PLAN_SEMANAL {
        int id
        date inicio_semana
        bool vigente
    }
    COMIDA_PLAN {
        int id
        date fecha
        string tiempo_comida
        string nombre
    }
    HORARIO_ATENCION {
        int id
        string dia_semana
        string hora_inicio
        string hora_fin
        int duracion_min
    }
    CITA {
        int id
        int paciente_id
        int nutriologo_id
        int horario_id
        datetime fecha_hora
        string motivo
        string estado
    }
    AUTORIZACION {
        int id
        int paciente_id
        int nutriologo_id
        date desde
    }
    RESUMEN_MENSUAL {
        int id
        string mes
        decimal peso_promedio
        decimal imc_promedio
        int dias_con_registro
    }
```

---
## Notas de entrega

- Todos los bloques usan la sintaxis estándar de Mermaid; el tema del curso va en la línea `%%{init: ...}%%` de cada bloque y no se evalúa.
- El nivel 1 no menciona tecnología a propósito: es el diagrama que se le enseña al paciente, no al programador.
- Los niveles 1 a 3 son C4 de la Unidad 3. Los diagramas UML de la Unidad 4 están en los archivos de la tabla del inicio.
- La versión 1.1 (17/11/2026) agrega el servicio de notificaciones del RF-033 a los niveles 1 a 3, agrupa el nivel 3 en los módulos de la Actividad 2 y alinea el modelo de datos con el diagrama de clases. El porqué de cada cambio está en [consistencia.md](consistencia.md).
