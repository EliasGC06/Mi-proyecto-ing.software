# Especificación de requisitos

Sistema: Base de Datos de vacantes y prospectos R.H.

Autor: Elias García Cisneros

Versión: 0.1 

Fecha de la última actualización: 30/09/2026

## 1. Propósito y alcance

Enlace Figma: 

Propósito del documento: En este documento se especifica lo que se hace y con que calidad debe tener el sistema que desarrollaremos para recopilar y concentrar la información de las vacantes, prospectos y personal activo de la empresa. Esté nace en base a la visión del producto que hicimos y la entrevista realizada.

Alcance del sistema: Registrar prospectos según su vacante, darles un estatus, verificar sus datos, filtrar la información, adjuntar y consultar sus documentos, ver el historial profesional del prospecto, restringir la información sensible a las otras áreas y mostrar con prioridad las vacantes urgentes.   

Fuera del alcance:

| Fuera | Por qué |
|---|---|
| Comparar prospectos automáticamente. | No hay criterios de evaluación definidos |
| Reportes financieros o de nómina. | Este tema lo manejaría Contaduría/Finanzas especificamente, confirmado en la entrevista (pregunta 13) |
| Notificaciones automáticas por correo o SMS. | Requiere un desarrollo más extenso no contemplado para esta versión. | 

## 2. Usuarios y su contexto

| Usuario | Qué hace hoy sin el sistema | Qué espera del sistema | 
|---|---|---|
| Recursos Humanos | Gestiona de 5 a 10 vacantes activas a la vez, de distintas empresas, horarios, zonas y etapas. Buscando archivo por archivo lo que necesita y hablando temas con otras áreas en persona, perdiendo tiempo y esfuerzo. | Registrar, asignar, dar seguimiento y encontrar el historial de un prospecto en un solo lugar. |
| Finanzas/Contaduría | Pregunta que vacantes ya están ocupadas y por quien, en lo cual recopila sus datos o documentos (Numero de cuenta, banco, adeudos importantes como INFONAVIT O FONACOT), también si un empleado renuncia, para cerrar su nomina y verificar que le deben dar de finiquito o no. | Consultar el estatus de las vacantes en tiempo real en un solo lugar. |
| Corporativo | Supervisa, coordina y comunica con R.H sobre las vacantes, prospectos y empleados que tiene la empresa, preguntando en persona sobre su estatus de cada uno; quitando mucho tiempo y esfuerzo. | Supervisar el estatus de todo el personal en una sola plataforma, viendo con claridad y transparencia lo que su cede con cada uno sin tener que preguntar al área de R.H. |

**Conflictos identificados entre usuarios:**
Finanzas/Contaduría y Corporativo quieren visibilidad amplia, pero los datos sensibles (problemas médicos, comentarios internos, temas familiares) solo los puede ver R.H, como se confirmo en la entrevista, la visibilidad por área, las demás áreas que no son R.H ven únicamente estatus, puesto y urgencia. 

## 3. Requisitos funcionales

### 3.1 
| ID | Nombre | Prioridad | Origen |
|---|---|---|---|
| RF-001 | Registrar prospecto y vacante | Alta | Visión (alcance 1 y 2) y P5 |
| RF-002 | Mantener estatus del prospecto, si no termina su proceso, y la vacante | Alta | Visión (alcance 3) y P12 |
| RF-003 | Archivar prospecto que no termina su proceso, para usar su registro en un futuro, si vuelve a aplicar | Alta | P10 y P12 |
| RF-004 | Validar campos obligatorios, formato de teléfono y correo | Alta | Visión (alcance 4) |
| RF-005 | Impedir que un prospecto esté activo en dos vacantes | Alta | P11 |
| RF-006 | Filtrar vacantes y prospectos |Alta| Visión (alcance 5) |
| RF-007 | Adjuntar documentos a un prospecto | Media | Visión (alcance 6) |
| RF-008 | Consultar el historial profesional de un prospecto en una solo lugar| Alta | P7 y P9|
| RF-009 | Limitar lo que ven Finanzas/Contaduría y Corporativo | Alta | Visión (resolución del conflicto) | 
| RF-010 | Reservar los datos sensibles a RH | Alta | Visión (regla 2) |
| RF-011 | Mostrar las vacantes urgentes con prioridad | Alta | P8 |
| RF-012 | Señalar las vacantes reabiertas a Finanzas/Contaduría | Alta | P4, P8 |

