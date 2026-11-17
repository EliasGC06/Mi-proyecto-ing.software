# Diagramas del Sistema de Vacantes y Prospectos de RH

Sistema de software instalado (un solo cliente) que centraliza vacantes, prospectos, documentos y postulaciones para el área de Recursos Humanos, con visibilidad controlada por rol para Finanzas/Contaduría y Corporativo.

## 1. Diagrama de contexto
 
```mermaid
flowchart TB
    rh["Personal de RH<br/>Registra y da seguimiento<br/>a vacantes y prospectos"]
    fin["Finanzas<br/>Consulta la información<br/>de contratación"]
    corp["Corporativo<br/>Consulta el estado<br/>de las vacantes"]
 
    sistema["Sistema de Vacantes y Prospectos<br/>Centraliza vacantes, prospectos,<br/>documentos y postulaciones"]
 
    correo["Servicio de correo<br/>(sistema externo)"]
 
    rh -->|"Registra y consulta"| sistema
    fin -->|"Consulta"| sistema
    corp -->|"Consulta"| sistema
    sistema -->|"Envía avisos de cambios"| correo
```
 
---
 
## 2. Diagrama de contenedores
 
```mermaid
flowchart TB
    usuario["Usuario<br/>(RH, Finanzas, Corporativo)"]
 
    subgraph servidor["Servidor de la empresa"]
        spa["Aplicación web<br/>[React]<br/>Pantallas según el rol"]
        api["Aplicación de negocio<br/>[Node.js + Express]<br/>Reglas de negocio y bitácora"]
        bd[("Base de datos<br/>[PostgreSQL]")]
        archivos[("Almacén de documentos<br/>[Archivos del servidor]")]
    end
 
    correo["Servicio de correo<br/>[SMTP]"]
 
    usuario -->|"Usa el navegador [HTTPS]"| spa
    spa -->|"Pide datos y acciones [JSON / HTTPS]"| api
    api -->|"Lee y guarda registros [SQL]"| bd
    api -->|"Lee y guarda archivos [acceso a disco]"| archivos
    api -->|"Envía avisos de cambios [SMTP]"| correo
```
 
---
 
## 3. Diagrama de componentes

```mermaid
flowchart TB
    spa["Aplicación web<br/>[React]"]
    bd[("Base de datos<br/>[PostgreSQL]")]
    archivos[("Almacén de documentos<br/>[Archivos del servidor]")]
    correo["Servicio de correo<br/>[SMTP]"]

    subgraph api["Aplicación de negocio [Node.js + Express]"]
        direction TB
        ctrl["API<br/>Recibe las peticiones"]
        usr["Usuarios y Roles<br/>Identifica el rol de quien consulta"]
        vis["Visibilidad por rol<br/>Define qué campos ve cada rol (Strategy)"]
        pros["Prospectos"]
        vac["Vacantes"]
        post["Postulaciones"]
        docs["Documentos"]
        ev["Eventos internos<br/>Avisan cada cambio (Observer)"]
        bit["Bitácora<br/>Registra usuario, fecha y hora"]
        notif["Notificaciones<br/>Prepara los avisos"]
        mail["Adaptador de correo<br/>Aísla al proveedor (Adapter)"]
        repo["Persistencia<br/>Único acceso a datos y archivos"]
    end

    spa -->|"JSON / HTTPS"| ctrl
    ctrl --> usr
    ctrl --> pros
    ctrl --> vac
    ctrl --> post
    ctrl --> docs
    usr --> vis

    pros -.->|"publica: prospecto archivado"| ev
    vac -.->|"publica"| ev
    post -.->|"publica"| ev
    ev -.->|"reacciona"| bit
    ev -.->|"reacciona"| notif
    notif --> mail

    pros --> repo
    vac --> repo
    post --> repo
    docs --> repo
    bit --> repo
    usr --> repo

    repo -->|"SQL"| bd
    repo -->|"Acceso a disco"| archivos
    mail -->|"SMTP"| correo
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
+asignar(prospecto) Postulacion
}
 
class Prospecto {
-nombre: string
-edad: int
-telefono: string
-correo: string
-escolaridad: string
-comentarios: string
-estado: string
-motivoArchivo: string
-clasificacionArchivo: ClasificacionArchivo
-fechaArchivo: Date
+verificarDocumentos() boolean
+agregarDocumento(documento) void
+estaActivo() boolean
+cambiarEstado(estado) void
+archivar(motivo, clasificacion) void
+reactivar() void
+esElegible() boolean
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
+Postulacion(prospecto, vacante)
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
 
class ClasificacionArchivo {
<<enumeration>>
Recontratable
DescartadoDefinitivo
}

class Evento {
-tipo: string
-usuario: string
-fecha: Date
-detalle: string
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
Prospecto "1" --> "0..1" ClasificacionArchivo : clasificado como
Prospecto ..> Evento : crea al archivar
Prospecto ..> Observador : notifica el archivado
Postulacion ..> Evento : crea
Postulacion ..> Observador : notifica cambios
Observador ..> Evento : recibe
Observador <|.. Bitacora
Observador <|.. Notificador
Bitacora "1" --> "*" RegistroBitacora : guarda
Notificador "1" --> "1" ServicioCorreo : envía con
ServicioCorreo <|.. AdaptadorSMTP
```
 
