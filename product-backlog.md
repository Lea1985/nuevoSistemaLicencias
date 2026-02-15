# PRODUCT BACKLOG – Sistema de Gestión Institucional (v2)

Stack definido:

- Next.js + TypeScript (App Router)
- Prisma ORM
- PostgreSQL
- Clean Architecture interna (domain / application / infrastructure / interfaces)
- JWT + Roles
- CI básico

---

# 🔵 EPIC 0 – Diseño y Modelado Inicial

## 🟢 Feature 0.1 – Modelado Conceptual

- Crear Diagrama ER completo
- Validar relaciones críticas:
  - Persona – Cargo
  - Cargo – Comisión
  - Cargo – MóduloHorario
  - Ausencia – Reemplazo (cadena)
- Validar reglas de negocio contra el modelo

## 🟢 Feature 0.2 – Modelado de Reemplazos

- Diseñar diagrama de cadena de reemplazos
- Validar soporte para N niveles
- Validar impacto en consultas (CTE recursiva)

## 🟢 Feature 0.3 – Ajustes previos a implementación

- Ajustar entidades antes de crear schema Prisma
- Revisar restricciones críticas
- Confirmar estructura final de dominio

---

# 🔵 EPIC 1 – Infraestructura Técnica Base

## 🟢 Feature 1.1 – Setup del Proyecto

- Crear proyecto Next.js + TypeScript (App Router)
- Configurar Prisma ORM
- Configurar PostgreSQL
- Configurar variables de entorno
- Implementar estructura Clean Architecture interna:
  - domain
  - application
  - infrastructure
  - interfaces
- Configurar ESLint + Prettier
- Configurar GitHub Actions (CI básico)
- Dockerizar base de datos (opcional)

---

# 🔵 EPIC 2 – Modelado Base de Dominio

## 🟢 Feature 2.1 – Modelado Inicial DB

- Definir schema Prisma base
- Crear migración inicial
- Definir seeds para:
  - Días de semana
  - Módulos horarios

---

# 🔵 EPIC 3 – Interfaz Base (UI mínima estructural)

## 🟢 Feature 3.1 – Layout Base

- Crear layout principal
- Sidebar de navegación
- Header
- Sistema básico de rutas protegidas

## 🟢 Feature 3.2 – Páginas Placeholder

- Página Personas
- Página Cargos
- Página Ausencias
- Página Artículos
- Página Escuelas

Objetivo: validar flujo general del sistema desde Sprint 2.

---

# 🔵 EPIC 4 – Gestión Estructural Académica

## 🟢 Feature 4.1 – Escuela

- Crear escuela
- Editar escuela
- Listar escuelas

## 🟢 Feature 4.2 – Curso

- Crear curso
- Asociar a escuela
- Listar cursos por escuela

## 🟢 Feature 4.3 – Comisión

- Crear comisión
- Asociar a curso
- Listar comisiones por curso

---

# 🔵 EPIC 5 – Gestión de Personas

## 🟢 Feature 5.1 – CRUD Persona

### User Stories

- Como administrador quiero crear una persona
- Como administrador quiero editar datos personales
- Como administrador quiero listar personas
- Como administrador quiero ver detalle de persona

### Validaciones

- DNI único
- Email válido
- No permitir eliminación si tiene cargos asociados

---

# 🔵 EPIC 6 – Gestión de Cargos (Núcleo del sistema)

## 🟢 Feature 6.1 – Alta de Cargo

### User Stories

- Como secretario quiero crear un cargo con SARH
- Como secretario quiero asignar el cargo a una persona
- Como secretario quiero indicar tipo de cargo

### Reglas

- SARH único
- SARH inmutable
- Estado Activo/Inactivo
- Docente secundaria requiere comisión
- Jornada no requiere comisión

---

## 🟢 Feature 6.2 – Distribución Horaria

- Asignar módulos a cargo

### Validaciones

- No superposición horaria por persona
- Suma módulos = cantidad_horas
- No duplicar módulo

---

## 🟢 Feature 6.3 – Inactivación de Cargo

- Cambiar estado
- No permitir borrado físico
- Mantener historial

---

# 🔵 EPIC 7 – Validaciones Críticas de Dominio

- Validación de superposición horaria
- Validación de integridad referencial
- Constraint único SARH
- Validación comisión obligatoria según tipo
- Validación de módulos iguales a horas

---

# 🔵 EPIC 8 – Gestión de Artículos / Licencias

## 🟢 Feature 8.1 – CRUD Artículo

- Crear artículo
- Editar artículo
- Definir:
  - Genera reemplazo
  - Cantidad de días
  - Requisitos

---

# 🔵 EPIC 9 – Gestión de Ausencias

## 🟢 Feature 9.1 – Alta de Ausencia

- Crear ausencia asociada a cargo
- Seleccionar artículo
- Definir rango de fechas
- Validar que no exista superposición de ausencia activa

---

## 🟢 Feature 9.2 – Cadena de Reemplazos

- Permitir ausencia que referencie otra ausencia
- Permitir N niveles
- Consultar cadena completa (CTE recursiva)

---

# 🔵 EPIC 10 – Reemplazos

## 🟢 Feature 10.1 – Crear Reemplazo

- Crear cargo temporal
- Asociarlo a ausencia madre
- Definir fechas
- Validar consistencia con ausencia original

---

# 🔵 EPIC 11 – Historial y Trazabilidad

- Historial de estado de cargo
- Historial de distribución horaria
- Registro de cambios estructurales
- Auditoría básica de cambios críticos

---

# 🔵 EPIC 12 – Consultas Estratégicas

- Horario por persona
- Horario por comisión
- Cargos activos por escuela
- Ausencias activas
- Licencias por artículo
- Cadena completa de reemplazos
- Validación automática de conflictos

---

# 🔵 EPIC 13 – Seguridad

- Autenticación JWT
- Roles:
  - Admin
  - Secretaría
  - Directivo
  - Consulta
- Middleware de autorización
- Protección de rutas en frontend

---

# 🔵 EPIC 14 – Testing

- Tests unitarios de dominio
- Tests de aplicación
- Tests de integración (API)
- Tests de validación horaria

---

# 🔵 EPIC 15 – Observabilidad y Mantenibilidad

- Logs estructurados
- Manejo centralizado de errores
- Health check endpoint
- Versionado de API

---

# 🔵 EPIC 16 – Documentación Final

- Documentación API
- README técnico profesional
- Guía de despliegue
- Guía de migraciones
- Manual técnico de arquitectura

---

# 📊 Macro Roadmap Sugerido

## Sprint 0

EPIC 0 – Diseño y Modelado Inicial

## Sprint 1

EPIC 1 + EPIC 2

## Sprint 2

EPIC 3 + EPIC 4 + EPIC 5

## Sprint 3

EPIC 6 + EPIC 7

## Sprint 4

EPIC 8 + EPIC 9 + EPIC 10

## Sprint 5

EPIC 11 + EPIC 12

## Sprint 6

EPIC 13 + EPIC 14 + EPIC 15 + EPIC 16

---

# 🎯 Objetivo Estratégico

Construir un sistema institucional:

- Escalable
- Con trazabilidad completa
- Sin pérdida de historial
- Con reglas de negocio fuertes
- Preparado para crecer como SaaS futuro