### 3.2 Fichas 

#### RF-001 - Registrar prospecto
 
## RF-001 · Registrar prospecto y vacante
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe permitir a RH registrar prospectos con nombre, edad, puesto, empresa, horario, localidad, escolaridad, teléfono y correo, y vacantes con puesto, área, horario, localidad, nivel de urgencia y motivo de apertura. |
| Origen | Supuesto (Visión, alcance 1 y 2). El campo "motivo de apertura" está Confirmado (P5). |
| Prioridad | Alta |
| Criterio de aceptación | Dado un usuario RH autenticado, cuando captura todos los campos de un prospecto y guarda, entonces aparece en el listado con estatus "prospecto"; cuando captura todos los campos de una vacante y guarda, entonces aparece en el listado con estatus "disponible" y su nivel de urgencia. |
| Relacionado con | CU-001, CU-002 · RF-002, RF-004 |
 
## RF-002 · Mantener estatus del prospecto y de la vacante
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe asignar a cada prospecto exactamente un estatus (prospecto, en proceso, contratado o archivado) y a cada vacante exactamente uno (disponible u ocupada), y permitir a RH reabrir una vacante ocupada. |
| Origen | Supuesto (Visión, alcance 3). El estatus "archivado" está Confirmado (P12) y la reapertura de vacantes viene de la excepción de la ficha de dominio (P4). |
| Prioridad | Alta |
| Criterio de aceptación | Todo prospecto y toda vacante guardados tienen un solo estatus válido; cuando RH reabre una vacante ocupada e indica el motivo, su estatus pasa a "disponible", conserva su urgencia y se registra usuario, fecha y motivo. |
| Relacionado con | CU-003, CU-007, CU-008 · RF-003, RF-012 · RNF-003, RNF-009 |
 
## RF-003 · Archivar prospecto que no termina su proceso y reutilizar su registro
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe cambiar a "archivado" a un prospecto que no termina su proceso, sin eliminar su información, y reutilizar ese registro si vuelve a aplicar. |
| Origen | Confirmado (P10, P12) |
| Prioridad | Alta |
| Criterio de aceptación | Dado un prospecto "en proceso" que RH descarta, cuando RH confirma, entonces su estatus es "archivado" y sus datos y documentos siguen consultables; cuando RH registra a una persona que ya tiene un registro archivado, el sistema le muestra ese registro y le permite reactivarlo en lugar de crear uno nuevo. |
| Relacionado con | CU-008, CU-001 · RF-002, RF-008 · RNF-004 |
 
## RF-004 · Validar campos obligatorios, formato de teléfono y correo
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe impedir guardar un registro con campos obligatorios vacíos, con un teléfono que no tenga 10 dígitos o con un correo sin formato válido. |
| Origen | Supuesto (Visión, alcance 4) |
| Prioridad | Alta |
| Criterio de aceptación | Cuando RH guarda con un campo obligatorio vacío, con teléfono "55123" o con correo "juan@", el sistema no guarda, marca cada campo con error e indica el formato esperado. |
| Relacionado con | CU-001, CU-002 · RF-001 · RNF-005 |
 
## RF-005 · Impedir que un prospecto esté activo en dos vacantes
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe rechazar la asignación de un prospecto que ya esté activo en otra vacante. |
| Origen | Confirmado (P11) |
| Prioridad | Alta |
| Criterio de aceptación | Dado un prospecto "en proceso" en la vacante A, cuando RH intenta asignarlo a la vacante B, el sistema rechaza la asignación, indica que está activo en la vacante A y el prospecto sigue ligado solo a A. |
| Relacionado con | CU-003 · RF-002 |
 
