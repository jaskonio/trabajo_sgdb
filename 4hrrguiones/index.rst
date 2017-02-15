Herramientas para la creación de guiones
****************************************

Los SGDB desarrollaron un conjunto de técnicas con el motivo de ampliar la capacidad del lenguaje SQL y aumentar la eficiencia en el desarrollo de aplicaciones sobre la base de datos
Estas técnicas son:

	* Guiones simples
	* Disparadores
	* Eventos

Planificación de tareas administrativas mediante guiones
----------------------------------------------------------------------------------

Tambien conocidos por 'scripts', son guardados en texto plano con la extension '.SQL' donde se podemos especificar una serie tarea que serán ejecutas en el SGBD.
Su principal ventaja recae en su facilidad de uso ya sea desde consola y mediante una interfaz grafica de getión de los SGBD que permiten generar el codigo SQL  para la creacion de una base de datos alojada en nuestro servidor, dicho archivo consta con todas las sentencias SQL que permite crear la estructura de la base de datos.

Un ejemplo del uso de guiones, vamos a crear un fichero SQL, donde indicamos la base de datos que vamos a usar, creamos una vista que va listar  los primeros 10 comentarios de nuestra base de datos de wordpress y para finalizar damos permiso de *SELECT* al usuarios consultor::


	USE wordpress;

	CREATE OR REPLACE VIEW comentarios_p10 AS

	SELECT id, nombre, fecha, comentario

	FROM comentarios

	LIMIT 10

	GRANT SELECT ON wordpress.comentarios_p10 TO consultor;

Una vez guardado el fichero este puede ser ejecuta en cualquie momento, cabe destacar la orden *REPLACE VIEW* que permite reemplazar si existe una vista con el mismo nombre.


Eventos
-----------

El uso de eventos permite realizar tareas programadas como por ejemplo elimiar registros, añadir, actulizar registros, etc. Este evento pueder ser lanzado en su creacion, podemos indicar cuando debe actuar y durante cuanto tiempo actuará el evento.

.. note ::
	Para iniciar el uso de los eventos antes debemos activar el planificador de mysql, es quien se encarga de buscar en segundo plano eventos a ajecutar::

		SET GLOBAL event_sheduler= ON:

	Y del mismo modo para descativar el plenificador::

		SET GLOBAL event_sheduler = OFF;

Creacion de eventos::

	CREATE
	[DEFINER = { user | CURRENT_USER }]
	EVENT
	[IF NOT EXISTS]
	event_name
	ON SCHEDULE schedule
	[ON COMPLETION [NOT] PRESERVE]
	[ENABLE | DISABLE | DISABLE ON SLAVE]
	[COMMENT 'comment']
	DO event_body;

	schedule:
	AT timestamp [+ INTERVAL interval] ...
	| EVERY interval
	[STARTS timestamp [+ INTERVAL interval] ...]
	[ENDS timestamp [+ INTERVAL interval] ...]

	interval:
	quantity {YEAR | QUARTER | MONTH | DAY | HOUR | MINUTE |
	WEEK | SECOND | YEAR_MONTH | DAY_HOUR | DAY_MINUTE |
	DAY_SECOND | HOUR_MINUTE | HOUR_SECOND | MINUTE_SECOND}

Un ejemplo simple de la sentencia minima::

	CREATE EVENT nombre_evento
	ON SCHEDULE AT CURRENT_TIMESTAMP + INTERVAL 1 HOUR
	DO
	UPDATE db.tabla SET campo = campo + 1;

Este ejemplo aumenta el valor del campo, esta sentencia será ejecutada en un intervalo de tiempo de un hora.

Un ejemplo algo más complejo, en este ejemplo usamos las clausalas START y END le diremos cuando debe empezar a ejectar el evento y cuanto debe acabar::

	CREATE EVENT nombre_evento
	ON SCHEDULE EVERY 1 HOUR
	STARTS CURRENT_TIMESTAMP + INTERVAL 1 DAY
	ENDS CURRENT_TIMESTAMP + INTERVAL 1 YEAR
	DO
	BEGIN
	UPDATE db.tabla SET campo = campo + 1;
	END



