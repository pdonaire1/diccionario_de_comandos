Comandos Básicos de PostgreSQL


A continuación un ejemplo de comandos básicos para la administración de PostgreSQL. Siempre estará disponible el aplicativo Pgadmin, para una administración gráfica.

1)El primer comando nos enseñará como iniciar el cliente de psql en nuestra consola:

    psql -U user -W -h host database

2)Nuestro segundo comando nos ayudara a saber la lista de nuestras bases de datos, el comando es:

    \l
3)Seleccionar una base de datos o cambiar de base:

    \c basename
4)Listar tablas de una base de datos:

    \d
Si la lista es muy larga veremos que podemos movernos hacia abajo y luego para salir solo digitamos la letra “q”

5)Para ver la información de la estructura de una tabla en especifico:

    \d table
6)Vaciar una tabla en especifico o el famoso TRUNCATE que conocemos:

TRUNCATE TABLE table RESTART IDENTITY
Con este comando borramos el contenido de una tabla y reiniciamos su indice sino agregamos RESTART IDENTITY nuestros indices no seran reiniciados y seguiran según el ultimo registro.

7)Crear una base de datos:

CREATE DATABASE basename;
8)Borrar o eliminar una base de datos:

DROP DATABASE basename;
9)Borrar o eliminar una tabla en especifico:

DROP TABLE tablename;
10)Enviar resultados de una consulta a un archivo delimitado por |

COPY (SELECT * FROM tablename) TO '/home/tablename.csv' WITH DELIMITER '|';
Cabe mencionar que el archivo necesito permisos de escritura.

11)Uso de LIMIT y OFFSET

SELECT * FROM table LIMIT limit OFFSET offset;
Donde:
limit: es nuestro limite de registros a mostrar
offset: indica desde donde comenzaran a mostrarce los registros

12)Uso de comillas:

SELECT “column” FROM “table” WHERE “column” = 'value';
Generalmente podemos utilizar comillas dobles para nuestras columnas y comillas simples para nuestros valores, esto no es una regla pero a veces es necesario en casos especiales, tales como cuando ocupamos nombres reservados, por ejemplo:

SELECT to FROM table;
En este caso tenemos un campo llamado “to”, esto nos dará un error de sintaxis, por lo tanto tendremos que usar comillas dobles:

SELECT “to” FROM table;

13)Salir del cliente psql:

\q

Lista de Comandos:
------------------
1. Listar usuarios:

 postgres=# SELECT * FROM pg_user ;
2. Crear usuario BD:

 operador@equipo:/$ sudo createuser -s -U postgres nuevo_usuario

3. Cambiar contraseña:

 postgres=# ALTER USER postgres WITH PASSWORD '*****';
    ALTER ROLE
4. Renombrar usuario:

 postgres=# ALTER USER pedro RENAME TO admin;
    ALTER ROLE
5. Crear BD:

postgres=# CREATE DATABASE "nombre_bd"  WITH ENCODING='UTF8' OWNER=usuario CONNECTION LIMIT=-1;
6. Lista bases de datos del sistema:

 postgres=# SELECT datname FROM pg_database ;
7. Respaldo BD:

 operador@equipo:/$ sudo pg_dump -Uusuario -dnombre_bd -f /carpeta/destino/archivo.sql
8. Montar BD:

 operador@equipo:/$ sudo psql -Uusuario -dnombre_bd -f /carpeta/destino/archivo.sql
9. Dar todos los privilegios de una BD a un usuario:

 postgres=# GRANT ALL PRIVILEGES ON DATABASE nombre_bd to usuario;
10. Cambiar dueño de BD:

 postgres=# ALTER DATABASE nombre_bd OWNER TO usuario;
11. Respaldo BD remota:

 operador@equipo:/$ sudo pg_dump -O -Uusuario -p 5470 -h127.0.0.1 nmbre_bd > /carpeta/destino/archivo.sql
donde:
-O = –no-owner
-U = usuario que establece la conexión a la BD
-p = puerto de conexión BD
-h = host donde se encuentra la BD

12. Renombrar BD:

 postgres=# ALTER DATABASE nombre_anterior RENAME TO nuevo_nombre;
