# Diagrama de Flujo – Alta de Ausencia

Inicio
↓
Secretaría carga ausencia
↓
¿Cargo existe?
├─ No → Error
└─ Sí →
↓
Validar fechas
↓
¿Superposición con otra ausencia?
├─ Sí → Error
└─ No →
↓
Consultar Artículo
↓
¿Genera reemplazo?
├─ No → Guardar ausencia → Fin
└─ Sí →
↓
Crear nueva Ausencia
↓
Asignar docente reemplazante
↓
Crear Cargo temporal
↓
Fin
