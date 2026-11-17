# Análisis de impacto de SC-001

> Ingeniería de Software I · SIS3407
> Autor: Elias García Cisneros · Sistema: Base de datos de vacantes y prospectos de R.H. · Fecha: 16/11/2026

---

## Solicitud que se analiza

`sc-001-clasificacion-motivo-archivo.md`. Pedida por la dupla de revisión, en el papel de Coordinadora de Recursos Humanos, el 17/11/2026. Tipo de mantenimiento: perfectivo. Prioridad sugerida por el solicitante: alta.

## Traducción a términos del sistema

Lo pedido: que al archivar a un prospecto se exija el motivo, que se marque si es "Recontratable" o "Descartado definitivo", y que un descartado definitivo no pueda reactivarse ni asignarse a una vacante si vuelve a postularse.

Traducido a mi sistema:

1. La clase **Prospecto** gana tres datos que se capturan al archivar: `motivoArchivo` (texto, obligatorio), `clasificacionArchivo` (enum Recontratable / DescartadoDefinitivo, obligatoria) y `fechaArchivo`. Gana también tres operaciones: `archivar(motivo, clasificacion)`, `reactivar()` y `esElegible()`.
2. **CU-08 (Archivar prospecto)** deja de ser "confirmar y archivar": antes de guardar exige motivo y clasificación.
3. Regla de negocio nueva: un prospecto archivado como DescartadoDefinitivo no se puede reactivar (CU-01, flujo A3) ni asignar a una vacante (CU-03, flujo A3). El sistema muestra una alerta con el motivo y la fecha, sin borrar su expediente (se respeta RNF-03).
4. El motivo y la clasificación son datos sensibles: solo los ve RH, igual que "problemas médicos" y "comentarios internos". En el diseño actual eso se logra en la política de visibilidad de cada rol (`VisibilidadRH`).

**Supuesto S-1 (por confirmar con la solicitante):** la solicitud dice "si vuelve a postularse", pero hoy RF-003 y CU-01 no dicen cómo se reconoce que es la misma persona. Prospecto ya tiene `telefono` y `correo`, así que asumo que se reconoce por nombre más teléfono o correo, con confirmación de RH. Este supuesto se retoma en el riesgo 1 y en la condición 1 de la recomendación.

## Impacto directo

| Artefacto | Unidad | Qué hay que cambiar |
|---|---|---|
| RF-003 Archivar prospecto | 2 | Agregar motivo y clasificación obligatorios al archivar; precisar que reactivar depende de la clasificación. |
| RF-014 (nuevo) Bloquear reingreso de descartados definitivos | 2 | Nuevo requisito: bloquear reactivación y asignación, y mostrar alerta con motivo y fecha. Su criterio de aceptación es el punto 2 de la SC-001. |
| CU-08 Archivar prospecto | 2 | Agregar la captura de motivo y clasificación antes de confirmar; nuevo flujo alterno A4 (falta motivo o clasificación: no guarda). |
| CU-01 Registrar prospecto (A3) | 2 | Si el registro archivado es DescartadoDefinitivo: mostrar alerta y no ofrecer reactivar. |
| CU-03 Asignar prospecto (paso 3 y A3) | 2 | El descartado definitivo no aparece como elegible; si RH lo busca, ve la alerta y no puede asignarlo. |
| Prototipo en Figma | 2 | Diálogo de archivar con los dos campos nuevos y aviso de alerta de reingreso. |
| Trazabilidad y registro de cambios de la SRS | 2 | Agregar RF-014 y actualizar RF-003, CU-08, CU-01 y CU-03. |
| Diagrama de clases | 4 | Prospecto: `-motivoArchivo`, `-clasificacionArchivo`, `-fechaArchivo`, `+archivar(motivo, clasificacion)`, `+reactivar()`, `+esElegible()`; nuevo enum `ClasificacionArchivo`; nueva dependencia `Prospecto ..> Evento : crea` (hoy solo Postulacion crea eventos); `VisibilidadRH` incluye los campos nuevos. |
| Diagrama de secuencia (CU-03) | 4 | Agregar `API->>Pro: esElegible()` y una rama `alt` de rechazo con motivo y fecha (A3). |
| Máquina de estados | 4 | La transición Archivado → Prospecto lleva la guarda `[clasificación ≠ DescartadoDefinitivo]`. Además, su etiqueta hoy dice solo "CU-03 A3"; debe decir "CU-01 A3 / CU-03 A3", porque la reactivación ocurre por ambos flujos. |
| Diagrama de componentes (C4) | 3 | Agregar la dependencia de Prospectos hacia Bitácora y Notificaciones. Para respetar el patrón Observer del ADR-001, se redacta así: Prospectos publica el evento de archivado y Bitácora y Notificaciones lo reciben (no son llamadas directas). Hoy solo Postulaciones los alcanza, y archivar un prospecto sin postulación no quedaría en la bitácora ni avisaría por correo. |

## Impacto indirecto