Actualizacion de eventos::

	Alter EVENT name_event
	ON SCHEDULE EVERY 1 MONTH
	STARS "2017-15-01 01:00:00"

Modificar el nombre de un evento::

	Alter EVENT evento
	RENAME TO new_name_event;


Disparadores
-------------------

Tambien conocidos como trigger, es un objeto con nombre dentro de la base de datos, este objeto es asociado a una tabla esta tabla y se activa cuando se dispara un evento en la tabla en particular, esta tabla no puede ser temporal y tampoco puede ser usada en una vista. La creacion de los disparadores debe ser unica por cada evento que se activa en la tabla.

Como bien hemos dicho antes, los disparadores son activas mediante eventos estos eventos pueden ser INSERT, DELETE o UPDATE. Tambien podemos definir cuando deben activarse antes o despues de la ejecucion del evento. Un ejemplo aplicable podría ser la verificacion de datos insertados, podemos crear un disparador que sea ejecutado antes de realizar el evento INSERT y verificar los datos antes de insertarlos en la tabla. El uso de los trigger es amplio, el ejemplo anterior haci referencia a la verificacion de los datos, tambien podemos crear triggers que registran los datos introducidos e informar qué registros son introducidos en la tabla.


Sintaxis para la creacion de un trigger::

	CREATE [DEFINER={usuario|CURRENT_USER}]
	TRIGGER nombre_del_trigger {BEFORE|AFTER} {UPDATE|INSERT|DELETE}
	ON nombre_de_la_tabla
	FOR EACH ROW
	<instrucciones>

- La sentencia CREATE indicar que vamos a crear un nuevo objeto
- DEFINER: Indica qué usuarios tiene privilegios de lanzar el disparador, por defecto el valor es CURRENT_USER que hace referencia al usuario que está creando el objeto.

- nombre_del_trigger: Nombre del trigger, el nombre debe ser muy explicito, lo que ha generado una nomenclatura para la asignacion del nombre, Nombre_tabla_OperacionMomentodeEjecucion_TRIGGER

- BEFORE|AFTER : Momento de la ejecucion del trigger antes o depués del evento.

- UPDATE|INSERT|DELETE:  	Sentencia que se usa para que se ejecute el trigger

- ON nombre_de_la_tabla : 	Nombre de la tabla asociada.

- FOR EACH ROW: 		Indica que se ejecuta el trigger por cada fila en la tabla

- <instrucciones>:		Sentencia a ejecutar por el trigger


Identificadores
Los identificadores permiten la relacion entre columnas espeficas de una tabla, estos identificacdores son OLD y NEW:

	- OLD.campo	Indica el valor antiguo de la columna. Puede ser usado en sentencia DELETE, UPDATE, 

	- NEW.campo 	Indica el nuevo valor que puede tomar. Puede ser usado en sentencias haga referencia a registros luego de actualizarlo, por ejemplo, INSERT y  UPDATE.


Presentación de ejemplos
Vamos a registrar cuando un usuario cambia su nombre de nuestra tabla clientes, el cambio será reflejado en la tabla log_clientes. El siguiente trigger va actuar antes de ser lanzado la sentencia UPDATE, por ello haremos uso de los identificadores OLD para hace referencia al campo antiguo y NEW para el nuevo campo que va se introducido::


	CREATE TRIGGER clientes_AU_Trigger
	AFTER UPDATE ON clientes FOR EACH ROW
	BEGIN
	INSERT INTO log_clientes 
	VALUES (user(), OLD.id_cliente, OLD.nombre_cliente, NEW.id_cliente, NEW.nombre_cliente);


Para eliminar un trigger se usa la sentencia DROP TRIGGER::

	DROP TRIGGER nombre_trigger 


Excepciones
-------------------

El manejo de errores durante el funcionamiento del programa es vita es de suma importancia, ya que sino controlamos el error puede que nuestro programa falle y termine su ejecución, por ello podemos capturar el error analizarlo y dar una respuesta personalizada a los diferentes errores, es importante realizar estás pruebas en el proceso de desarrrollo sel codigo.


.. toctree::


