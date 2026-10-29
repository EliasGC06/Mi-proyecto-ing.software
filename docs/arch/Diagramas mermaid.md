# Diagramas de guía de referencia Mermaid

## 1. Diagrama de contexto

```mermaid
flowchart TD
rec([Recepcionista])
vet([Veterinario])
sis[Sistema de la clinica]
lab[Laboratorio externo]
pago[Pasarela de pagos]
rec -->|agenda y cobra| sis
vet -->|consulta expedientes| sis
sis -->|envia estudios| lab
sis -->|cobra consultas| pago
style sis fill:#ff5d00,color:#fff
```

---

## 2. Diagrama de contenedores

```mermaid
flowchart TD
rec([Recepcionista])
subgraph sistema[Sistema de la clinica]
web[App web<br/>React]
api[API<br/>Node.js]
db[(Base de datos<br/>PostgreSQL)]
end
lab[Laboratorio externo]
rec -->|usa| web
web -->|HTTPS/JSON| api
api -->|lee y escribe| db
api -->|SFTP| lab
style api fill:#ff5d00,color:#fff
```

---

## 3. Diagrama de componentes

```mermaid
flowchart TD
web[App web]
subgraph api[API del sistema]
agenda[Agenda<br/>citas y horarios]
cobros[Cobros<br/>cuentas y pagos]
exp[Expedientes<br/>historial clinico]
notif[Notificador<br/>interfaz de envio]
end
db[(Base de datos)]
correo[Servicio de correo]
web --> agenda
web --> cobros
web --> exp
cobros -->|pregunta que se atendio| agenda
cobros -->|enviarRecibo| notif
notif --> correo
agenda --> db
cobros --> db
exp --> db
style cobros fill:#ff5d00,color:#fff
```

---

## 4. Diagrama de flujo con decisiones

```mermaid
flowchart TD
ini([Inicio])
val{Monto mayor a cero?}
exc{Excede el saldo?}
apl[Aplicar el abono]
err1[Monto invalido]
err2[Excede el saldo]
fin([Fin])
ini --> val
val -->|No| err1
val -->|Si| exc
exc -->|Si| err2
exc -->|No| apl
err1 --> fin
err2 --> fin
apl --> fin
style apl fill:#ff5d00,color:#fff
```

---

## 5. Diagrama de secuencia

```mermaid
sequenceDiagram
actor Rec as Recepcionista
participant Web as App web
participant Cob as Cobros
participant Age as Agenda
participant BD as Base de datos
Rec->>Web: Registra un abono de 500
Web->>Cob: abonar(cuenta, 500)
Cob->>Age: Que servicio se atendio?
Age-->>Cob: Consulta general
Cob->>BD: Guarda el pago
BD-->>Cob: Listo
Cob-->>Web: Pago registrado
Web-->>Rec: Muestra el nuevo saldo
```

---

## 6. Diagrama de clases

```mermaid
classDiagram
class CuentaDePaciente {
-saldo: double
-pagos: List~Pago~
+abonar(monto) string
+consultarSaldo() double
}
class Pago {
-monto: double
-fecha: Date
}
class Paciente {
-nombre: string
-especie: string
}
Paciente "1" --> "1" CuentaDePaciente : tiene
CuentaDePaciente "1" --> "*" Pago : registra
```

---

## 7. Diagrama de estados

```mermaid
stateDiagram-v2
[*] --> Agendada
Agendada --> Confirmada: el dueno confirma
Agendada --> Cancelada: se cancela a tiempo
Confirmada --> Atendida: llega a consulta
Confirmada --> NoAsistio: no se presenta
Atendida --> [*]
Cancelada --> [*]
NoAsistio --> [*]
```

---

## 8. Diagrama entidad-relación

```mermaid
erDiagram
PACIENTE ||--o{ CITA : tiene
PACIENTE ||--|| CUENTA : tiene
CITA ||--o| EXPEDIENTE : genera
CUENTA ||--o{ PAGO : registra
PACIENTE {
int id
string nombre
string especie
}
CITA {
int id
date fecha
string motivo
}
PAGO {
int id
decimal monto
date fecha
}
```
