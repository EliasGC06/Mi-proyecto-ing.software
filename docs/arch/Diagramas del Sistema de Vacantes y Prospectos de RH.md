# Diagramas del Sistema de Vacantes y Prospectos de RH

Sistema de software instalado (un solo cliente) que centraliza vacantes, prospectos, documentos y postulaciones para el área de Recursos Humanos, con visibilidad controlada por rol para Finanzas/Contaduría y Corporativo.

## 1. Diagrama de contexto

```mermaid
flowchart TB
    rh[" Personal de RH<br/>Registra, filtra y da seguimiento<br/>a vacantes y prospectos"]
    fin[" Finanzas / Contaduría<br/>Consulta información de<br/>contratación para su área"]
    corp[" Corporativo<br/>Se mantiene informado del<br/>estado de las vacantes"]

    sistema["<b>Sistema de Vacantes y Prospectos</b><br/>Centraliza vacantes, prospectos, documentos<br/>y postulaciones. Muestra a cada área<br/>solo lo que le corresponde"]

    correo[" Servicio de correo<br/>de la empresa<br/>(sistema externo)"]

    rh -->|"Registra y consulta"| sistema
    fin -->|"Consulta (solo lectura)"| sistema
    corp -->|"Consulta (solo lectura)"| sistema
    sistema -->|"Envía avisos de cambios"| correo
    correo -.->|"Entrega avisos a"| rh
```

---

## 2. Diagrama de contenedores

```mermaid
flowchart TB
    usuario[" Usuario<br/>(RH, Finanzas, Corporativo)"]

    subgraph servidor["Servidor de la empresa (instalación única del cliente)"]
        spa["<b>Aplicación web</b><br/>[React, SPA]<br/>Pantallas de vacantes, prospectos<br/>y documentos según el rol"]
        api["<b>Aplicación de negocio</b><br/>[Node.js + Express, monolito modular]<br/>Reglas de negocio, permisos por rol<br/>y bitácora de cambios"]
        bd[("<b>Base de datos</b><br/>[PostgreSQL]<br/>Vacantes, prospectos, postulaciones,<br/>usuarios y bitácora")]
        archivos[("<b>Almacén de documentos</b><br/>[Sistema de archivos del servidor]<br/>CV, identificaciones y comprobantes")]
    end

    correo[" Servicio de correo<br/>de la empresa [SMTP]"]

    usuario -->|"Usa desde el navegador [HTTPS]"| spa
    spa -->|"Solicita datos y acciones [JSON sobre HTTPS / API REST]"| api
    api -->|"Lee y escribe registros [SQL, conexión TCP]"| bd
    api -->|"Guarda y lee archivos [acceso a disco local]"| archivos
    api -->|"Envía avisos de cambios [SMTP]"| correo
```

---

## 3. Diagrama de componentes

Interior de la Aplicación de negocio, el contenedor donde viven las reglas de negocio.

```mermaid
flowchart TB
    spa["<b>Aplicación web</b><br/>[React, SPA]"]
    bd[("Base de datos<br/>[PostgreSQL]")]
    archivos[("Almacén de documentos<br/>[Sistema de archivos]")]
    correo["Servicio de correo<br/>[SMTP]"]

    subgraph api["Aplicación de negocio [Node.js + Express, monolito modular]"]
        direction TB
        ctrl["<b>Capa de API</b><br/>Recibe las peticiones REST<br/>y valida los datos de entrada"]
        auth["<b>Módulo Usuarios y Roles</b><br/>Autentica y determina el rol<br/>(RH, Finanzas, Corporativo)"]
        vis["<b>Módulo Visibilidad por rol</b><br/>Decide qué campos y registros ve<br/>cada rol (Strategy por rol)"]
        pros["<b>Módulo Prospectos</b><br/>Alta, clasificación, verificación<br/>y filtrado de prospectos"]
        vac["<b>Módulo Vacantes</b><br/>Alta, urgencia, estado<br/>y filtrado de vacantes"]
        post["<b>Módulo Postulaciones</b><br/>Relaciona prospectos con vacantes<br/>y controla su avance"]
        docs["<b>Módulo Documentos</b><br/>Carga, verifica y recupera<br/>los documentos del prospecto"]
        bit["<b>Módulo Bitácora</b><br/>Registra quién cambió qué y cuándo<br/>(RNF-06)"]
        notif["<b>Módulo Notificaciones</b><br/>Avisa de cambios relevantes<br/>(Observer + Adapter de correo)"]
        repo["<b>Capa de Persistencia</b><br/>Acceso a base de datos<br/>y al almacén de documentos"]
    end

    spa -->|"JSON / HTTPS"| ctrl
    ctrl --> auth
    ctrl --> pros
    ctrl --> vac
    ctrl --> post
    ctrl --> docs
    pros --> vis
    vac --> vis
    post --> vis
    vis --> auth
    post --> pros
    post --> vac
    pros --> docs
    pros -->|"Publica cambios"| bit
    vac -->|"Publica cambios"| bit
    post -->|"Publica cambios"| bit
    pros -->|"Publica eventos"| notif
    vac -->|"Publica eventos"| notif
    post -->|"Publica eventos"| notif
    pros --> repo
    vac --> repo
    post --> repo
    docs --> repo
    bit --> repo
    auth --> repo
    repo -->|"SQL"| bd
    repo -->|"Acceso a disco"| archivos
    notif -->|"SMTP"| correo
```

---

## 4. Diagrama de clases