## RF-006 · Filtrar vacantes y prospectos
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe filtrar los listados de vacantes y de prospectos por puesto, urgencia, localidad y estatus. |
| Origen | Supuesto (Visión, alcance 5) |
| Prioridad | Alta |
| Criterio de aceptación | Con uno o más filtros aplicados en cualquiera de los dos listados, solo aparecen los registros que cumplen todos los filtros. |
| Relacionado con | CU-005, CU-003 · RNF-007 |
 
## RF-007 · Adjuntar documentos a un prospecto
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe permitir a RH adjuntar documentos PDF, JPG o PNG de hasta 10 MB a un prospecto. |
| Origen | Supuesto (Visión, alcance 6). El formato y el tamaño son propuesta. |
| Prioridad | Media |
| Criterio de aceptación | Cuando RH adjunta un PDF de 2 MB, aparece en los documentos del prospecto con nombre y fecha; un archivo de 15 MB es rechazado con un mensaje. |
| Relacionado con | CU-001, CU-004 · RF-008 |
 
## RF-008 · Consultar el historial profesional de un prospecto en un solo lugar
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe mostrar en una sola pantalla los datos, documentos y vacantes anteriores de un prospecto, incluso si aplicó hace meses. |
| Origen | Confirmado (P7, P9) |
| Prioridad | Alta |
| Criterio de aceptación | Dado un prospecto que aplicó hace seis meses, cuando RH lo busca por nombre y abre su registro, ve en una sola pantalla sus datos, sus documentos y las vacantes a las que aplicó, con fechas. |
| Relacionado con | CU-004 · RF-003, RF-007 · RNF-006 |
 
## RF-009 · Limitar lo que ven Finanzas/Contaduría y Corporativo
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe mostrar a los usuarios de Finanzas/Contaduría y Corporativo únicamente estatus, puesto y urgencia. |
| Origen | Supuesto (Visión, resolución del conflicto) |
| Prioridad | Alta |
| Criterio de aceptación | Dado un usuario de Finanzas/Contaduría o Corporativo, cuando abre una vacante o un prospecto, ve solo estatus, puesto y urgencia; no ve documentos ni datos sensibles. |
| Relacionado con | CU-006 · RF-010 · RNF-001 |
 
## RF-010 · Reservar los datos sensibles a RH
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe permitir ver y editar los campos "problemas médicos" y "comentarios internos" solo a usuarios RH. |
| Origen | Supuesto (Visión, regla 2) |
| Prioridad | Alta |
| Criterio de aceptación | Dado un usuario sin rol RH, cuando intenta abrir o editar esos campos, el sistema no los muestra y rechaza la edición. |
| Relacionado con | CU-001, CU-004 · RF-009 · RNF-001, RNF-009 |
 
## RF-011 · Mostrar las vacantes urgentes con prioridad
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe mostrar las vacantes urgentes antes que las demás y con una etiqueta "Urgente" para todas las áreas. |
| Origen | P8. Supuesto: la visibilidad prioritaria para todas las áreas no se confirmó en la entrevista. |
| Prioridad | Alta |
| Criterio de aceptación | En el listado de vacantes de cualquier rol, las vacantes urgentes aparecen en las primeras posiciones con la etiqueta "Urgente". |
| Relacionado con | CU-005, CU-006 · RF-006 |
 
## RF-012 · Señalar las vacantes reabiertas a Finanzas/Contaduría
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe mostrar a Finanzas/Contaduría, al iniciar sesión, las vacantes reabiertas en las últimas 24 horas. |
| Origen | Confirmado (P4, P8) la necesidad de avisar a Finanzas. Supuesto que sea mediante una sección en el sistema, porque las notificaciones automáticas están fuera del alcance. |
| Prioridad | Alta |
| Criterio de aceptación | Dado una vacante reabierta hoy, cuando un usuario de Finanzas/Contaduría abre su pantalla de inicio, la ve en "Reabiertas recientes" con fecha y hora de reapertura. |
| Relacionado con | CU-007 · RF-002 · RNF-003 |

## 4. Requisitos no funcionales

### 4.1 Resumen

