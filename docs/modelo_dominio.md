# Modelo de Dominio – Sistema de Gestión Institucional v2

## 1. Entidades Principales

### Persona

Representa cualquier trabajador de la institución.

**Atributos**

- id (PK)
- nombre
- apellido
- domicilio
- telefono
- mail_personal
- mail_laboral
- dni
- situacion_revista

Relaciones:

- 1 Persona → N Cargos

---

### Escuela

**Atributos**

- idEscuela (PK)
- nombreEscuela
- direccion
- telefono

---

### Curso

**Atributos**

- idCurso (PK)
- nombreCurso
- nivel

Relaciones:

- 1 Curso → N Comisiones

---

### Comision

**Atributos**

- idComision (PK)
- nombreComision
- curso_id (FK)

---

### Cargo

**Atributos**

- id (PK)
- sarh (UNIQUE, inmutable)
- tipo
- cantidad_horas
- estado (Activo / Inactivo)
- persona_id (FK)
- comision_id (nullable)
- idCurso (nullable)
- nombreCargo

Relaciones:

- 1 Cargo → N Ausencias
- N Cargo → N ModuloHorario (via CargoModuloHorario)

---

### ModuloHorario

**Atributos**

- idModuloHorario (PK)
- dia
- horaDesde
- horaHasta

---

### CargoModuloHorario

Tabla intermedia de distribución horaria.

**Campos**

- cargo_id (FK)
- modulo_horario_id (FK)
- dia_semana

PK compuesta: (cargo_id, modulo_horario_id)

---

### Articulo

**Atributos**

- idArticulo (PK)
- nombreArticulo
- sueldo
- cantDias
- generaReemplazo

---

### Ausencia

**Atributos**

- idAusencia (PK)
- cargo_id (FK)
- fecha_desde
- fecha_hasta
- articulo_id (FK)
- reemplaza_ausencia_id (FK nullable)
- observacion

Permite relación recursiva para cadena de reemplazos.

---

### Historico

**Atributos**

- idHistorico (PK)
- cargo_id (FK)
- horario_json
- fechaVigencia
