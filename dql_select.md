## Consultas SQL

1. Encuentra el cliente que ha realizado la mayor cantidad de alquileres en los últimos 6 meses.
```sql
SELECT
    cl.nombre AS cliente,
    COUNT(al.id_alquiler) AS Cant_Alquileres
FROM cliente cl
JOIN alquiler al ON al.id_cliente = cl.id_cliente
WHERE al.fecha_alquiler >=DATE_SUB(CURDATE(), INTERVAL 6 MONTH)
GROUP BY cl.id_cliente
ORDER BY Cant_Alquileres DESC
LIMIT 1;
```
2. Lista las cinco películas más alquiladas durante el último año.
```sql
SELECT 
    p.titulo AS peliculas,
    COUNT(a.id_alquiler) AS total_alquileres
FROM alquiler a
JOIN inventario i ON a.id_inventario = i.id_inventario
JOIN pelicula p ON i.id_pelicula = p.id_pelicula
WHERE a.fecha_alquiler >= DATE_SUB(CURDATE(), INTERVAL 1 YEAR)
GROUP BY p.id_pelicula
ORDER BY total_alquileres DESC
LIMIT 5;
```

3. Obtén el total de ingresos y la cantidad de alquileres realizados por cada categoría de película.
```sql
SELECT 
    cat.nombre AS categoria,
    COUNT(a.id_alquiler) AS total_alquileres,
    SUM(pa.total) AS ingresos
FROM alquiler a
JOIN pago pa ON a.id_alquiler = pa.id_alquiler
JOIN inventario i ON a.id_inventario = i.id_inventario
JOIN pelicula p ON i.id_pelicula = p.id_pelicula
JOIN pelicula_categoria pc ON p.id_pelicula = pc.id_pelicula
JOIN categoria cat ON pc.id_categoria = cat.id_categoria
GROUP BY cat.id_categoria;
```

4. Calcula el número total de clientes que han realizado alquileres por cada idioma disponible en un mes específico.
```sql
SELECT 
    i.nombre AS idioma,
    COUNT(DISTINCT c.id_cliente) AS total_clientes
FROM cliente c
JOIN alquiler a ON c.id_cliente = a.id_cliente
JOIN inventario inv ON a.id_inventario = inv.id_inventario
JOIN pelicula p ON inv.id_pelicula = p.id_pelicula
JOIN idioma i ON p.id_idioma = i.id_idioma
WHERE MONTH(a.fecha_alquiler) = 8 AND YEAR(a.fecha_alquiler) = 2006
GROUP BY i.id_idioma;
```

5. Encuentra a los clientes que han alquilado todas las películas de una misma categoría.
```sql
SELECT 
    c.nombre AS cliente,
    cat.nombre AS categoria
FROM cliente c
JOIN alquiler a ON c.id_cliente = a.id_cliente
JOIN inventario i ON a.id_inventario = i.id_inventario
JOIN pelicula p ON i.id_pelicula = p.id_pelicula
JOIN pelicula_categoria pc ON p.id_pelicula = pc.id_pelicula
JOIN categoria cat ON pc.id_categoria = cat.id_categoria
GROUP BY c.id_cliente, pc.id_categoria
HAVING 
    COUNT(DISTINCT p.id_pelicula) = (
        SELECT COUNT(*) 
        FROM pelicula_categoria 
        WHERE id_categoria = pc.id_categoria
    );
```

6. Lista las tres ciudades con más clientes activos en el último trimestre.
```sql
SELECT 
    ciu.nombre AS ciudad
FROM cliente c
JOIN direccion d ON c.id_direccion = d.id_direccion
JOIN ciudad ciu ON d.id_ciudad = ciu.id_ciudad
JOIN alquiler a ON c.id_cliente = a.id_cliente
WHERE a.fecha_alquiler >= DATE_SUB(CURDATE(), INTERVAL 3 MONTH)
GROUP BY ciu.id_ciudad
ORDER BY total_clientes DESC
LIMIT 3;
```

7. Muestra las cinco categorías con menos alquileres registrados en el último año.
```sql
SELECT 
    cat.nombre AS categoria
FROM alquiler a
JOIN inventario i ON a.id_inventario = i.id_inventario
JOIN pelicula p ON i.id_pelicula = p.id_pelicula
JOIN pelicula_categoria pc ON p.id_pelicula = pc.id_pelicula
JOIN categoria cat ON pc.id_categoria = cat.id_categoria
WHERE a.fecha_alquiler >= DATE_SUB(CURDATE(), INTERVAL 1 YEAR)
GROUP BY cat.id_categoria
ORDER BY total_alquileres ASC
LIMIT 5;
```

8. Calcula el promedio de días que un cliente tarda en devolver las películas alquiladas.
```sql
SELECT
    AVG(DATEDIFF(a.fecha_devolucion, a.fecha_alquiler)) AS promedio_dias
FROM alquiler a;
```

9. Encuentra los cinco empleados que gestionaron más alquileres en la categoría de Acción.
```sql
SELECT
    em.nombre
FROM empleado em
```

10. Genera un informe de los clientes con alquileres más recurrentes.
```sql
```

11. Calcula el costo promedio de alquiler por idioma de las películas.
```sql
```

12. Lista las cinco películas con mayor duración alquiladas en el último año.
```sql
SELECT
    pel.titulo
FROM pelicula
LIMIT 5
```

13. Muestra los clientes que más alquilaron películas de Comedia.
```sql
```

14. Encuentra la cantidad total de días alquilados por cada cliente en el último mes.
```sql
```

15. Muestra el número de alquileres diarios en cada almacén durante el último trimestre.
```sql
```

16. Calcula los ingresos totales generados por cada almacén en el último semestre.
```sql
```

17. Encuentra el cliente que ha realizado el alquiler más caro en el último año.
```sql
SELECT
    cl.nombre AS cliente,
    p.total AS Total_precio
FROM pago p
JOIN cliente c ON p.id_cliente = c.id_cliente
WHERE p.fecha_pago >= DATE_SUB(CURDATE(), INTERVAL 1 YEAR)
ORDER BY p.total DESC
LIMIT 1;
```

18. Lista las cinco categorías con más ingresos generados durante los últimos tres meses.
```sql
SELECT
    cat.nombre AS categoria
LIMIT 5
```

19. Obtén la cantidad de películas alquiladas por cada idioma en el último mes.
```sql
```

20. Lista los clientes que no han realizado ningún alquiler en el último año.
```sql
SELECT
    cl.cliente AS cliente
```