|ID|Atributo de calidad|Descripción Métrica|Origen|Prioridad|Por qué importa|Afecta a|
|---|---|---|---|---|---|---|
|RNF-01|Confidencialidad|El 100% de los intentos de un usuario sin rol RH por ver o editar datos sensibles (problemas médicos, comentarios internos) debe ser rechazado. Se verifica con pruebas de los tres roles.| Visión (atributo de confidencialidad)| Alta | Son datos de salud y comentarios internos; una filtración daña a la persona y genera riesgo legal, por eso no admite excepciones.|RF-10, RF-09, CU-01, CU-04, CU-06|
|RNF-02|Integridad|Un cambio de estatus de una vacante o prospecto debe ser visible para los tres roles en 5 segundos o menos.| Entrevista (P8 y ficha de dominio: respuestas desactualizadas a Contaduría); Visión (integridad)|Alta| Hoy Contaduría recibe respuestas desactualizadas porque la información no se sincroniza entre áreas; con 5 segundos el problema desaparece en la práctica.|RF-09, RF-12, CU-03, CU-06, CU-07|
|RNF-03|Integridad|Cero registros de prospecto eliminados físicamente: toda baja se hace por archivado y se conservan sus datos y documentos. | Confirmado (P10, P12)| Alta | La información de un prospecto nunca se borra, porque puede volver a aplicar y RH necesita su historial.| RF-03, CU-01, CU-08|
|RNF-04|Usabilidad|Una persona de RH que no haya usado el sistema debe registrar un prospecto completo en 3 minutos o menos, sin capacitación. Se prueba con 3 personas.|Visión (usabilidad)|Alta|Si el sistema es más lento que Excel o WhatsApp, RH regresa al método anterior y el sistema fracasa.|RF-01, RF-07, RF-08, CU-01|
|RNF-05|Usabilidad|Encontrar el historial de un prospecto debe tomar 30 segundos o menos y 3 clics como máximo. | Confirmado (P7, P9)| Alta | Buscar archivo por archivo es el mayor dolor reportado por RH y lo que más tiempo le quita.|RF-08, CU-04|
|RNF-05|Trazabilidad|El 100% de los cambios de estatus y de datos sensibles debe registrar usuario, fecha y hora. | Visión (Corporativo teme falta de transparencia)| Media | Permite saber quién cambió qué y cuándo, y evita incongruencias entre áreas.|RF-09, RF-10, RF-12, CU-03, CU-07, CU-08|


## 5. Casos de uso

Se trabajan en la semana 7, después de la entrevista. Cada caso de uso se relaciona con los requisitos funcionales que realiza.

|Casos de uso| Descripción |
|---|---|
| CU-01 | Registrar prospecto |
| CU-02 | Registrar vacante |
| CU-03 | Asignar prospecto a vacante |
| CU-04 | Consultar historial de un prospecto |
| CU-05 | Filtar vacantes y prospectso |
| CU-06 | Consultar estatus de vacantes |
| CU-07 | Reabrir vacante ocupada |
| CU-08 | Archivar prospectos |


## 6. Trazabilidad

Esta tabla es la que hace posible el análisis de impacto de la semana 15. Mantenla actualizada conforme cambien los requisitos.

| Requisito | Origen | Caso de uso | Elemento del prototipo |
|---|---|---|---|
| RF-01 | Visión | CU-01 | — |
| RF-02 | Visión, P5 | CU-02 | — |
| RF-03 | Visión, P12 | CU-01, CU-03, CU-08 | P4, P5 |
| RF-04 | Visión | CU-02, CU-03, CU-07 | P1, P2, P5 |
| RF-05 | P10, P12 | CU-08 | — |
| RF-06 | P10 | CU-01 | — |
| RF-07, RF-08 | Visión | CU-01, CU-02 | — |
| RF-09 | P11 | CU-03 | P6 (flujo alterno) |
| RF-10 | Visión | CU-05 | P1 |
| RF-11 | Visión | CU-05, CU-03 | P3 |
| RF-12 | Visión | CU-01, CU-04 | — |
| RF-13 | P7, P9 | CU-04 | — |
| RF-14 | Visión | CU-06 | P7 |
| RF-15 | Visión | CU-01, CU-04 | P7 |
| RF-16 | Visión | CU-05, CU-06 | P1 |
| RF-17 | Ficha, P4, P8 | CU-07 | — |
| RF-18 | P4, P8 | CU-07 | — |
| RF-19 | Derivado | Transversal | — |


