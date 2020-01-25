# SQL Injection en DVWA

Una **aproximación a la query** suele ser:

```SQL
SELECT * FROM <x> WHERE <y>=___ ;
```

o

```SQL
SELECT * FROM <x> WHERE <y> LIKE '%_____%';
```

## MySQL database

Una manera de conocer si la DB es **vulnerable**, es poniendo en la query un `'`. Si vemos que la DB devuelve algún error, eso puede ser indicio de que es vulnerable.

Es **importante** que, para que la query sea correcta sintácticamente, añadamos al principio un `'` y al final de la sentencia `--+`o `#` (para comentar el resto de la query en el backend).

Primero, es importante saber con cuantas **columnas** trabajamos o podemos trabajar. Para ello podemos jugar con la sentencia `ORDER BY`. 

Podemos conocer el **nombre y versión** de la DB añadiendo a la query

```SQL
SELECT database(), version()
```

Para obtener **[todas las tablas](https://dev.mysql.com/doc/refman/8.0/en/tables-table.html)** de la DB, se puede hacer añadiendo la consulta:

```SQL
SELECT table_name FROM information_schema.tables
```

Si queremos ver **todas las columnas de una tabla**, podemos hacerlo con la siguiente query:

```SQL
SELECT column_name FROM information_schema.columns
WHERE table_name=char(117,115,101,114,115)
```

En el ejemplo anterior, se intenta buscar todas las columnas de la tabla _users_, pero en la query no se puede poner tal cual _users_, si no que se ha de [pasar a decimal](https://cryptii.com/pipes/text-decimal). (_users en decimal es 117 115 101 114 115_)

Si deseamos obtener datos de alguna columna concreta (en este caso intentaremos obtener _user_ y _password_), deberemos añadir lo siguiente a la query:

```SQL
SELECT user, password FROM users
```

## Enlaces de interes
[SQL Injection Cheat Sheet](http://pentestmonkey.net/cheat-sheet/sql-injection/mysql-sql-injection-cheat-sheet)
