# SC-001: Clasificación de motivo de archivo y bloqueo de reingreso a prospectos no elegibles

---

## Estado

Propuesta · 17 de noviembre de 2026

## Quién la pide

Dupla de revisión (actuando en el papel de Coordinadora de Recursos Humanos).

## Qué se pide

Quiero que cuando Recursos Humanos archive a un prospecto que no terminó su proceso (CU-08), el sistema pida obligatoriamente el motivo por el que se archiva y permita marcar si el candidato es "Recontratable (elegible en el futuro)" o "Descartado definitivo". Si una persona marcada como "Descartado definitivo" vuelve a postularse meses después, el sistema debe mostrar una alerta con el motivo por el que fue descartada y bloquear su reactivación o asignación a cualquier vacante.

## Por qué

Hoy el sistema guarda y archiva a todos los prospectos por igual (RF-003) y permite reactivarlos cuando vuelven a aplicar (CU-01 y CU-03). El problema es que no es lo mismo archivar a alguien porque no le acomodó el horario que archivarlo porque entregó documentos falsos, falló en filtros de confianza o dejó plantadas las entrevistas. Como en R.H. manejamos de 5 a 10 vacantes al mismo tiempo, meses después no recordamos por qué se archivó a cada persona.

## Qué pasa si no se hace

Recursos Humanos puede reactivar y asignar por error a candidatos que ya habían sido descartados por causas graves, perdiendo tiempo en entrevistas repetidas y poniendo en riesgo las contrataciones de las vacantes urgentes.

## Tipo de mantenimiento

- [ ] **Correctivo** · algo está mal y hay que repararlo
- [ ] **Adaptativo** · el entorno cambió y hay que ajustarse
- [x] **Perfectivo** · funciona, pero puede ser mejor
- [ ] **Preventivo** · todavía no falla, pero va a fallar

**Justificación:** El archivado y la reactivación de prospectos (RF-003, CU-08) funcionan según lo especificado y conservan el historial (RNF-03), pero añadir el motivo de archivo y el bloqueo de reingreso mejora las reglas de negocio de R.H. y evita reprocesos.

## Criterio de aceptación

1. Cuando R.H. archiva un prospecto en el CU-08, el sistema exige capturar el motivo de archivo y seleccionar si permite reingreso o si es descarte definitivo; si falta el motivo, no permite guardar.
2. Cuando R.H. intenta reactivar en el CU-01 (flujo A3) o asignar a una vacante en el CU-03 (flujo A3) a un prospecto archivado como "Descartado definitivo", el sistema deshabilita la reactivación y muestra en pantalla el motivo y la fecha en que fue descartado, sin borrar su expediente.

## Prioridad que sugiere el solicitante

Alta. Evita retrabajo directo en R.H. y protege la calidad de las contrataciones sin violar la regla de conservar el historial.

---
