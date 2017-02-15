Bases de datos distribuidas
***************************

Una Base de Datos Distribuidas o BDD es una base de datos que está conformada por un conjunto de ordenadores distrubuidos en diferentes posiciones geográficas o diferentes puntos de la red con esto consguimos un sistema mucho mas que permite la expansión de datos, mejora de rendimiento de respuesta, y una alta disponibilidad de datos.

Este sistema sistema constituido por varios servidores están conectados entre sí, mediante el uso de las redes de comunicaciones en el cual cada sitio lógico puede tener un sistema de base de datos, estos sitios son deseñados para trabajar en conjunto, con un mismo fin, el poder acceder a los datos desde cualquier punto geografico de tal modo como si todos los datos estuvieran almacenados en la posición propio del usuario. 
Cada sitio tiene su propia base de datos, sus propios usuarios, programas para la administracion de transacciones y su propio administrador de comunicaciones de datos. Entonces una BDD es una combinacione de SGBD, donde cada componente realizará las funciones necesarios para la sociedad y en su combinacion tendremos una Sistema de Administracion o Getión de Bases de Datos Distribuidas(DDBMS)

Para el usuarios final este sistema distribuido es identido a uno no distribuido, con la ventaja que ya no habrá problemas del tipo externo o a nivel de usuario final, los problemas serán de tipo interno.

Cada componente del sistema distribuido debe cumplir una serie de requisitos:
	- Antonomia del resto de integrantes: El nuevo componente debe poder realizar sus operaciones sin tener que depender de otro componente para poder efectuar dicha operacion.

	- Operaciones continúa:  No debemos que apagar ningun componente de la sociedad a la hora de ñadir un nuevo componente al grupo o  actualización de uno de ellos.

	- Acceso comun a los datos: El usuario final no tiene  o no debe saber donde están almacenados fisicamente los datos, para él los datos será presentados como  si estuvieran lamacenados en su propio sitio local.

	- Independencia a la fragmentacion:	Los datos deberán poder comportarse como si los datos no estuvieran fragmentados en realidad.

	- Manejo distribuido de consultas: Debemos aplicar ciertas reglas sobre como deben ser ejecutas las consultas, debemos encontrar el camino mas rapido.

	- Manejo distribuido de transacciones: Debemos evitar la concurrencia de datos ya que nuestro sistema distribuidos implica la participacion de varias ejecuaciones en diferentes sitios.

	- Otras: Independencia del Sistema Operativo, del sistema de comunicacion, del equipo donde es instalado.


Tipos de SGBD distribuidos
---------------------------------------

Durante la creación del diseño de la base de datos debemos decidir el esquema sobre el cual serán posicionados los datos en el sistema, para ello tenemos estas alternativas:
Centralizada
Modelo similar al Cliente/Servidor, el BDD está centralizado en un lugar y los usuarios distribuidos, su ventaja principal es el procesamiento distribuido, por contra no obtenemos mejora en cuanto a disponibilidad y fiabilidad de los datos.

- Replicada
	Sistema donde cada componente debe tener su copia completa de la base de datos. Este sistema  premia su alta disponibilidad y fiabilidad de los datos, por contra su costo en almacenamiento de informacion es muy alto igual al costo de escritura de los datos, por ende es recomendable el uso de este diseño en sistema de poca escritura y de mucha consulta.

- Fragmentada
	Este modelo se basa en realizar una copia  de cada elemento, y este estará distribuida en diferentes nodos. Con este sistema conseguimos menos alamacenamiento acosta de la disponibilidad y fialibidad de los datos.

- Híbrida
	Combinacion entre la Replica y la Fragmentada. En el se particiona las relaciones y a la vez los fragmentos son replicados a través de los nodos.

Componentes de un SGBD distribuido
------------------------------------------------------

- Hardware involucrado
	El2 hardware es igual al que se utiliza en un servidor normal

- Software
	Sistema gestor de bases de datos distribuida(DDBMS)
	Es el conjunto de transaciones y los administradores de la base de datos distribuidos.

- Administracion de transacciones distribuidas(DTM)
	El es encargado de traducir las solicitudes de procesamiento de los progrmas de consulta y las traduce en acciones para los administradores de la base de datos

- Sitema Manejador de base de datos (DMBS)
	Programa que procesa parte de la base de datos distribuida.
- Nodo
	El componente que ejecuta el DTM o un DBM, incluso ambos.

Técnicas de fragmentación
--------------------------------------

La técnica de fragmentacion se usa para dividir la Base de Datos en unidades logica, tambien llamadas fragmentos, estos fragmentos serán lamacenados en diferentes sitios. Esta técnica es usada en el proceso de diseño de BDD, donde toda la informacion de la fragmentacion de datos es alamcenada en catalagos globales del sistema.

Este concepto surge en los modelos de particonado y tiene implicaciones lógicas y fisica:

- Lógico	
	Asdasda
- Físico: 
	fragmento logico que se almacena de forma completa en un nodo. Cada fragmento fiísico se alamcena en uno o mas nodos.

Existen diferentes tipos de fragmentacion

- Vertical 
	Divide la relacion verticalemente mediante columnas, con ello se consigue que cada fragmento conserve ciertos artibutos de la relacion original.

- Horizontal
	- Primaria
		Particiona las tuplas/registros de una relacion global en subconjustos, cada uno de ellos tiene significado logico.

	- Derivada
		Se aplica donde una relacion R a fragmentar depende de la relacion Q.

	- Mixta
		- Vertical- Horizontal
			Se dessarrolla la fragmentacion vertical  y luego la horizontal

		- Horizontal- vertical 
			La Inversa de la anterior.


Técnicas de asignación
--------------------------------

Esta tecnica se realiza una vez definada el esquema de fragmentación, ahora ya podemos asignar los fragmentos a diferentes sitios físicos del SGDD.
Su cometido principal el la optimización de de la base de datos, permitiendo una mejor comunicacion entre el usuario y sistema, estas optimizaciones se realizan mediante la evaluacion de la metricas de rendimiento, como por ejemplo el tiempo de respuesta, numero de trabajo ejecutados por unidad de tiempo.

.. toctree::