13. Retaurar backup base de datos

 operador@equipo:/$ sudo pg_restore -i -h localhost -p 5432 -U usuario -d base_de_datos -v "/ruta/archivo.backup"
 
 
 ---------------------------------------------------------------------------------
 Otra lista de comandos
 ---------------------------------------------------------------------------------
 1. Crear un Usuario.
[postgres@GNU][~]$ createuser luix
Clase_Maritima=> CREATE USER pilar with password ‘pilar';
2. Listando todos los usuarios
Clase_Maritima=> du
Clase_Maritima=> SELECT * FROM pg_user ;

3. Cambiando el Password de un Usuario.
Clase_Maritima=> ALTER USER pilar with password ‘123456’;

4. Cambiando el nombre de un usuario
Clase_Maritima=> ALTER USER pilar RENAME TO manolo;

5. Borrando Usuarios

[postgres@GNU][~]$ dropuser pilar

Clase_Maritima=>drop user pilar;

6. Crear una Base Datos
[postgres@GNU][~]$ createdb Maritima
Clase_Maritima=> CREATE DATABASE marimar;

7. Listando todas las Base Datos
Clase_Maritima=> l
Clase_Maritima=> SELECT datname FROM pg_database ;
[postgres@GNU][~/data]$ psql -l

8. Cambiando el nombre de una Base datos
Clase_Maritima=> ALTER DATABASE marimar RENAME TO Maritmar;

9. Borrando una Base Datos
postgres@GNU][~]$ dropdatadb Maritima
Clase_Maritima=>drop database Maritima;

10. Accesando a una Base Datos con un usuario.
[postgres@GNU][~]$ psql -U pilar -h localhost -d Maritima

11. Creando Tablas
CREATE TABLE Pollo (
Codigo char(5),
Nombre varchar(40),
Peso integer ,
Edad date,
Famila varchar(10)
);

12. Creando tabla desde un SELECT
Clase_Maritima=> create table Mar as SELECT * FROM pollo;

13. Listando las Tablas creadas
Clase_Maritima=>dt
Clase_Maritima=> SELECT * FROM pg_tables;

14. Viendo la Estructura de una Tabla
Clase_Maritima=>d pollo

15. Cambiando el nombre de una Tabla
Clase_Maritima=> ALTER TABLE pollo RENAME TO pollos;

16. Cambiando el nombre de un campo de una Tabla
Clase_Maritima=> ALTER TABLE pollos RENAME edad TO Fecha_Muerte;

17. Agregandole un campo a una tabla
Clase_Maritima=> ALTER TABLE pollos ADD column sex char(1);

18. Borrando un campo de una tabla
Clase_Maritima=> ALTER TABLE pollos DROP sex;

19. Cambiando el tipo de dato de una columna de una tabla.
Clase_Maritima=> ALTER TABLE pollos ALTER codigo TYPE varchar;

20. Borrando una Tabla
Clase_Maritima-> DROP TABLE pollo;

21. Insertando Datos en una Tabla
Clase_Maritima=> INSERT INTO pollo VALUES ( ‘1’, ‘Gallina’, 8, Current_date, ‘Criollo’);

22. Insertando datos a partir de un SELECT
Clase_Maritima=> INSERT INTO pollos (nombre, famila) SELECT bandera, codigo FROM buque ;

23. Selecionado datos de una tabla
Clase_Maritima=> SELECT * FROM pollo ;

24. Muestra el plan de ejecución de la sentencia
Clase_Maritima=# EXPLAIN SELECT * FROM buque ;

25. Para saber la cantidad de registro en una tabla (Count)
Clase_Maritima=# SELECT count(*) FROM buque ;

26. Selecionar los registros no repetidos de una campo (DISTINCT)
Clase_Maritima=# SELECT distinct(bandera) FROM buque ;

27. Actualizando datos de una tabla
Clase_Maritima=> UPDATE pollo SET nombre = ‘Gallo’ WHERE codigo=1;

28. Borrando registros de una tabla.
Clase_Maritima=> DELETE FROM pollo WHERE codigo =’1′;

29. Truncando tablas
Clase_Maritima=> TRUNCATE pollo ;

