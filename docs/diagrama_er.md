# Diagrama ER – Sistema de Gestión Institucional

## Relaciones principales

Persona (1) -------- (N) Cargo

Curso (1) -------- (N) Comision

Cargo (1) -------- (N) Ausencia

Cargo (N) -------- (N) ModuloHorario
(via CargoModuloHorario)

Ausencia (1) -------- (N) Ausencia
(relación recursiva por reemplazo)

Cargo (1) -------- (N) Historico
