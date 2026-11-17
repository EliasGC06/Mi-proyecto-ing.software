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
        usr["Usuarios y Roles<br/>Define qué ve cada rol"]
        pros["Prospectos"]
        vac["Vacantes"]
        post["Postulaciones"]
        docs["Documentos"]
        bit["Bitácora<br/>Registra los cambios"]
        notif["Notificaciones<br/>Avisa por correo"]
        repo["Persistencia<br/>Guarda y lee datos"]
    end
 
    spa -->|"JSON / HTTPS"| ctrl
    ctrl --> usr
    ctrl --> pros
    ctrl --> vac
    ctrl --> post
    ctrl --> docs
    post --> bit
    post --> notif
    pros --> repo
    vac --> repo
    post --> repo
    docs --> repo
    bit --> repo
    usr --> repo
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
+verificarDocumentos() boolean
+agregarDocumento(documento) void
+estaActivo() boolean
+cambiarEstado(estado) void
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
participant Pos as Postulacion
participant Bit as Bitacora
participant Not as Notificador
participant Smtp as AdaptadorSMTP
RH->>Web: Elige vacante y prospecto y confirma
Web->>API: asignar(idVacante, idProspecto)
API->>Pro: estaActivo()
Pro-->>API: resultado
API->>Vac: estaAbierta()
Vac-->>API: resultado
alt prospecto activo o vacante no disponible (A1, A2)
API-->>Web: rechaza la asignacion y explica el motivo
Web-->>RH: Muestra el aviso
else todo correcto
Note over API,Not: Una sola transaccion (ADR-001)
API->>Vac: asignar(prospecto)
Vac->>Pos: crear(prospecto, vacante)
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
EnProceso --> Contratado: se contrata
Prospecto --> Archivado: RH archiva (CU-08)
EnProceso --> Archivado: RH archiva y libera la vacante (CU-08)
Archivado --> Prospecto: vuelve a aplicar y RH lo reactiva (CU-03 A3)
Contratado --> [*]
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