## 7. Registro de cambios

| Fecha | Requisito | Qué cambió | Por qué |
|---|---|---|---|
| 30/09/2026 | RF-05 | Se agregó "Archivar prospecto que no avanza" | La entrevista confirmó que la información de un prospecto nunca se borra, se archiva (P10, P12) | Sí |
| 30/09/2026 | RF-06 | Se agregó "Reutilizar el registro de un prospecto que vuelve a aplicar" | La entrevista confirmó que si vuelve a aplicar se actualiza su registro con la información nueva (P10) |
| 30/09/2026 | RF-13 | Se agregó "Consultar el historial de un prospecto en una sola pantalla" | Buscar archivo por archivo apareció como el mayor dolor, repetido en dos respuestas (P7, P9) | 
| 30/09/2026 | RF-17 | Se agregó "Reabrir una vacante ocupada" | La excepción de la ficha de dominio (vacante urgente que se cierra de golpe) no estaba en la Visión; se confirmó con P4 y P8 | 
| 30/09/2026 | RF-18 | Se agregó "Señalar las vacantes reabiertas a Finanzas y Contaduría" | Hay que avisar con urgencia a Finanzas (P4, P8); como las notificaciones automáticas están fuera del alcance, se resuelve con una sección visible en el sistema | Sí |
| 30/09/2026 | RF-03 | Se añadió el estatus "archivado" a los estatus del prospecto | La entrevista indicó que los prospectos no contratados se archivan (P12); la Visión solo tenía prospecto, en proceso y contratado | 
| 30/09/2026 | RF-02 | Se agregó el campo "motivo de apertura" a la vacante | La urgencia depende de cómo se desocupó la vacante (P5) | Sí |
| 30/09/2026 | RF-15 y RNF-04 | La Visión permitía a RH "eliminar" datos sensibles; se sustituyó por archivado y se prohíbe el borrado físico | Contradice lo confirmado en la entrevista: la información nunca se borra (P10, P12) | 
| 30/09/2026 | RF-16 | La prioridad bajó de Alta a Media y quedó marcado como supuesto pendiente de validar | La entrevista no confirmó que las vacantes urgentes deban verse con prioridad para todas las áreas; P4 indica que las áreas interactúan solo cuando es necesario | 
| 30/09/2026 | RF-19 | Se agregó "Autenticar usuarios con un rol" | Es necesario para poder cumplir RF-14 y RF-15 (visibilidad por área); no estaba en la Visión | Sí |
| [FECHA] | [REQUISITO] | [CAMBIO A PARTIR DE LA REVISIÓN DE LA DUPLA] | [OBSERVACIÓN QUE LO MOTIVÓ] | 

## Revisión de la dupla 
| Campo | Elemento |
|---|---|
| Revisora| |
| Fecha de revisión |  |
| Observaciones recibidas |  |
| Cambios realizados |  |

    [ ] Todos los requisitos tienen identificador único y ninguno está repetido
    [ ] Cada requisito expresa una sola idea
    [ ] Cada requisito funcional tiene criterio de aceptación comprobable
    [ ] Cada requisito no funcional tiene una métrica, no solo un adjetivo
    [ ] El campo Origen distingue lo confirmado por el cliente de lo que sigo suponiendo
    [ ] Hay al menos un requisito no funcional por cada atributo de calidad que impone mi tipo de sistema
    [ ] Ningún requisito impone una solución técnica
    [ ] Todos los requisitos caben dentro del alcance declarado
    [ ] La tabla de trazabilidad está completa
    [ ] Mi dupla revisó el documento y su revisión está registrada
    [ ] Borré los ejemplos y las instrucciones en cursiva
