Usuarios
********

Los usuarios son aquellas entidades que registramos en la base de datos, ellos tienen acceso a la misma mediante los privilegios que determina el administrados.
Estos usuarios pueden ser finales o normales, especializado, programadores y administradores. Es recomendable tener una estrucutra de los privilegios que vamos a otorgar a los usuarios de la base de datos.

Creación de usuarios
------------------------------

Para la creación de un usuario hacemos uso la orden *CREATE USER* que permite añadir multiples opciones:::

	CREATE USER [IF NOT EXISTS]
	    user [auth_option] [, user [auth_option]] ...
	    [REQUIRE {NONE | tls_option [[AND] tls_option] ...}]
	    [WITH resource_option [resource_option] ...]
	    [password_option | lock_option] ...

Desgranando la sintaxis:

 	* ``user`` :  Indicamos el nombre del usuarios y desde donde se va a conectar, *'nombre_usuario'@'host'*.
 	* ``auth_option`` : Pemite asignar una contraseña para el usuario, podemos hacer uso de 
 		- *IDENTIFIED BY* 'auth_string'		Password en texto plano.
		- *IDENTIFIED BY PASSWORD* 'hash_string'	El hash del password(RECOMENDABLE)
	* ``resource_option``:  Restricciones sobre las acciones que puede realizar por ejemplo el numero queries maximas por horas, actualizaciones, conecciones.. sus opciones  *MAX_QUERIES_PER_HOUR* count, *MAX_UPDATES_PER_HOUR* count, *MAX_CONNECTIONS_PER_HOUR* count, *MAX_USER_CONNECTIONS* count.
	* ``password_option`` : Opción para la expiración del password, acepta: *PASSWORD EXPIRE*, *PASSWORD EXPIRE DEFAULT*, *PASSWORD EXPIRE NEVER* y *PASSWORD EXPIRE INTERVAL N DAY*
	* ``lock_option`` : Para decir que el usuario esta bloqueado usamod *ACCOUNT LOCK* y para desbloquear *ACCOUNT UNLOCK*



Creación de un usuario que sólo podrá acceder desde el propio servidor que expira en diez días::

	CREATE USER 'jonatan'@'localhost' IDENTIFIED BY 'asd2a3s12S ' PASSWORD EXPIRE INTERVAL 10 DAY;

Creacion de un usuarios que podrá acceder desde cualquir host::
	
	CREATE USER 'jonatan'@'*' IDENTIFIED BY 'asd2a3s12.@';


Eliminación de usuarios
----------------------------------

Para la eliminacion del usuarios usamos la sentencia *DROP USER*::

	DROP USER user;

Asignación de derechos a usuarios
-------------------------------------------------

Para otorgar privilegios a los usuarios haremos uso se la orden *GRANT* su sintaxis es la siguiente::

	GRANT tipo_privilegio [(columnas)] [, tipo_privilegio [(columnas)]]
	ON { nombre_tabla | * | *.*  | base_datos.* }
	TO usuario [IDENTIFIED BY [PASSWORD] 'pasword'] [, usuario [IDENTIFIED BY [PASSWORD] 'password']]
	[WITH option [opcion]]


Desgranando la sintaxis:

	- Tipo_privilegio: 
		Es la clase se privilegio que va ser otorgado al usuario, el tipo de permiso es muy variado: *ALTER*, *SELECT*, *CREATE* *USER*, *CREATE* *VIEW*, *INSERT*, *DROP*, *GRANT OPTION*(Permite dar permisos), *ALL* (Permisos simples excepto *GRANT OPTION*)....

	- Expresiones:
		Los privilegios se pueden aplicar a las siguientes expresiones

		.. list-table::
		   :widths: 15 15
		   :header-rows: 1

		   * - Expresión
		     - Se aplica a
		   * - nombre_tabla
		     - La tabla *nombre_tabla*
		   * - '*'
		     - Todas las tablas de la base de datos que se está usando
		   * - *.*
		     - Todas las tablas de todas las bases de datos
		   * - base_datos.*
		     - Todas las tablas de la base de datos *base_datos* 
		   * - base_datos.nombre_tabla
		     - Solo la tabla de la base de datos

	- WITH:
		Este clausula permite añadir otro tipo de privilegios

		.. list-table::
		   :header-rows: 1

		   * - Expresión
		     - Función
		   * - GRAN OPTION
		     - Permite al usuario conceder sus permisos a otros usuarios
		   * - MAX.QUERIES_PER_HOUR count
		     - Restringe el número de consultas por horas al usuario.
		   * - MAX.UPDATE_PER_HOUR count
		     -  Permite restringir el número de modificaciones por hora.
		   * - MAX.CONNECTIONS_PER_HOUT count 
		     - Indica el numero de conexiones por hora que realiza el usuario.
		   * - MAX.USER_CONNECTIONS count
		     - Limita el número de conexiones simultáneas que puede tener un usuario.


Ejemplo de asignacion de permisos en MySQL.::

	# Necesitamos un administrador para nuestro CMS de Wordpess
	GRANT ALL ON wordpress.* TO 'wordpressuser'@'localhost' 
	IDENTIFIED BY 'password';

	# Permisos de select e insert a todas las tablas de wordpress
	GRANT SELECT, INSERT ON wordpress.* TO 'user'@'localhost' 
	IDENTIFIED BY 'password';

	# Otorgamos permiso de select a todas las tabla de todas las bases de datos
	# y permite otorgar este eprmiso a otros
	GRANT SELECT ON *.* TO 'consultor'@'localhost' 
	IDENTIFIED BY 'password' WITH GRAN OPTION;


Desasignación de derechos a usuarios
------------------------------------------------------

La sentencia REVOKE deniega permisos a un usuarios sobre un objeto. Su sintaxis es la siguiente::

	REVOKE
	tipo_privilegio [(column_list)] [, priv_type [(column_list)]] ...
	ON  { nombre_tabla | * | *.*  | base_datos.* }
	FROM usuarios [, usuarios ]...

Aplicar la sintaxis es muy parecida a la sentencia *GRANT*, por ejemplo vamos a quitar permisos de *SELECT* al usuarios 'wordpressconsultor'::

	REVOKE SELECT on  wordpress.comentarios 
	from 'wordpressconsultor'@'localhost';

Tambien podemos hacer uso de las expresiones::

	REVOKE INSERT,UPDATE  on wordpressconsultor.* from 'jonatan'@'localhost';

.. toctree::