---
 
## 5. Diagrama de secuencia: asignar prospecto a vacante (CU-03)

```mermaid
sequenceDiagram
actor RH as Personal de RH
participant Web as App web
participant API as API
participant Pro as Prospecto
participant Vac as Vacante
participant Bit as Bitacora
participant Not as Notificador
participant Smtp as AdaptadorSMTP
RH->>Web: Elige vacante y prospecto y confirma
Web->>API: asignar(idVacante, idProspecto)
API->>Pro: estaActivo()
Pro-->>API: resultado
API->>Pro: esElegible()
Pro-->>API: resultado
API->>Vac: estaAbierta()
Vac-->>API: resultado
alt prospecto activo o vacante no disponible (A1, A2)
API-->>Web: rechaza la asignacion y explica el motivo
Web-->>RH: Muestra el aviso
else prospecto Descartado definitivo (A3, propuesta SC-001)
API-->>Web: bloquea, con el motivo y la fecha del descarte
Web-->>RH: Muestra la alerta de reingreso no permitido
else todo correcto
Note over API,Not: Una sola transaccion (ADR-001)
opt prospecto archivado como Recontratable (A3)
API->>Pro: reactivar()
end
API->>Vac: asignar(prospecto)
create participant Pos as Postulacion
Vac->>Pos: Postulacion(prospecto, vacante)
Vac-->>API: postulacion
API->>Pro: cambiarEstado("en proceso")
API->>Pos: avanzar("en proceso")
Pos->>Bit: actualizar(evento)
Pos->>Not: actualizar(evento)
Not->>Smtp: enviar(destino, mensaje)
Pos-->>API: listo
API-->>Web: asignacion registrada
Web-->>RH: Muestra la vacante con el prospecto asignado
end
```

---
 
## 6. Diagrama de estados: prospecto

```mermaid
stateDiagram-v2
state "En proceso" as EnProceso
[*] --> Prospecto: RH registra al prospecto (CU-01)
Prospecto --> EnProceso: se asigna a una vacante (CU-03)
EnProceso --> Contratado: se contrata (CU-09)
Prospecto --> Archivado: RH archiva con motivo y clasificación (CU-08)
EnProceso --> Archivado: RH archiva con motivo y clasificación y libera la vacante (CU-08)
Archivado --> Prospecto: [clasificación ≠ DescartadoDefinitivo] vuelve a aplicar y RH lo reactiva (CU-01 A3, CU-03 A3)
Contratado --> [*]
note right of Archivado
Si la clasificación es DescartadoDefinitivo no hay transición de salida:
el expediente se conserva (RNF-03) pero no se reactiva (propuesta SC-001)
end note
```

## 7. Diagrama de flujo
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
