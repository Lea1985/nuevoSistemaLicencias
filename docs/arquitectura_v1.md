# Sistema de Gestión Institucional – Arquitectura v2

## 1. Propósito

Definir el modelo de dominio base para la gestión de:

- Personas
- Cargos (SARH)
- Comisiones
- Cursos
- Horarios
- Ausencias
- Reemplazos
- Artículos/licencias

Esta arquitectura constituye la base de la versión 2 del sistema.

---

## 2. Principios del Modelo

1. El SARH identifica un cargo y nunca se modifica.
2. Si cambia el SARH → se crea un nuevo Cargo.
3. El estado del Cargo puede ser: Activo / Inactivo.
4. Toda modificación estructural genera historial (no se sobreescribe).
5. La distribución horaria semanal es fija.
6. Un reemplazo puede generar un reemplazo de reemplazo.
7. Una persona no puede estar en dos lugares al mismo tiempo.

---

## 3. Entidades Principales

### Persona

Representa cualquier trabajador de la institución.

Puede:

- Tener múltiples cargos.
- Trabajar en distintos turnos.
- Trabajar en primaria y secundaria.
- Tener un cargo de jornada completa y también módulos horarios asignados.

**Atributos agregados:**

- nombre, apellido
- domicilio
- teléfono
- mail personal y laboral
- DNI
- situación de revista

---

### Escuela

Unidad institucional.

**Atributos agregados:**

- idEscuela
- nombreEscuela
- dirección
- teléfono

---

### Curso

Ej: 1°, 2°, 3°.

**Atributos agregados:**

- idCurso
- nombreCurso
- nivel (Primaria/Secundaria)

---

### Comisión

División de un curso (Ej: 1° A, 1° B).  
Un Curso puede tener varias Comisiones.

**Atributos agregados:**

- idComision
- nombreComision
- curso_id

---

### Cargo

Unidad estructural otorgada por la provincia.

**Atributos:**

- id
- sarh (único, inmutable)
- tipo (hora cátedra / jornada / directivo / auxiliar)
- cantidad_horas
- estado (Activo / Inactivo)
- persona_id
- comision_id (nullable según tipo)
- idCurso (opcional)
- nombreCargo (opcional, descriptivo)

---

### MóduloHorario

Bloque de 40 minutos. Representa la grilla horaria institucional.

**Atributos agregados:**

- idModuloHorario
- día
- horaDesde
- horaHasta

---

### CargoModuloHorario

Tabla intermedia que distribuye las horas del Cargo en la semana.

**Campos:**

- cargo_id
- modulo_horario_id
- día_semana

Reglas:

- La suma de módulos debe coincidir con cantidad_horas.
- Es fija semanalmente.

---

### Ausencia

Representa una licencia o inasistencia.

**Campos:**

- cargo_id
- fecha_desde
- fecha_hasta
- artículo_id (referencia a Artículos)
- reemplaza_ausencia_id (nullable)
- observación (opcional)
- idHorario (opcional)
- idProfesor (opcional)

Permite cadena de reemplazos.

---

### Artículo / Licencia

Define tipos de licencia o artículo legal que aplica a una ausencia.

**Campos:**

- idArticulo
- nombreArticulo
- sueldo (opcional)
- ingreso (fecha)
- requisitos (texto)
- cantDias
- días (string o enum)
- generaReemplazo

---

### Reemplazo

Se modela implícitamente a través de:

- Nuevo Cargo temporal + Ausencia que referencia la anterior.

Permite:

- Titular → Reemplazo → Reemplazo del reemplazo.

**Campos agregados:**

- idReemplazo
- idAusencia
- idProfesor
- fechaDesde
- fechaHasta

---

### Historico / Registro de cambios

- Mantiene trazabilidad de cambios de horarios y cargos.
- Registra estado y fecha de vigencia de cada modificación.

**Campos agregados:**

- idHistorico
- horario (texto o JSON que represente distribución horaria)
- fechaVigencia

---

## 4. Reglas Horarias

- Cada hora cátedra dura 40 minutos.
- Educación Física puede dictarse en contra turno.
- Aunque se dicte en turno tarde, pertenece al horario estructural principal.
- No puede existir superposición horaria por Persona.

---

## 5. Historial

- No se eliminan registros.
- Los cargos se inactivan.
- Las ausencias mantienen trazabilidad.
- Los horarios son históricos por estado de cargo.

---

## 6. Consultas que debe soportar

- Horario vigente de una comisión.
- Horario por persona.
- Ausencias activas.
- Reemplazos en cadena.
- Distribución semanal de un cargo.
- Cargos activos por escuela.
- Validación de superposición horaria.
- Licencias por artículo.

---

## 7. Estado actual

Este documento define la arquitectura base validada conceptualmente.
Se utilizará como referencia para comenzar la implementación.

---

## 8. Stack Tecnológico (Decisiones técnicas)

**Backend:**

- Node.js + TypeScript
- Clean Architecture / DDD light: `domain / application / infrastructure / interfaces`

**ORM:**

- Prisma (tipado automático, migraciones, relaciones complejas)

**Base de Datos:**

- PostgreSQL (constraints, transacciones, CTE recursivas para reemplazos)

**Frontend:**

- Next.js + TypeScript (SSR, API routes, escalable a SaaS)

**Seguridad:**

- JWT + Refresh Tokens
- Roles: Admin / Secretaría / Directivo / Consulta

**Testing:**

- Jest + Supertest + Testing Library

---

## 9. Infraestructura

- Dockerizar backend y PostgreSQL
- Repositorio GitHub
- ESLint + Prettier
- CI básico → GitHub Actions

---

## 10. Validaciones críticas a nivel DB

- Constraints compuestos para evitar superposición horaria
- Exclusion constraints con rangos horarios
- Migraciones versionadas con seeds iniciales (módulos horarios, días de la semana)

---

## 11. Diagramas por hacer

- Diagrama ER
- Diagrama de flujo de alta de ausencia
- Diagrama de estados de Cargo
- Diagrama de cadena de reemplazos

---

## 12. Estado actual (v2)

- Arquitectura conceptual validada (v1)
- Stack tecnológico definido (v2)
- Preparado para inicio de implementación por sprints
