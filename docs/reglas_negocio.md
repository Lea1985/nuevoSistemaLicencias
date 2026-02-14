# Reglas de Negocio – Sistema de Gestión

## 1. Sobre Personas

- Una persona puede tener múltiples cargos.
- Puede trabajar en distintos niveles.
- Puede ser directivo y docente.
- No puede tener superposición horaria efectiva.

---

## 2. Sobre Cargos

- El SARH es único e inmutable.
- Si cambia SARH → nuevo cargo.
- El cargo puede estar Activo o Inactivo.
- Docentes de secundaria deben pertenecer a una comisión.
- Cargos de jornada no requieren comisión.
- Si se redistribuyen módulos estructuralmente → nuevo cargo.

---

## 3. Sobre Horarios

- Cada módulo dura 40 minutos.
- La distribución semanal es fija.
- La suma de módulos debe coincidir con horas del cargo.
- Educación Física puede dictarse en contra turno.
- Debe validarse superposición por persona.

---

## 4. Sobre Ausencias

- Una ausencia siempre afecta a un cargo.
- Puede generar un reemplazo.
- El reemplazo puede ausentarse.
- Se permite cadena de hasta N niveles.
- No se elimina historial.

---

## 5. Sobre Reemplazos

- Se debe poder identificar qué ausencia generó cuál.
- Se mantiene trazabilidad completa.
- Lo habitual: máximo 2 niveles.
- El sistema debe permitir más.

---

## 6. Restricciones

- No se puede editar el SARH.
- No se puede borrar cargos con historial.
- No se puede asignar horario si genera superposición.