```mermaid
classDiagram
class Empresa {
-nombre: string
-vacantes: List~Vacante~
+registrarVacante(vacante) void
+consultarVacantesAbiertas() List~Vacante~
}

class Vacante {
-puesto: string
-horario: string
-localidad: string
-urgencia: string
-estado: string
+cambiarEstado(estado) void
+estaAbierta() boolean
}

class Prospecto {
-nombre: string
-edad: int
-telefono: string
-correo: string
-escolaridad: string
-comentarios: string
+verificarDocumentos() boolean
+agregarDocumento(documento) void
}

class Documento {
-tipo: string
-archivo: string
-verificado: boolean
+marcarVerificado() void
}

class Postulacion {
-fecha: Date
-estado: string
+avanzar(nuevoEstado) void
}

class Usuario {
-nombre: string
-rol: string
-politica: PoliticaVisibilidad
+consultarProspecto(prospecto) Map
}

class PoliticaVisibilidad {
<<interface>>
+camposVisibles(prospecto) Map
}

class VisibilidadRH {
+camposVisibles(prospecto) Map
}

class VisibilidadFinanzas {
+camposVisibles(prospecto) Map
}

class VisibilidadCorporativo {
+camposVisibles(prospecto) Map
}

class RegistroBitacora {
-usuario: string
-accion: string
-fecha: Date
}

class Observador {
<<interface>>
+actualizar(evento) void
}

class Bitacora {
-registros: List~RegistroBitacora~
+actualizar(evento) void
}

class Notificador {
-servicio: ServicioCorreo
+actualizar(evento) void
}

class ServicioCorreo {
<<interface>>
+enviar(destino, mensaje) boolean
}

class AdaptadorSMTP {
+enviar(destino, mensaje) boolean
}

Empresa "1" --> "*" Vacante : publica
Vacante "1" --> "*" Postulacion : recibe
Prospecto "1" --> "*" Postulacion : realiza
Prospecto "1" --> "*" Documento : entrega
Usuario "1" --> "1" PoliticaVisibilidad : usa
PoliticaVisibilidad <|.. VisibilidadRH
PoliticaVisibilidad <|.. VisibilidadFinanzas
PoliticaVisibilidad <|.. VisibilidadCorporativo
Postulacion ..> Observador : notifica cambios
Observador <|.. Bitacora
Observador <|.. Notificador
Bitacora "1" --> "*" RegistroBitacora : guarda
Notificador "1" --> "1" ServicioCorreo : envía con
ServicioCorreo <|.. AdaptadorSMTP
```

---

## 5. Diagrama de estados: ciclo de vida de una postulación

```mermaid
stateDiagram-v2
[*] --> Registrada
Registrada --> EnRevision: RH verifica los documentos
Registrada --> Retirada: el prospecto se retira
EnRevision --> Entrevista: RH aprueba el perfil
EnRevision --> Rechazada: no cumple el perfil
EnRevision --> Retirada: el prospecto se retira
Entrevista --> Contratada: se acepta al prospecto
Entrevista --> Rechazada: no pasa la entrevista
Entrevista --> Retirada: el prospecto se retira
Contratada --> [*]
Rechazada --> [*]
Retirada --> [*]
```

---

## 6. Diagrama de flujo: validación al avanzar una postulación

```mermaid
flowchart TD
ini([Inicio])
vac{"La vacante esta abierta?"}
doc{"Los documentos estan verificados?"}
rol{"El usuario es de RH?"}
apl[Avanzar la postulacion y registrar en bitacora]
err1[Vacante cerrada]
err2[Documentos incompletos]
err3[Sin permiso para modificar]
fin([Fin])
ini --> rol
rol -->|No| err3
rol -->|Si| vac
vac -->|No| err1
vac -->|Si| doc
doc -->|No| err2
doc -->|Si| apl
err1 --> fin
err2 --> fin
err3 --> fin
apl --> fin
style apl fill:#ff5d00,color:#fff
```

---

## 7. Diagrama de secuencia: avanzar una postulación

```mermaid
sequenceDiagram
actor RH as Personal de RH
participant Web as App web
participant Post as Postulaciones
participant Bit as Bitacora
participant Not as Notificaciones
participant BD as Base de datos
participant Cor as Servicio de correo
RH->>Web: Cambia la postulacion a Entrevista
Web->>Post: avanzar(postulacion, Entrevista)
Post->>BD: Guarda el nuevo estado
BD-->>Post: Listo
Post->>Bit: Publica el cambio
Bit->>BD: Guarda el registro de bitacora
BD-->>Bit: Listo
Post->>Not: Publica el evento
Not->>Cor: enviar(destino, mensaje)
Cor-->>Not: Aviso enviado
Post-->>Web: Postulacion actualizada
Web-->>RH: Muestra el nuevo estado
```

---

## 8. Diagrama entidad-relación

```mermaid
erDiagram
EMPRESA ||--o{ VACANTE : publica
VACANTE ||--o{ POSTULACION : recibe
PROSPECTO ||--o{ POSTULACION : realiza
PROSPECTO ||--o{ DOCUMENTO : entrega
USUARIO ||--o{ BITACORA : genera
POSTULACION ||--o{ BITACORA : registra
EMPRESA {
int id
string nombre
}
VACANTE {
int id
string puesto
string horario
string localidad
string urgencia
string estado
}
PROSPECTO {
int id
string nombre
int edad
string telefono
string correo
string escolaridad
}
POSTULACION {
int id
date fecha
string estado
}
DOCUMENTO {
int id
string tipo
string archivo
boolean verificado
}
USUARIO {
int id
string nombre
string rol
}
BITACORA {
int id
string accion
date fecha
}
