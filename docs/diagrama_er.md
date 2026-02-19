# Diagrama ER – Sistema de Gestión Institucional (Alineado v4)

## Relaciones principales

- **Persona (1) — (N) Designación**  
  Cada persona puede tener múltiples designaciones a lo largo del tiempo.

- **Designación (1) — (1) Cargo**  
  Cada designación ocupa un cargo específico.

- **Designación (1) — (N) Ausencia**  
  Las ausencias se registran sobre la designación, no directamente sobre el cargo.

- **Curso (1) — (N) Comision**  
  Cada curso puede tener varias comisiones.

- **Cargo (N) — (N) ModuloHorario**  
  Vía **CargoModuloHorario**, que permite versionado y control de superposición.

- **Ausencia (1) — (N) Ausencia**  
  Relación recursiva por reemplazo (`reemplaza_ausencia_id`).

- **Cargo (1) — (N) CargoModuloHorario**  
  Permite mantener histórico de asignación de módulos y vigencias.

---

### Notas de alineación

1. Se incorpora **Designación** como intermediario entre Persona y Cargo.
2. `Ausencia` depende de **Designación**, respetando trazabilidad y reglas de superposición.
3. `CargoModuloHorario` reemplaza “Historico”, para reflejar versionado de horarios.
4. Relaciones como `Curso → Comision`, `Cargo → ModuloHorario` y `Ausencia → Ausencia` permanecen iguales.
