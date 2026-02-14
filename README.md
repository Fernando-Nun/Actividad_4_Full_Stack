# 📦 Backend - Endpoint de búsqueda avanzada de productos

## 📖 Introducción
Este proyecto extiende el backend existente agregando un **endpoint avanzado de búsqueda de productos** (`GET /productos/search`).  
El objetivo es implementar consultas dinámicas y seguras en la base de datos, permitiendo:

- 🔍 Búsqueda por nombre (ILIKE)
- 📊 Filtro por rango de precio
- 📄 Paginación real con `LIMIT` y `OFFSET`
- 🛡️ Prevención de SQL Injection mediante parámetros seguros

---

## 🛠️ Construcción de la query dinámica
La consulta se construye de forma incremental:

1. Se parte de una base común:
   ```sql
   SELECT id, nombre, precio FROM productos WHERE activo = true
   ```
2. Se agregan condiciones opcionales según los parámetros recibidos:
- `nombre` → `AND nombre ILIKE $n`
- `minPrecio` → `AND precio >= $n`
- `maxPrecio` → `AND precio <= $n`
3. Se aplica ordenamiento:
   ```sql
   ORDER BY id DESC
   ```
4. Se añade paginación real:
   ```sql
   LIMIT $n OFFSET $n
   ```
5. Se ejecuta una segunda consulta con COUNT(*) para obtener el total de coincidencias.

##

# 🛡️ Prevención de SQL Injection
- Se usan placeholders ($1, $2, $3, …) en lugar de concatenar valores directamente.  
- Los valores reales se pasan en un arreglo (`params`) que PostgreSQL interpreta de forma segura.  
- Se validan los parámetros `page` y `limit` para asegurar que sean números positivos.  
- Nunca se insertan valores del usuario directamente en la query.  

---

# 📄 Conclusión
El endpoint `/productos/search` cumple con todos los requisitos de la actividad:

- Construcción de query dinámica segura.  
- Funcionalidad completa de filtros y combinaciones.  
- Paginación real con `LIMIT` y `OFFSET`.  
- Separación clara de responsabilidades en la arquitectura.  
- Evidencias documentadas en Postman.  

Este trabajo demuestra comprensión en SQL, seguridad y buenas prácticas de arquitectura backend.

---

# 📸 Pruebas en Postman

[Ver documentación en Postman](https://documenter.getpostman.com/view/51906927/2sBXcBnhaP)
