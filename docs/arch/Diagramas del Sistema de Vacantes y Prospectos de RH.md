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