30. Agregando una llave primaria a un campo de una tabla
Clase_Maritima=> ALTER TABLE pollos ADD CONSTRAINT pk_codigo PRIMARY KEY (codigo);

31. Creando una Vista
Clase_Maritima=# CREATE VIEW v_pollo as SELECT * FROM pollos ;

32. Seleccionando datos de una Vista
Clase_Maritima=# SELECT * FROM v_pollo ;

33. Viendo las Vistas Creadas
Clase_Maritima=#dv
Clase_Maritima=# SELECT viewname FROM pg_views ;

34. Borrando una Vista
Clase_Maritima=# DROP VIEW v_pollo ;

35. Agreando una llave foraneas a un campo de una tabla
Clase_Maritima=> ALTER TABLE pollos ADD CONSTRAINT pk_codigo FOREIGN KEY (codigo) REFERENCES buque (codigo);

36. Borrando una un CONSTRAINT
Clase_Maritima=> ALTER TABLE pollos DROP CONSTRAINT pk_codigo;

37. Agregando un CONSTRAINT CHECK a un campo
Clase_Maritima=> ALTER TABLE pollos ADD CONSTRAINT c_check check (fecha_muerte > ‘2007-01-01′);

38. Agregando un CONSTRAINT DEFAULT a un campo
Clase_Maritima=> ALTER TABLE pollos ALTER peso SET DEFAULT 23;

39. Creando un índice a una tabla
Clase_Maritima=> CREATE INDEX pkU_pollo ON pollos (codigo);

40. Creando un indice unico
Clase_Maritima=> CREATE UNIQUE INDEX pku_pollo ON pollos (peso );

41. Cambiandole el nombre a un indice
Clase_Maritima=> ALTER INDEX pku_pollo RENAME TO pki_pollo;

42. Ver los indices creados en una Base Datos
Clase_Maritima=>di
Clase_Maritima=> SELECT indexname, tablename FROM pg_indexes;

43. Borrando un indice
Clase_Maritima=> DROP INDEX pku_pollo ;

44. Creando un sequence
Clase_Maritima=> CREATE SEQUENCE s_mari start with 1000 increment by 2 maxvalue 1100;

45. Ver el siguente valor de un sequence
Clase_Maritima=> SELECT nextval(‘s_mari’);

46. Ver el valor actual de un sequence
Clase_Maritima=> SELECT currval(‘s_mari’);

47. Modificar el valor inicial de un sequence
Clase_Maritima=> SELECT setval(‘s_mari’, 1000);

48. Utilizando INNER JOIN
Clase_Maritima=# SELECT * FROM files f Inner Join lineas l ON l.codigo=f.linea;

49. Utilizando LEFT OUTER JOIN
Clase_Maritima=# SELECT * FROM files f LEFT OUTER JOIN lineas l ON l.codigo=f.linea;

50. Utilizando RIGHT OUTER JOIN
Clase_Maritima=# SELECT * FROM files f RIGHT OUTER JOIN lineas l ON l.codigo=f.linea;

51. Utilizando FULL OUTER JOIN
Clase_Maritima=# SELECT * FROM files f FULL OUTER JOIN lineas l ON l.codigo=f.linea;

52. Utilizando LEFT OUTER JOIN
Clase_Maritima=# SELECT * FROM files f LEFT Join lineas l USING(Linea);

53. Utilizando operador Mayor que
Clase_Maritima=# SELECT buque, loa FROM buque WHERE loa > 1000;

54. Utilizando operador Menor que
Clase_Maritima=# SELECT buque, loa FROM buque WHERE loa < 1000;

55. Utilizando operador Igual
Clase_Maritima=# SELECT buque, loa FROM buque WHERE buque=’AIDA';

56. Utilizando operador Menor o igual que
Clase_Maritima=# SELECT buque, loa FROM buque WHERE loa <= 1000;

57. Utilizando operador Mayor o igual que
Clase_Maritima=# SELECT buque, loa FROM buque WHERE loa >= 1000;

58. Utilizando operador No igual
Clase_Maritima=# SELECT buque, loa FROM buque WHERE loa <> 1000;

Clase_Maritima=# SELECT buque, loa FROM buque WHERE loa != 1000;

