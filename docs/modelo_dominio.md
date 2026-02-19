# 📘 Modelo de Dominio – Sistema de Gestión Institucional v4

---

# 🏢 0. Institución (Arquitectura Multi-Tenant)

Representa la organización cliente del sistema (escuela privada, pública o instituto terciario).

Cada institución constituye un **tenant aislado** a nivel de datos.

## Atributos

- id (PK)
- nombre
- tipo_institucion (Privada / Pública / Terciario)
- cuit
- direccion
- telefono
- email_contacto
- estado (Activa / Inactiva)
- created_at
- updated_at

## Relaciones

- 1 Institucion → N Escuelas
- 1 Institucion → N Usuarios
- 1 Institucion → N Personas
- 1 Institucion → N Cargos

> 🔐 Todas las entidades operativas deben incluir `institucion_id` como clave foránea obligatoria para garantizar aislamiento de datos.

---

# 👤 Usuario

Representa los usuarios autenticados del sistema.

## Atributos

- id (PK)
- email (UNIQUE global)
- password_hash
- rol (Admin / Directivo / Secretaria / Supervisor)
- institucion_id (FK)
- estado (Activo / Inactivo)
- last_login_at
- created_at
- updated_at

---

# 👥 Persona

Representa cualquier trabajador vinculado a la institución.

## Atributos

- id (PK)
- institucion_id (FK)
- dni
- nombre
- apellido
- domicilio
- telefono
- mail_personal
- mail_laboral
- estado (Activo / Inactivo)
- created_at
- updated_at

## Restricciones

- UNIQUE (institucion_id, dni)

## Relaciones

- 1 Persona → N Designaciones

> La situación de revista pertenece exclusivamente a la Designación, no a la Persona.

---

# 🏫 Escuela

Permite modelar múltiples sedes dentro de una misma institución.

## Atributos

- id (PK)
- institucion_id (FK)
- nombre
- direccion
- telefono
- estado
- created_at
- updated_at

## Relaciones

- 1 Escuela → N Cargos

---

# 🎓 Curso

## Atributos

- id (PK)
- institucion_id (FK)
- nombre
- nivel
- estado
- created_at
- updated_at

## Relaciones

- 1 Curso → N Comisiones

---

# 🏷 Comisión

Subdivisión organizativa dentro de un curso.

## Atributos

- id (PK)
- institucion_id (FK)
- nombre
- curso_id (FK)
- estado
- created_at
- updated_at

## Relaciones

- 1 Comision → N Cargos

---

# 🧱 Cargo (Estructura Presupuestaria)

Representa el cargo estructural aprobado presup consideredario.

Es una entidad **estructural**, independiente de quién lo ocupe.

## Atributos

- id (PK)
- institucion_id (FK)
- SARH (UNIQUE por institución)
- nombre
- tipo_cargo (CAT / OTR)
- horas_semanales
- estado_estructural (Activo / Inactivo)
- escuela_id (FK)
- comision_id (nullable FK)
- created_at
- updated_at

### Datos Normativos (importados por CSV desde sistema externo)

- agrupamiento
- codigo_cgo
- tarea
- calificacion
- plan_nro
- plan_anio
- terminalidad
- descripcion

## Relaciones

- 1 Cargo → N Designaciones
- 1 Cargo → N CargoModuloHorario

---

# 📑 Designación (Ocupación del Cargo)

Representa la ocupación temporal de un cargo por una persona.

Es la entidad que modela la relación laboral efectiva.

## Atributos

- id (PK)
- cargo_id (FK)
- persona_id (FK)
- fecha_toma_posesion
- fecha_cese (nullable)
- situacion_revista
- estado_ocupacion (Ocupado / Vacante / Licencia)
- tipo_designacion
- instrumento_legal
- nro_instrumento_legal
- fecha_instrumento_legal
- motivo_novedad
- created_at
- updated_at

## Reglas de Negocio

- No puede existir más de una Designación activa simultánea para el mismo Cargo.
- Debe validarse superposición de fechas.
- La fecha_cese debe ser mayor o igual a fecha_toma_posesion.

## Relaciones

- 1 Designacion → N Ausencias

---

# 🕒 ModuloHorario

Define bloques horarios fijos de 40 minutos (configurables por institución).

Ejemplo: Módulo 1 → 07:30 a 08:10

Se cargan una sola vez por institución.

## Atributos

- id (PK)
- institucion_id (FK)
- numero_modulo
- hora_desde
- hora_hasta
- created_at
- updated_at

## Restricciones

- UNIQUE (institucion_id, numero_modulo)

---

# 📅 CargoModuloHorario (Distribución Horaria Versionada)

Define la asignación semanal de módulos a un cargo.

Permite versionado estructural mediante vigencia temporal.

## Atributos

- cargo_id (FK)
- modulo_horario_id (FK)
- dia_semana (1–7)
- vigente_desde
- vigente_hasta (nullable)
- created_at

## PK Compuesta

(cargo_id, modulo_horario_id, dia_semana, vigente_desde)

## Reglas

- No puede haber superposición de vigencias para la misma combinación cargo/día/módulo.
- Permite mantener historial estructural sin necesidad de JSON.

---

# 📜 Artículo Normativo

Representa el artículo legal que justifica una ausencia.

## Atributos

- id (PK)
- institucion_id (FK)
- nombre
- con_goce_sueldo (boolean)
- cantidad_dias_max
- genera_reemplazo (boolean)
- created_at
- updated_at

---

# 🚫 Ausencia

Representa una licencia asociada a una Designación.

## Atributos

- id (PK)
- designacion_id (FK)
- fecha_desde
- fecha_hasta
- articulo_id (FK)
- reemplaza_ausencia_id (nullable FK recursivo)
- observacion
- created_at
- updated_at

## Reglas de Negocio

- fecha_hasta ≥ fecha_desde
- Puede generar una nueva Designación de reemplazo.
- Permite cadena recursiva de reemplazos.
- No debe superponerse con otra ausencia incompatible para la misma designación.

---

# 🔎 Relaciones Clave

Persona  
↓  
Designacion  
↓  
Cargo  
↓  
Escuela  
↓  
Institucion

Designacion → Ausencia → Articulo  
Cargo → CargoModuloHorario → ModuloHorario

---

# 🧠 Principios Arquitectónicos del Modelo v4

- Arquitectura multi-tenant real con aislamiento por institución.
- Separación estricta entre estructura (Cargo) y ocupación (Designación).
- Versionado estructural de horarios mediante vigencias.
- Trazabilidad histórica completa.
- Soporte para reemplazos encadenados.
- Integración normativa vía importación CSV.
- Preparado para evolución a SaaS escalable.
- Modelo normalizado, sin uso de estructuras JSON para datos críticos.