| Artefacto | Unidad | Por qué se ve afectado |
|---|---|---|
| RF-010 Datos sensibles solo para RH y RNF-01 | 2 | Motivo y clasificación pasan a ser datos sensibles; se agregan a RF-010 y a las pruebas de los tres roles de RNF-01. |
| RF-009 y `PoliticaVisibilidad` | 2 y 4 | `VisibilidadFinanzas` y `VisibilidadCorporativo` no cambian si devuelven solo estatus, puesto y urgencia (lista permitida). Si alguna usa lista de exclusión, hay que excluir los campos nuevos. Verificar en el código. |
| RF-013 y `Notificador` | 2 y 4 | El correo se arma con el `Evento`; el evento de archivado debe decir solo "prospecto archivado", sin motivo ni clasificación. |
| RNF-06 y `Bitacora` | 2 y 4 | `RegistroBitacora` guarda usuario, acción y fecha, sin texto de motivo; por eso el motivo vigente vive en Prospecto. Archivar debe generar un Evento para que la bitácora lo registre. |
| RF-008 y CU-04 Historial | 2 | El historial de un archivado debe mostrar a RH motivo, clasificación y fecha. |
| RF-002 Estatus del prospecto | 2 | No se agrega un estatus nuevo: la clasificación es un atributo del estado "Archivado", no un estado. Decisión de diseño que queda documentada aquí. |
| Vacante y Postulacion (archivar desde "En proceso") | 4 | El diagrama de estados ya permite Archivado desde "En proceso" y libera la vacante (CU-08). No cambian sus atributos ni operaciones, pero hay que verificar que archivar como DescartadoDefinitivo siga el mismo camino: libere la vacante, cierre la postulación y la bitácora lo registre. Se agrega una prueba de regresión (ver estimación). |
| Registros ya archivados (datos existentes) | 4 | No tienen motivo ni clasificación; hay que definir un valor por omisión para que la obligatoriedad no los invalide. **Decisión propuesta:** asignarles Recontratable, sin motivo registrado. Esto tiene un efecto de negocio explícito: ningún prospecto archivado antes del cambio queda bloqueado, y RH tendría que reclasificarlo a mano si lo necesita. |
| ADR-001 Monolito modular | 3 | Sin impacto. El cambio ocurre dentro de módulos que ya existen (Prospectos, Usuarios y Roles, Bitácora, Notificaciones). La transacción única que fija el ADR protege que archivar y registrar en la bitácora ocurran juntos. Tampoco se cumple ninguna condición de revisión del ADR (multi-tenancy o carga). |
| Diagrama de contenedores C4 | 3 | Sin impacto: no hay contenedor ni flecha nueva; los campos viven en el PostgreSQL existente. |
| Diagrama de contexto C4 | 3 | Sin impacto: no hay sistemas externos nuevos. |

## Lo que queda sin tocar

- **Requisitos:** RF-001, RF-004 (el motivo obligatorio se define en RF-003 y CU-08; las validaciones de teléfono y correo no cambian), RF-005, RF-006, RF-007, RF-011, RF-012, RNF-02, RNF-04 y RNF-05 (el historial agrega campos, no clics).
- **Casos de uso:** CU-02, CU-05, CU-06, CU-07 y CU-09, y el diagrama de casos de uso.
- **Clases:** Empresa, Vacante, Documento, Postulacion, Usuario, Observador, ServicioCorreo y AdaptadorSMTP (Vacante y Postulacion solo con la verificación indicada en el impacto indirecto).
- **Arquitectura:** los cuatro contenedores, el monolito modular y los patrones Observer, Adapter y Strategy (se reutilizan; no se agrega ninguno).
- **Alcance "fuera de alcance":** sigue igual; no se agrega comparación automática de prospectos.

**Verificación aparte:** RNF-03 se sigue cumpliendo, porque el descartado definitivo se archiva y no se elimina.

## Riesgos

| # | Riesgo | Probabilidad | Impacto | Cómo se reduce |
|---|---|---|---|---|
| 1 | El reingreso se detecta por nombre (supuesto S-1): un homónimo inocente queda bloqueado, o el descartado se registra con una variación del nombre y se salta el bloqueo. | Media | Alto | Comparar también `telefono` o `correo` y pedir confirmación de RH antes de bloquear. |
| 2 | El motivo se filtra a Finanzas o Corporativo (pantalla o correo de RF-013). | Baja si las políticas son listas permitidas; media si no | Alto | Campos solo en `VisibilidadRH`, evento sin motivo y pruebas de los tres roles en RNF-01. |
| 3 | Un error de captura deja a alguien como DescartadoDefinitivo sin forma de corregirlo. | Media | Medio | Definir quién puede cambiar la clasificación (por ejemplo, solo la coordinadora de RH) y registrarlo en la bitácora. |
| 4 | El motivo puede contener acusaciones (documentos falsos) o datos personales; conservarlo para siempre puede chocar con derechos de los titulares sobre sus datos o con criterios de no discriminación. | Baja o media | Alto | Revisarlo con el área legal o de cumplimiento de la empresa antes de implementar, tomando como marco a consultar la Ley Federal de Protección de Datos Personales en Posesión de los Particulares; acotar el motivo a hechos concretos del proceso. No es asesoría legal. |
| 5 | Los registros archivados antes del cambio quedan sin clasificación y rompen la regla de obligatoriedad. | Alta | Bajo | Valor por omisión definido en la migración (ver decisión en el impacto indirecto). |
| 6 | El motivo en texto libre queda vago ("no sirvió") y no cumple el propósito. | Media | Bajo | Ofrecer una lista de motivos más un campo de detalle. |
| 7 | **Riesgo de no hacerlo:** RH reactiva o asigna a descartados por causas graves, repite entrevistas y retrasa vacantes urgentes. | Media (supuesto: se basa en lo que plantea la solicitante, no en datos medidos) | Medio | Es la razón de la solicitud. |

