# 📘 Reglas de Negocio – Sistema de Gestión

## Enfoque Operativo Flexible

---

## 1. Sobre Persona

- Una persona puede tener múltiples cargos.
- Puede desempeñarse en distintos niveles y modalidades.
- Puede ejercer simultáneamente distintos cargos.
- El sistema debe validar posibles superposiciones horarias efectivas entre cargos activos de una misma persona.
- En caso de superposición, el sistema debe advertir al usuario, permitiendo continuar bajo responsabilidad operativa.
- Puede existir una persona sin cargo.
- Un cargo puede existir sin ocupación activa.
- La relación entre persona y cargo se materializa exclusivamente mediante una Designación.
- Una persona puede ser creada para ser asignada como reemplazo o para cargar ausencias en un SARH interno hasta que llgue el numero definitivo.

---

## 2. Sobre Cargos

- El SARH (identificador estructural del cargo) es único por institución.
- El SARH es inmutable una vez creado.
- Si cambia el SARH o la estructura normativa del cargo → debe crearse un nuevo cargo.
- El cargo puede estar Activo o Inactivo.
- Los cargos de jornada completa u otros tipos estructurales pueden no requerir comisión.
- La redistribución horaria estructural se gestiona mediante versionado de vigencias horarias.
- Solo cambios estructurales normativos implican creación de un nuevo cargo.

---

## 3. Sobre Horarios

- Cada módulo tiene una duración configurable por institución (40 minutos por defecto).
- La distribución semanal puede versionarse mediante vigencias.
- El sistema debe validar que la suma de módulos coincida con las horas del cargo y advertir en caso de inconsistencias.
- Se permite dictado en contra turno cuando la estructura lo habilite.
- El sistema debe advertir posibles superposiciones horarias por persona, sin bloquear necesariamente la operación.

---

## 4. Sobre Ausencias

- Una ausencia siempre se registra sobre una designación activa.
- Puede generar una designación de reemplazo.
- El reemplazo puede a su vez registrar ausencias.
- Se permite encadenamiento de reemplazos sin límite estructural.
- El historial de ausencias y reemplazos es inmutable.
- No se eliminan registros históricos; solo pueden inactivarse según reglas administrativas.

---

## 5. Sobre Reemplazos

- Debe mantenerse trazabilidad completa entre ausencia original y reemplazos derivados.
- El sistema debe permitir identificar la cadena completa de reemplazos.
- Aunque operativamente lo habitual es hasta 2 niveles, el modelo permite N niveles.
- La trazabilidad no puede perderse por modificaciones posteriores.

---

## 6. Restricciones Operativas

- No se permite editar el SARH de un cargo existente.
- No se permite eliminar cargos que posean historial de designaciones.
- Las validaciones críticas (superposición, inconsistencias horarias, duplicidades) generan advertencias obligatorias antes de confirmar la operación.
- El sistema prioriza continuidad operativa institucional sobre bloqueo rígido de operaciones.