59. Utilizando operador Concatenación
Clase_Maritima=# SELECT buque||’ ‘ ||dueno FROM buque ;

60. Utilizando EXISTS
SELECT * FROM boardingclerk WHERE exists(SELECT 1 FROM files);

61. Utilizando conector IN
SELECT * FROM files WHERE boarding_clerk IN (31, 33, 35);
SELECT * FROM files WHERE boarding_clerk NOT IN (31, 33, 35);

62. La cláusula ORDER BY
Clase_Maritima=# SELECT * FROM puertos ORDER BY 1 ASC;
Clase_Maritima=# SELECT codigo, puerto FROM puertos ORDER BY puerto DESC;

63. La cláusula GROUP BY
Clase_Maritima=# SELECT buque, count(*) FROM files GROUP BY buque;

64. Funciones para calcular
Clase_Maritima=# SELECT AVG(LOA) FROM BUQUE;
Clase_Maritima=# SELECT MAX(LOA) FROM BUQUE;
Clase_Maritima=# SELECT MIN(LOA) FROM BUQUE;
Clase_Maritima=# SELECT SUM(LOA) FROM BUQUE;

65. Operaciones de conjunto (UNION).
SELECT linea FROM files
union
SELECT codigo FROM lineas ;

66. Operaciones de conjunto (UNION ALL).
SELECT linea FROM files
union all
SELECT codigo FROM lineas ;

67. Operaciones de conjunto (INTERSECT).
SELECT linea FROM files
INTERSECT
SELECT codigo FROM lineas ;

68. Utilizando operadores aritméticos
FCLD=# SELECT 8+3 as Suma;
FCLD=# SELECT 8-3 as Resta;
FCLD=# SELECT 8/3 as Divide;
FCLD=# SELECT 8*3 as Multiplica;

69. Utilizando Funciones Matemáticas
FCLD=# SELECT 20-233 as Resta ; — El resultado Sera Negativo
FCLD=# SELECT abs(20-233) as Resta ; Esta Funcion
FCLD=# SELECT cbrt(27); — Retorna El cubo
FCLD=# SELECT round(99.4);
FCLD=# SELECT round(99.2, 3);
FCLD=# SELECT pi();
FCLD=# SELECT trunc(99.1);

70. Funciones Cadenas
FCLD=# SELECT ‘Jose’||’Paredes';
FCLD=# SELECT bit_length(‘k’) ;
FCLD=# SELECT char_length(‘jose’);
FCLD=# SELECT lower(‘GNU’);
FCLD=# SELECT upper(‘gnu’);
FCLD=# SELECT initcap(‘manuel’);
FCLD=# SELECT ascii(‘K’);
FCLD=# SELECT chr(75);
FCLD=# SELECT md5(‘1′);

71. Funciones Fechas y Horas
FCLD=# SELECT abstime(‘now'::timestamp); –convierte a abstime
FCLD=# SELECT age(‘now’,’1957-06-13′::timestamp); –preserva meses y años
FCLD=# SELECT to_char(current_timestamp,’HH12:MI:SS’); –convierte datetime a string
FCLD=# SELECT to_char( now(), ‘HH12:MI:SS’);
FCLD=# SELECT current_date;
FCLD=# SELECT current_timestamp;
Clase_Maritima=# SELECT to_date(fecha_llegada, ‘Mon MM YY’) FROM files ;
Clase_Maritima=# SELECT to_char(to_date(fecha_llegada, ‘Mon MM YY’), ‘YYYY-Month-Day’) FROM files ;
FCLD=# SELECT to_date(’08 Dec 2007 13′, ‘DD Mon YYYY HH’); –convierte string a date

72. Los conectores lógicos en SQL son AND-OR- NOT
Clase_Maritima=# SELECT buque, capitan, bandera, loa FROM buque WHERE capitan like ‘A%’ AND loa <1000 OR loa=2450;

73. Copiando datos desde un archivo a una tabla
COPY buque FROM ‘/var/lib/pgsql/Buquedatos.txt';
 
Fuentes:
- https://juantrucupei.wordpress.com/2015/03/24/comandos-basicos-de-postgresql/
- https://caronates.wordpress.com/2010/01/12/comandos-para-postgres/
