Vistas. ¿Qué son?
*****************
Una vista es una tabla virtual que devuelve el resultado de una consulta SQL compleja. La informacion presenta siempre está actualizada, ya que está no se guarda en disco, es generada.
La creacion de vista permite añadir una capa de seguridad sobre los datos de la DB ya que los usuarios solo podrán acceder solo a los datos que arroje la vista.


Creación de vistas
--------------------------
Para la creación de vistas utilizamos *CREATE* la sintaxis es la siguientes::

	CREATE 
		[OR REPLACE]
		[ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE} ]
		[DEFINER = { USER | CURRENT_USER }]
		[SQL SECURITY { DEFINER | INVOKER }]
		VIEW view_name [{column_list}]
		AS sentencia_select
		[WITH [CASCADED | LOCAL ] CHECK OPTION]

.. list-table::
 :widths: 1,1
 :stub-columns: 1

 * - Expresión
   - Descripción
 * - *OR REPLACE*
   - Esta sentencia permite crear una vista o reemplezar la existente.
 * - *ALGORITHM*
   - Permite elegir el algoritmo que debe usar para la creación de la vista, permite tres opcioes: *MERGE*, *TEMPTABLE* esta opciones crea una tabla temporal y *UNDEFINED* para que sea elegido por MySQL.
 * - *DEFINER*
   - Determina qué usuario tiene la cuenta asociada a la vista.
 * - *SQL_SECURITY*
   - Permite aplicar el permiso que se definio en *DEFINER* o aplicar los permisos por quien la invoca *INVOKE*.
 * - *WITH CHECK OPTION*
   - Evita las modicaciones de la filas que no están involucradas por la consulta definida en la vista. *LOCAL* actua en la vista actual o *CASCADE* de extiende a otras vistas incluidas.


Ejemplos:
	Usando la base de datos **world** de MySQL:

	Creamos una vista con aquellas ciudad con una población menor a mil habitantes:
		.. image:: img/createview-1.png

	Ahora vamos a crear una vista para el usuarios jonatan pero denegamos que la modificaciones de registros no involucrados., vista el la misma a la anterior.
		.. image:: img/createview-2.png

	SQL::

		CREATE
			DEFINER='jonatan' 
			VIEW menos_1000_jonatan 
			AS SELECT * FROM city WHERE Population <= 10000 
			WITH LOCAL CHECK OPTION;

Debemos saber:

	- Las tablas a la que se hace referencia deben existir siempre.
	- No debemos eliminar un campo de una tabla o un registro que es usado por una vista, para evitar esto utilizamos la sentencia *CHECK TABLE*, que permite comprobrar la existencia de la tabla;
	- No permite subconsultas.
	


Modificación de vistas
-------------------------------
Para la modificacion de una vista basta con saber el nombre para poder modificarla::

	ALTER [ ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE} ]
		VIEW nombre_vista [ {columnas} ]
		AS sentencia_select
		[WITH [CASCADED | LOCAL | CHECK ]OPTION ]

EL uso de las opciones es igual al comando *CREATE VIEW*.

Eliminación de vistas
------------------------------
Para eliminar la vista su sintaxis es simple, pero debemos asegurarnos de tener los permisos necesarios para poder realizar esta accion.::

	DROP VIEW [IF EXISTS] vista1, vista2, ...;

Es recomendable usar la clausula *IF EXISTS* para no mostrar error en el caso de que la tabla no exista.

.. toctree::
