---
name: optimizacion-sql
description: 'Asistente universal para optimizar el rendimiento SQL: ajuste de consultas, estrategias de indices e analisis de rendimiento en todas las bases de datos SQL. Universal SQL performance optimization assistant for comprehensive query tuning, indexing strategies, and database performance analysis across all SQL databases (MySQL, PostgreSQL, SQL Server, Oracle). Provides execution plan analysis, pagination optimization, batch operations, and performance monitoring guidance.'
---

# Asistente de Optimización de Rendimiento SQL

Optimización experta de SQL para ${selection} (o el proyecto completo si no hay selección). Enfoque en técnicas universales de optimización SQL que funcionan en MySQL, PostgreSQL, SQL Server, Oracle y otras bases de datos SQL.

## 🎯 Áreas Principales de Optimización

### Análisis de Rendimiento de Consultas
```sql
-- ❌ BAD: Inefficient query patterns
SELECT * FROM orders o
WHERE YEAR(o.created_at) = 2024
  AND o.customer_id IN (
      SELECT c.id FROM customers c WHERE c.status = 'active'
  );

-- ✅ GOOD: Optimized query with proper indexing hints
SELECT o.id, o.customer_id, o.total_amount, o.created_at
FROM orders o
INNER JOIN customers c ON o.customer_id = c.id
WHERE o.created_at >= '2024-01-01' 
  AND o.created_at < '2025-01-01'
  AND c.status = 'active';

-- Required indexes:
-- CREATE INDEX idx_orders_created_at ON orders(created_at);
-- CREATE INDEX idx_customers_status ON customers(status);
-- CREATE INDEX idx_orders_customer_id ON orders(customer_id);
```

### Optimización de Estrategia de Índices
```sql
-- ❌ BAD: Poor indexing strategy
CREATE INDEX idx_user_data ON users(email, first_name, last_name, created_at);

-- ✅ GOOD: Optimized composite indexing
-- For queries filtering by email first, then sorting by created_at
CREATE INDEX idx_users_email_created ON users(email, created_at);

-- For full-text name searches
CREATE INDEX idx_users_name ON users(last_name, first_name);

-- For user status queries
CREATE INDEX idx_users_status_created ON users(status, created_at)
WHERE status IS NOT NULL;
```

### Optimización de Subconsultas
```sql
-- ❌ BAD: Correlated subquery
SELECT p.product_name, p.price
FROM products p
WHERE p.price > (
    SELECT AVG(price) 
    FROM products p2 
    WHERE p2.category_id = p.category_id
);

-- ✅ GOOD: Window function approach
SELECT product_name, price
FROM (
    SELECT product_name, price,
           AVG(price) OVER (PARTITION BY category_id) as avg_category_price
    FROM products
) ranked
WHERE price > avg_category_price;
```

## 📊 Técnicas de Ajuste de Rendimiento

### Optimización de JOIN
```sql
-- ❌ BAD: Inefficient JOIN order and conditions
SELECT o.*, c.name, p.product_name
FROM orders o
LEFT JOIN customers c ON o.customer_id = c.id
LEFT JOIN order_items oi ON o.id = oi.order_id
LEFT JOIN products p ON oi.product_id = p.id
WHERE o.created_at > '2024-01-01'
  AND c.status = 'active';

-- ✅ GOOD: Optimized JOIN with filtering
SELECT o.id, o.total_amount, c.name, p.product_name
FROM orders o
INNER JOIN customers c ON o.customer_id = c.id AND c.status = 'active'
INNER JOIN order_items oi ON o.id = oi.order_id
INNER JOIN products p ON oi.product_id = p.id
WHERE o.created_at > '2024-01-01';
```

### Optimización de Paginación
```sql
-- ❌ BAD: OFFSET-based pagination (slow for large offsets)
SELECT * FROM products 
ORDER BY created_at DESC 
LIMIT 20 OFFSET 10000;

-- ✅ GOOD: Cursor-based pagination
SELECT * FROM products 
WHERE created_at < '2024-06-15 10:30:00'
ORDER BY created_at DESC 
LIMIT 20;

-- Or using ID-based cursor
SELECT * FROM products 
WHERE id > 1000
ORDER BY id 
LIMIT 20;
```

### Optimización de Agregaciones
```sql
-- ❌ BAD: Multiple separate aggregation queries
SELECT COUNT(*) FROM orders WHERE status = 'pending';
SELECT COUNT(*) FROM orders WHERE status = 'shipped';
SELECT COUNT(*) FROM orders WHERE status = 'delivered';

-- ✅ GOOD: Single query with conditional aggregation
SELECT 
    COUNT(CASE WHEN status = 'pending' THEN 1 END) as pending_count,
    COUNT(CASE WHEN status = 'shipped' THEN 1 END) as shipped_count,
    COUNT(CASE WHEN status = 'delivered' THEN 1 END) as delivered_count
FROM orders;
```

## 🔍 Antipatrones de Consultas

### Problemas de Rendimiento con SELECT
```sql
-- ❌ BAD: SELECT * anti-pattern
SELECT * FROM large_table lt
JOIN another_table at ON lt.id = at.ref_id;

-- ✅ GOOD: Explicit column selection
SELECT lt.id, lt.name, at.value
FROM large_table lt
JOIN another_table at ON lt.id = at.ref_id;
```

### Optimización de Cláusulas WHERE
```sql
-- ❌ BAD: Function calls in WHERE clause
SELECT * FROM orders 
WHERE UPPER(customer_email) = 'JOHN@EXAMPLE.COM';

-- ✅ GOOD: Index-friendly WHERE clause
SELECT * FROM orders 
WHERE customer_email = 'john@example.com';
-- Consider: CREATE INDEX idx_orders_email ON orders(LOWER(customer_email));
```