## Alternativas consideradas

1. **Opción A · Atributos en Prospecto.** Se agregan `motivoArchivo`, `clasificacionArchivo` y `fechaArchivo` a la clase Prospecto y una guarda en `reactivar()` y `esElegible()`. Cuesta poco: un cambio de clase, una validación y una pantalla. Deja pendiente que, si una persona se archiva, se reactiva y se archiva otra vez, solo se conserva el último motivo (la bitácora guarda quién y cuándo, no el texto).
2. **Opción B · Entidad separada `RegistroDeArchivo`.** Cada archivado crea un registro (Prospecto 1..* RegistroDeArchivo) con motivo, clasificación, fecha y usuario. Conserva todo el historial y es más coherente con RNF-03 y RNF-06. Cuesta más: una clase y una relación nuevas, cambios en el historial (RF-008) y más pruebas. Deja pendiente decidir cuál registro manda si hay varios con clasificaciones distintas.

Me inclino por la **A**: el motivo vigente es el que importa para el bloqueo, el costo es menor y la B se puede adoptar después sin romper A, porque los datos de A se migran como el primer registro de B.

## Estimación de esfuerzo

| Concepto | Esfuerzo |
|---|---|
| Cambios de diseño y documentación | 9 h |
| Implementación | 12 h |
| Pruebas | 7 h |
| **Total** | **28 h (rango 23 a 34 h, unas 3.5 jornadas de 8 h; el rango equivale a unas 3 a 4 jornadas)** |

**De dónde sale este número.** Conté artefactos y les asigné un tiempo por unidad.
- **Diseño y documentación (9 h):** 9 elementos de la SRS (RF-003, RF-014, RF-010, RF-013, RF-008, CU-08, CU-01, CU-03 y la trazabilidad) a unos 30 min son 4.5 h; los diagramas de clases, secuencia y estados, más el de componentes, 3 h; el prototipo, 1.5 h.
- **Implementación (12 h):** campos y migración (2 h), formulario de archivar con validación (2 h), bloqueo en `reactivar()` y `esElegible()` (3 h), alerta (1.5 h), campos en `VisibilidadRH` (1 h), exclusión del motivo en el correo (0.5 h), y que Prospecto publique su evento a Bitácora y Notificaciones (2 h).
- **Pruebas (7 h):** los 2 criterios de aceptación, la prueba de los tres roles de RNF-01 con campos nuevos, que archivar un prospecto sin postulación quede en la bitácora y en el correo, y la regresión de CU-01, CU-03 y CU-08 (incluida la liberación de la vacante al archivar desde "En proceso").
- **Rango:** aplicué aproximadamente ±20 % al total (22.4 a 33.6 h), redondeado a 23 y 34 h, porque es una estimación por conteo y no por medición.
- **Supuestos:** una sola persona que ya conoce el sistema y se elige la Opción A.
- **No incluí:** la revisión legal del riesgo 4, ni cambiar el reconocimiento de reingreso a un identificador único (por ejemplo, CURP). Con la Opción B suben unas 6 a 8 h. El número viene de mi conteo de artefactos y no de un cambio previo comparable; es una estimación, no una medición.

## Recomendación

**Aceptar, con condiciones** que se resuelven antes de implementar:

1. Confirmar con la solicitante el supuesto S-1: cómo se reconoce el reingreso (nombre más teléfono o correo, con confirmación de RH).
2. Dejar los campos nuevos solo para RH, incluido el correo de RF-013.
3. Definir quién puede corregir una clasificación y confirmar el valor por omisión de los registros ya archivados (Recontratable, sin motivo registrado).
4. Revisar con el área legal el punto de conservar motivos de descarte.

Se acepta porque el costo es acotado (unas 3.5 jornadas), no toca vacantes ni la arquitectura y reduce el retrabajo que hoy sufre RH en vacantes urgentes. Si las condiciones 1 o 4 no se pueden resolver, aplazo el bloqueo y entrego primero solo el motivo de archivo, que no tiene riesgo de bloquear a la persona equivocada.

---

*El objetivo de este documento no es dar un número exacto. Es que cualquiera pueda seguir mi razonamiento y señalarme dónde me equivoqué.*

