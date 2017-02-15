Herramientas de monitorización
*******************************

Oracle y mysql disponen de herramientas graficas que permiten realizar tareas de monitorización y administración, estas herramientas suelen monitorizar algunos parametros generales de nuestro SGDB, generar alertar personalizadas mediante triggers.
Oracle dispone de Oracle Database Express Edition.
Mysql dispone de MySQL Workbench para la monitorización, en sistemas basados en Unix podemos hacer uso de la terminal que permite lanzar comandos para monitorizar el sistema de base de datos y el servidor. 


Elementos y parámetros susceptibles de ser monitorizados
------------------------------------------------------------------------------------

El principal elemento de monitorización son los ficheros de 'logs', estos ficheros son escritos en texto plano y permiten el seguimiento del sistema mediante trazas que va generando los procesos que son ejecutados, tambien es posible guardar estos datos en un tabla alojada en nuestra base de datos.
Otros factores que demos prestar atención pueden ser el consumo del SGDB, tanto en consumo de CPU para la integridad del server, el consumo de la memoria RAM, la velocidad de transferencia de datos por red.


Optimización
-------------------

- Memoria fisica:
	Conocer la estrucutra de momoria del sistema, parametros para su configuracion y aquellos metodos de monitorizacion y chequeo de la misma permitiran obtener una correcta configuracion de la memoria asignada al servidor.
	En MySQL, disponemos de varias zonas de memoria: memoria dela instacion, caché de consultas, caché de indices, y una para el sistema de archivos. Tambien reserva otro espacio de momoria para las conexiones de los clientes, este espacion puede ser configurada mediante el parametro SORT_BUFFER.
	El tamaño de la cache puede ser configurado mediante el parametro "query_cache_siza"

- Distribucion de los datos: 
	Debemos saber donde vamos a guardar los datos de la base de datos, por lo general se almacenan en discos o particion diferente al del sistema operativo, tambien podemos tener una configuracion de sistemas de RAID y aprovechar las optimizaciones que ofrecen este tipo de sistemas.

- Acceso a los datos:
	El SGDB de mysql dispone de un motor de almacenamiento independiente y transparente a los usuarios. Debemos realizar una  correcta eleccion del motor para la tabla ya que influye notablemente en el rendimiento del sistema. Existen diferentes motores para mysql y cada uno de ellos con sus propias caracteristicas Memory, Archive, Blackhole, CSV, MyISAM y por defecto  InnoDB desde MySQL 5.5

- En MYSQL podemos aumentar la optimizar mediante la busqueda de errores, esto es posible realizar mediante los diferentes fichero de 'logs':
	Registros de consulta	 Donde aloja todas las consultas ejecutas por los usuarios.
	Registros de errores 	 Informa de los errores mas importantes que suceden el MySQL, al igual que el inicio y el apagado del servidor.
	Registro binario 	Almacena las órdenes de actulizacion de datos
	Registro de conultas lentas 	Como bien su nombre indica, guarda las consultas lentas.

.. toctree::