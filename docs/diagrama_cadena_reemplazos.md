---

# 📁 diagrama_cadena_reemplazos.md

```md
# Diagrama – Cadena de Reemplazos

Modelo jerárquico recursivo:

Ausencia 1 (Titular)
    │
    ▼
Ausencia 2 (Reemplazo 1)
    │
    ▼
Ausencia 3 (Reemplazo 2)
    │
    ▼
Ausencia 4 (Reemplazo 3)

Estructura en base de datos:

id | reemplaza_ausencia_id
---

---

1 | null
2 | 1
3 | 2
4 | 3

Esto permite:

- Cadenas de reemplazo de N niveles
- Consultas recursivas con CTE
- Trazabilidad completa
