# Diagramas de arquitectura · NutriGo!

| Campo | Valor |
|---|---|
| **Sistema** | NutriGo!  |
| **Autor** | Diego Valdovinos Rodríguez |
| **Versión** | 1.0 |
| **Fecha** | 28/10/2026 |
| **Fuentes** | `docs/vision-del-producto.md`, `docs/especificacion-requisitos.md` |

Los diagramas van como texto de Mermaid, igual que el código: se comparan entre versiones y se revisan en el mismo diff. Para verlos, pega cada bloque en [mermaid.live](https://mermaid.live).

| # | Diagrama | Tipo |
|---|---|---|
| 1 | [Contexto (C4, nivel 1)](#1--nivel-1--diagrama-de-contexto) | `flowchart` |
| 2 | [Contenedores (C4, nivel 2)](#2--nivel-2--diagrama-de-contenedores) | `flowchart` |
| 3 | [Componentes (C4, nivel 3)](#3--nivel-3--diagrama-de-componentes) | `flowchart` |
| 4 | [Registrar el seguimiento diario](#4--registrar-el-seguimiento-diario) | `flowchart` con decisiones |
| 5 | [Registro del día con datos del dispositivo](#5--registro-del-día-con-datos-del-dispositivo) | `sequenceDiagram` |
| 6 | [Modelo de dominio](#6--modelo-de-dominio) | `classDiagram` |
| 7 | [Ciclo de vida de un registro diario](#7--ciclo-de-vida-de-un-registro-diario) | `stateDiagram-v2` |
| 8 | [Modelo de datos](#8--modelo-de-datos) | `erDiagram` |

---

## 1 · Nivel 1 · Diagrama de contexto

El sistema completo visto desde afuera: quién lo usa y con quién habla. No aparece ninguna tecnología a propósito, porque es el diagrama que se le puede enseñar al paciente sin explicarle nada. Los tres usuarios son los de la visión del producto, y cada flecha dice lo que hace cada uno con el sistema. Los dispositivos quedan fuera de la frontera porque NutriGo! no los fabrica, solo recibe lo que miden. El servicio de correo también queda afuera: NutriGo! lo usa para enviar los enlaces de contraseña, pero no es parte del sistema.

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#fff8f4','primaryBorderColor':'#ff5d00'}}}%%
flowchart TD
    pac([Paciente])
    nut([Nutriólogo])
    adm([Administrador])
    sis[Sistema NutriGo!]
    disp[Dispositivos compatibles]
    correo[Servicio de correo]

    pac -->|registra su día y consulta su progreso y su plan| sis
    nut -->|atiende citas y revisa a sus pacientes| sis
    adm -->|da de alta nutriólogos y desactiva cuentas| sis
    disp -->|envía peso, sueño y actividad física| sis
    sis -->|envía enlaces para crear contraseñas| correo

    style sis fill:#ff5d00,color:#fff
```

---

## 2 · Nivel 2 · Diagrama de contenedores

Las piezas que se despliegan y se ejecutan por separado, con la tecnología de cada una. El subgraph dibuja la frontera del sistema. Los tres usuarios entran por la misma app móvil y la API decide qué puede hacer cada uno según su rol. Las tareas programadas son un proceso aparte porque hacen el trabajo que nadie pide en el momento: descargar las mediciones de los dispositivos, generar el plan de cada lunes y sustituir los registros de más de 30 días por el resumen mensual. La app nunca habla con los dispositivos: los datos llegan por el servicio de cada fabricante, así que el registro funciona aunque el paciente no tenga la báscula cerca.

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

    pac -->|usa| app
    nut -->|usa| app
    adm -->|usa| app
    app -->|HTTPS/JSON · registros, planes y citas| api
    api -->|lee y escribe| db
    api -->|OAuth · vincular| fab
    api -->|SMTP · enlaces| correo
    tar -->|HTTPS · mediciones| fab
    tar -->|guarda mediciones, planes y resúmenes| db

    style api fill:#ff5d00,color:#fff
```

---

## 3 · Nivel 3 · Diagrama de componentes

Lo que hay dentro de la API, que es donde vive la lógica del sistema. Hay dos cosas que el dibujo deja ver. Ningún componente le habla directo a un fabricante: todos pasan por el Conector de dispositivos, que es el único que conoce cómo responde cada servicio. Es el patrón Adapter, y es lo que permite agregar un fabricante nuevo sin tocar el registro diario. La otra es que el Plan alimenticio y los Informes le preguntan sus datos al Registro diario y al Progreso en lugar de leer sus tablas, así cada regla vive en un solo lugar.

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#fff8f4','primaryBorderColor':'#ff5d00'}}}%%
flowchart LR
    app[App móvil]

    subgraph api[API de NutriGo!]
        ses[Cuentas y sesión<br/>registro, acceso y roles]
        reg[Registro diario<br/>datos del día y correcciones]
        dis[Dispositivos<br/>vincular y desvincular]
        pro[Progreso<br/>IMC e indicadores]
        pla[Plan alimenticio<br/>preferencias y plan semanal]
        cit[Citas<br/>horarios, solicitudes y autorización]
        inf[Informes<br/>PDF y resumen mensual]
        adm[Administración<br/>alta y desactivación]
        con[Conector de dispositivos<br/>interfaz por fabricante]
        noti[Notificador<br/>interfaz de envío]
    end

    db[(Base de datos)]
    fab[Servicios de los fabricantes]
    correo[Servicio de correo]

    app --> ses
    app --> reg
    app --> dis
    app --> pro
    app --> pla
    app --> cit
    app --> inf
    app --> adm
    reg -->|pide los datos del día| con
    dis -->|vincula y desvincula| con
    con -->|HTTPS · mediciones| fab
    pro -->|lee los registros| reg
    pro -->|verifica la autorización del paciente| cit
    pla -->|pregunta los registros de la semana| reg
    inf -->|pide los indicadores| pro
    inf -->|pide el plan del periodo| pla
    adm -->|crea y desactiva cuentas| ses
    ses -->|enviarEnlace| noti
    noti --> correo
    reg --> db
    pla --> db
    cit --> db
    ses --> db

    style con fill:#ff5d00,color:#fff
```

---

## 4 · Registrar el seguimiento diario

El caso de uso de registrar el seguimiento diario (CU-07), que es donde el flujo se puede complicar. La caja naranja es el único camino que deja el día completo. Los dos supuestos que resultaron falsos en la entrevista están en el dibujo: los días anteriores sin registro solo generan un aviso y nunca detienen el flujo, y lo que no envía el dispositivo se captura a mano. Si el paciente no termina, el día se guarda como incompleto y lo que falta no se rellena con ningún valor.

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#fff8f4','primaryBorderColor':'#ff5d00'}}}%%
flowchart TD
    ini([Inicio])
    abrir[El paciente abre el registro del día]
    dias{Hay días anteriores sin registro?}
    aviso[Avisar y dejar continuar]
    disp{Llegaron todos los datos del dispositivo?}
    manual[Capturar a mano lo que no llegó]
    cap[Capturar agua y alimentos]
    rango{Valores dentro de rango?}
    corr[Señalar el campo y el rango permitido]
    todo{Capturó todos los datos?}
    comp[Guardar el día como Completo]
    inc[Guardar el día como Incompleto]
    fin([Fin])

    ini --> abrir
    abrir --> dias
    dias -->|Sí| aviso
    dias -->|No| disp
    aviso --> disp
    disp -->|No| manual
    disp -->|Sí| cap
    manual --> cap
    cap --> rango
    rango -->|No| corr
    corr --> rango
    rango -->|Sí| todo
    todo -->|Sí| comp
    todo -->|No| inc
    comp --> fin
    inc --> fin

    style comp fill:#ff5d00,color:#fff
```

---

## 5 · Registro del día con datos del dispositivo

El flujo principal del producto en el tiempo, cruzando los contenedores. Las flechas sólidas son llamadas y las punteadas son respuestas. Lo importante es que el peso ya está guardado antes de que el paciente abra la app: las tareas programadas lo descargaron del servicio del fabricante en la mañana. Por eso el paciente solo captura lo que ningún dispositivo mide, que fue justo lo que pidió en la entrevista. La API revisa los rangos aunque el dato venga de la báscula, porque un dispositivo también puede enviar un valor equivocado.

```mermaid
sequenceDiagram
    actor Pac as Paciente
    participant App as App móvil
    participant Api as API de NutriGo!
    participant Tar as Tareas programadas
    participant Fab as Servicio del fabricante
    participant BD as Base de datos

    Tar->>Fab: Pide las mediciones nuevas
    Fab-->>Tar: Peso 72.4 kg de la báscula a las 7:02
    Tar->>BD: Guarda el peso con origen dispositivo
    Pac->>App: Abre el registro de hoy
    App->>Api: GET /registros/hoy
    Api->>BD: Busca el registro del día
    BD-->>Api: Peso, sueño y actividad, sin agua ni alimentos
    Api-->>App: Datos del dispositivo y lo que falta
    App-->>Pac: Muestra qué datos faltan
    Pac->>App: Captura 1.5 litros de agua y la avena del desayuno
    App->>Api: PUT /registros/hoy
    Api->>Api: Verifica que cada valor esté en rango
    Api->>BD: Guarda el registro y recalcula el IMC
    BD-->>Api: Listo
    Api-->>App: Día completo con IMC de 23.6
    App-->>Pac: Muestra el día como Completo
```

---

## 6 · Modelo de dominio

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

Por qué estados pasa el registro de un día y qué evento lo mueve; los corchetes [*] marcan el inicio y el fin. SinRegistro es un estado válido y puede quedarse así para siempre: el sistema no lo rellena con ningún valor, que es la regla que salió de la entrevista. La corrección de un dato no cambia el estado del día, por eso es una flecha que regresa al mismo estado. Y sin importar cómo haya quedado el día, a los 30 días todos terminan en el resumen mensual.

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#fff8f4','primaryBorderColor':'#ff5d00'}}}%%
stateDiagram-v2
    [*] --> SinRegistro
    SinRegistro --> Incompleto: llega una parte de los datos
    SinRegistro --> Completo: registra todos los datos
    Incompleto --> Completo: completa lo que falta
    Completo --> Completo: corrige un dato
    Incompleto --> Incompleto: corrige un dato
    SinRegistro --> EnResumenMensual: pasan 30 días
    Incompleto --> EnResumenMensual: pasan 30 días
    Completo --> EnResumenMensual: pasan 30 días
    EnResumenMensual --> [*]

    classDef destacado fill:#ff5d00,color:#fff
    class Completo destacado
```

---

## 8 · Modelo de datos

Las tablas de la base de datos y cómo se relacionan, con la cardinalidad en los extremos de cada línea: || es uno y exactamente uno, o{ es cero o muchos, o| es cero o uno. Pacientes, nutriólogos y administradores están en la misma tabla USUARIO y se distinguen por el rol. CITA tiene dos relaciones con USUARIO porque guarda quién la solicita y quién la atiende. En REGISTRO_DIARIO la pareja usuario y fecha lleva restricción UNIQUE, para que exista un solo registro por día, y los datos que faltan se guardan como vacíos y no como cero: así la base de datos misma impide inventar un progreso que no ocurrió.

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
    USUARIO ||--o{ CITA : solicita
    USUARIO ||--o{ CITA : atiende
    USUARIO ||--o{ RESUMEN_MENSUAL : conserva

    USUARIO {
        int id
        string nombre
        string correo
        string hash
        string rol
        bool activo
        date fecha_nacimiento
        string sexo
        decimal estatura_m
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
        decimal porciones
    }
    PERFIL_ALIMENTICIO {
        int id
        string objetivo
        string alergias
        string restricciones
        string no_deseados
    }
    DISPOSITIVO {
        int id
        string tipo
        string fabricante
        string token_acceso
        datetime ultima_sincronizacion
        bool activo
    }
    PLAN_SEMANAL {
        int id
        date inicio_semana
        int dias_con_registro
    }
    COMIDA_PLAN {
        int id
        string dia
        string tiempo_comida
        string platillo
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
        datetime fecha_hora
        string motivo
        string estado
        bool autoriza_progreso
    }
    RESUMEN_MENSUAL {
        int id
        string mes
        decimal peso_promedio
        int dias_con_registro
        string archivo_pdf
    }
```

---
## Notas de entrega

- Los ocho bloques usan la sintaxis estándar de Mermaid; el tema del curso va en la línea `%%{init: ...}%%` de cada bloque y no se evalúa.
- El nivel 1 no menciona tecnología a propósito: es el diagrama que se le enseña al paciente, no al programador.
- Los niveles 2 y 3 son C4; los diagramas 4 a 8 corresponden al diseño de la Unidad 3 y son un adelanto de UML de la Unidad 4.
