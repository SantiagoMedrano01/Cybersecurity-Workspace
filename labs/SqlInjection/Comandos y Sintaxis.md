
Estos son los bloques de construcción para inyectar payloads.

|**Concepto**|**Sintaxis / Ejemplo**|**Descripción**|
|---|---|---|
|**Comentarios (MySQL)**|`--` o `#` (línea), `/* */` (multilínea)|Cortan el resto de la consulta original para evitar errores de sintaxis.|
|**UNION**|`UNION SELECT 1, 2, 3`|Combina resultados de varios `SELECT`. **Regla:** Deben tener la misma cantidad de columnas.|
|**LIKE & Comodines**|`LIKE 'adm%'`, `LIKE 'a_'`|Búsqueda de patrones. `%` coincide con cualquier secuencia; `_` coincide con un carácter exacto.|
|**LIMIT**|`LIMIT offset, count` (ej: `LIMIT 2, 1`)|Limita o salta filas en la salida.|
|**Funciones de Cadena**|`group_concat(col1, col2)`|Agrupa valores de múltiples filas en una sola cadena separada por comas.|
|**Concatenación**|`CONCAT(col1, ':', col2)`|Une valores individuales en una sola fila.|

### Metadatos: `information_schema`

Base de datos integrada con el "mapa" del servidor.

- **Tablas:** `information_schema.tables` (columnas clave: `table_schema`, `table_name`).
    
- **Columnas:** `information_schema.columns` (columnas clave: `table_name`, `column_name`).
    

## Tipos de SQL Injection (SQLi)

Ocurre cuando el input del usuario altera la lógica de la consulta sin sanitización.

### 1. In-Band (En Banda)

Los resultados se ven directamente en la respuesta de la web.

- **Error-Based:** Se extrae información leyendo los errores de base de datos que la aplicación filtra en pantalla.
    
- **Union-Based:** Usa `UNION` para añadir consultas y extraer datos en la página.
    
    - _Pasos:_ 1) Hallar cantidad de columnas (con errores al probar UNION). 2) Ver qué columna se renderiza. 3) Extraer DB (`database()`). 4) Extraer tablas. 5) Extraer columnas. 6) Extraer datos.
        

### 2. Blind (Ciega)

La aplicación no muestra resultados ni errores. Se infiere información por el comportamiento.

- **Authentication Bypass:** Logra que la consulta devuelva _al menos una fila_ (ej. `admin' OR 1=1;--`) saltando el control de contraseña.
    
- **Boolean-Based:** Se hacen preguntas "sí/no" a la BD. Se infieren datos letra por letra (usando `LIKE`) observando cambios sutiles en la respuesta HTTP o en la página (ej. un contenido distinto si es "true" o "false").
    
- **Time-Based:** No hay cambios visuales. Se usa `SLEEP()` para pausar la respuesta si la condición es verdadera. Demora = Verdadero.
    

### 3. Out-of-Band (OOB)

Se usa cuando las anteriores fallan y el servidor de base de datos tiene salida a la red.

- **Mecanismo:** Fuerza a la BD a enviar datos a un servidor del atacante vía peticiones externas (DNS o HTTP).
    
- **Ejemplo (MySQL):** Usar `LOAD_FILE()` para resolver un subdominio DNS que contenga los datos extraídos.
    

## 🛡️ Defensas (Remediación)

- **Sentencias Preparadas (Consultas Parametrizadas):** Es la solución principal. Separan la estructura del código del input del usuario, tratándolo siempre como datos literales.
    
- **Validación de Entrada (Allowlisting):** Definir exactamente qué formato es válido antes de llegar a la BD (usar junto con parametrización).
    
- **Principio de Menor Privilegio:** Limitar los permisos del usuario de BD al mínimo necesario (solo lectura, acceso restringido a tablas sensibles).