### Optimización de OR vs UNION
```sql
-- ❌ BAD: Complex OR conditions
SELECT * FROM products 
WHERE (category = 'electronics' AND price < 1000)
   OR (category = 'books' AND price < 50);

-- ✅ GOOD: UNION approach for better optimization
SELECT * FROM products WHERE category = 'electronics' AND price < 1000
UNION ALL
SELECT * FROM products WHERE category = 'books' AND price < 50;
```

## 📈 Optimización Independiente de la Base de Datos

### Operaciones en Lote (Batch)
```sql
-- ❌ BAD: Row-by-row operations
INSERT INTO products (name, price) VALUES ('Product 1', 10.00);
INSERT INTO products (name, price) VALUES ('Product 2', 15.00);
INSERT INTO products (name, price) VALUES ('Product 3', 20.00);

-- ✅ GOOD: Batch insert
INSERT INTO products (name, price) VALUES 
('Product 1', 10.00),
('Product 2', 15.00),
('Product 3', 20.00);
```

### Uso de Tablas Temporales
```sql
-- ✅ GOOD: Using temporary tables for complex operations
CREATE TEMPORARY TABLE temp_calculations AS
SELECT customer_id, 
       SUM(total_amount) as total_spent,
       COUNT(*) as order_count
FROM orders 
WHERE created_at >= '2024-01-01'
GROUP BY customer_id;

-- Use the temp table for further calculations
SELECT c.name, tc.total_spent, tc.order_count
FROM temp_calculations tc
JOIN customers c ON tc.customer_id = c.id
WHERE tc.total_spent > 1000;
```

## 🛠️ Gestión de Índices

### Principios de Diseño de Índices
```sql
-- ✅ GOOD: Covering index design
CREATE INDEX idx_orders_covering 
ON orders(customer_id, created_at) 
INCLUDE (total_amount, status);  -- SQL Server syntax
-- Or: CREATE INDEX idx_orders_covering ON orders(customer_id, created_at, total_amount, status); -- Other databases
```

### Estrategia de Índices Parciales
```sql
-- ✅ GOOD: Partial indexes for specific conditions
CREATE INDEX idx_orders_active 
ON orders(created_at) 
WHERE status IN ('pending', 'processing');
```

## 📊 Consultas de Monitoreo de Rendimiento

### Análisis de Rendimiento de Consultas
```sql
-- Generic approach to identify slow queries
-- (Specific syntax varies by database)

-- For MySQL:
SELECT query_time, lock_time, rows_sent, rows_examined, sql_text
FROM mysql.slow_log
ORDER BY query_time DESC;

-- For PostgreSQL:
SELECT query, calls, total_time, mean_time
FROM pg_stat_statements
ORDER BY total_time DESC;

-- For SQL Server:
SELECT 
    qs.total_elapsed_time/qs.execution_count as avg_elapsed_time,
    qs.execution_count,
    SUBSTRING(qt.text, (qs.statement_start_offset/2)+1,
        ((CASE qs.statement_end_offset WHEN -1 THEN DATALENGTH(qt.text)
        ELSE qs.statement_end_offset END - qs.statement_start_offset)/2)+1) as query_text
FROM sys.dm_exec_query_stats qs
CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) qt
ORDER BY avg_elapsed_time DESC;
```

## 🎯 Lista de Verificación Universal de Optimización

### Estructura de Consultas
- [ ] Evitar SELECT * en consultas de producción
- [ ] Usar tipos de JOIN adecuados (INNER vs LEFT/RIGHT)
- [ ] Filtrar pronto en las cláusulas WHERE
- [ ] Usar EXISTS en lugar de IN para subconsultas cuando sea apropiado
- [ ] Evitar funciones en cláusulas WHERE que impidan el uso de índices

### Estrategia de Índices
- [ ] Crear índices en columnas consultadas con frecuencia
- [ ] Usar índices compuestos en el orden correcto de columnas
- [ ] Evitar el exceso de índices (afecta el rendimiento de INSERT/UPDATE)
- [ ] Usar covering indexes cuando sea beneficioso
- [ ] Crear índices parciales para patrones de consulta específicos

### Tipos de Datos y Esquema
- [ ] Usar tipos de datos adecuados para eficiencia de almacenamiento
- [ ] Normalizar de forma apropiada (3NF para OLTP, desnormalizado para OLAP)
- [ ] Usar constraints para ayudar al optimizador de consultas
- [ ] Particionar tablas grandes cuando sea apropiado

### Patrones de Consulta
- [ ] Usar LIMIT/TOP para controlar el tamaño del resultado
- [ ] Implementar estrategias de paginación eficientes
- [ ] Usar operaciones en lote (batch) para cambios masivos de datos
- [ ] Evitar problemas de consultas N+1
- [ ] Usar prepared statements para consultas repetidas

### Pruebas de Rendimiento
- [ ] Probar consultas con volúmenes de datos realistas
- [ ] Analizar los planes de ejecución de las consultas
- [ ] Monitorear el rendimiento de las consultas en el tiempo
- [ ] Configurar alertas para consultas lentas
- [ ] Analizar regularmente el uso de índices

## 📝 Metodología de Optimización

1. **Identifica**: usa herramientas específicas de cada base de datos para encontrar consultas lentas
2. **Analiza**: examina los planes de ejecución e identifica cuellos de botella
3. **Optimiza**: aplica las técnicas de optimización adecuadas
4. **Prueba**: verifica las mejoras de rendimiento
5. **Monitorea**: sigue las métricas de rendimiento de forma continua
6. **Itera**: revisa y optimiza el rendimiento de forma regular

Enfócate en mejoras de rendimiento medibles y prueba siempre las optimizaciones con volúmenes de datos y patrones de consulta realistas.
