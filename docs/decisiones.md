# Decisiones Arquitectónicas – v1

## 1. SARH inmutable

Se decidió que el SARH no se edita.
Motivo: representa identidad administrativa oficial.

---

## 2. Historial mediante estado

Se evita borrar datos.
Se utiliza estado Activo/Inactivo para mantener trazabilidad.

---

## 3. Distribución horaria mediante tabla intermedia

Se utiliza CargoModuloHorario para permitir:

- Validaciones
- Consultas eficientes
- Historial implícito

---

## 4. Reemplazos modelados mediante referencia en Ausencia

Se permite cadena de reemplazos.
Se evita estructura rígida limitada a 2 niveles.

---

## 5. Validación de superposición por Persona

Se valida en capa de aplicación.
No se delega exclusivamente a base de datos.
