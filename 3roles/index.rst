Roles
*****

Definición de roles
--------------------------

Un rol es un conjunto de privilegios que van a ser asignador a un usuarios de mysql.

Por ejemplo podemo disponer de de tres tipos de roles para un pequeño proyecto.
Administrador , es quien podra efectuar cualquier accion sobre cualquier base de datos.
desarrollador, este perfil podrá realizar consultar, insertar, actualizar, eliminar, ...  alguna tablas de la base da datos.
consultor, este usuario es limitido, ya que solo podrá realizar consultas a la tablas y base de datos que el admin considere oportuno.

.. warning::
	
	*IMPLEMENTACION EN FASE BETA EN MYSQL 8*

Creacion de roles
-------------------------

Sintaxis de la sentencia *CREATE ROLE*::
	
	CREATE ROLE [IF NOT EXISTS] role [, role ] ...

Para nuestro proyecto crear los roles seria de esta forma::

	CREATE ROLE "admin", "desarrollador", "consultor";

Asignar permisos a los roles
----------------------------------------

La sentencia *GRANT* es la que permite asignar privilegios a los roles, aplicar la sintaxis es similar a la asignacion de privilegios sobre los usuarios::

	GRANT
	priv_type [(column_list)]
	[, priv_type [(column_list)]] ...
	ON [object_type] priv_level
	TO user [auth_option] [, user [auth_option]] ...
	[REQUIRE {NONE | tls_option [[AND] tls_option] ...}]
	[WITH {GRANT OPTION | resource_option} ...]


Nuesto rol de administrados tendrá todos los permisos sobre la base de datos, o sobre todas las bases de datos::

	GRANT ALL ON *.* TO 'admin';
	GRANT ALL ON base_da_datos.* TO 'admin';

El rol de consultor sólo podrá realizar consultas, y estarán limitadas::

	GRANT SELECT ON app.* TO 'consultor' WITH QUERIES_PER_HOUR 30;

El rol para el desarrollador::
	
	GRANT ALTER, DELETE, INSERT, SELECT, UPDATE, CREATE VIEW ON app.* TO 'desarrollador';

Asignación de roles a usuarios
-------------------------------------------

La asignación del rol al usuarios es muy simple::
	
	GRANT name_rol TO usuario;

Ejemplos para nuestros usuarios::
	
	GRANT 'admin' TO 'admin'@'localhost';

	GRANT 'desarrollador' TO 'desa1'@'localhost', 'desa2'@'host2';

	GRANT 'consultor' TO 'consultor'@'65.48.45.125';

Desasignación de roles a usuarios
------------------------------------------------

La sentencia para la designación de roles usamos REVOKE ::
	
	REVOKE 'role1', 'role2' FROM 'user1'@'localhost', 'user2'@'localhost';

Si necesitamos que el usuarios desa2 pase a usar el rol 'consultor' vamos a escribir la siguiente sentencia::

	REVOKE 'desarrollador' TO 'desa2'@'localhost',
	GRANT 'consultor' TO 'desa2'@'localhost';
	O
	ALTER USER 'desa2'@'localhost' DEFAULT ROLE consultor;


.. toctree::