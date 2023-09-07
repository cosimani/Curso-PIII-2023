.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 03 - PIII 2023
====================
(Fecha: 7 de septiembre)



Instrucciones básicas de Python
===============================

**Captura de Datos:** *input*

**Muestra de Datos:** *print*

.. code-block:: python 

	# Se pide el ingreso de un número por teclado y se muestra el número y el tipo de dato.
	# Si no forzamos el tipo de dato en el ingreso, lo toma como string (cadena de caracteres)
	numero = input( 'Escriba un número: ' )
	print( 'El número ingresado es: ', numero )
	print( 'El tipo del dato ingresado es ', type( numero ) )


¿Cómo ejecutamos este código?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Creamos una carpeta para almacenar nuestros códigos, por ejemplo: C:\\Cosas\\2022\\PIII2022\\Codigos\\Clase03
- Abrimos Sublime Text y guardamos estas líneas de código en un archivo llamado ``primerCodigo.py``
- Abrir consola con CMD
- Entrar al entorno virtual creado en la clase anterior, ejecutando lo siguiente (acomodar la ruta de ser necesario):

.. code-block:: bash 

	cd C:\Cosas\2022\PIII2022\EntornosVirtuales  # Accedemos a la carpeta

	.\entorno01\Scripts\activate  # Activamos el entorno virtual

	pip install numpy==1.19.5  # Instalamos numpy en la versión 1.19.5

	# Si aparece un mensaje Warning diciendo que hay una versión nueva de pip, podemos ejecutar el comando que nos recomienda

	pip freeze  # Revisamos el listado de paquetes instalados en el entorno virtual

	python C:\Cosas\2022\PIII2022\Codigos\Clase03\primerCodigo.py

	# Recordar que para salir debemos desactivar el entorno virtual
	deactivate

	exit  # Para cerrar la consola


Asignaciones, conversión de tipos, operaciones aritméticas, operadores lógicos y listas
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- El siguiente código es parte de un archivo nuevo (por ejemplo: variosClase02.py).

.. code-block:: python 

	# Tipos de datos básicos en Python: int float str bool complex

	####
	# Conversión de tipos

	numero = int( input( 'Escriba un número: ' ) )
	print( 'El número ingresado es: ', numero )
	print( 'El tipo del dato ingresado es ', type( numero ), 'porque se lo forzó' )

	####
	# Asignación directa

	x = 7  
	print( '\n', x, type( x ) )

	# Los números reales se almacenan en variables tipo "float" 

	nroi = 24
	print( "\nAsignacion de un número entero a una variable. El dato asignado es ", nroi, " y su tipo es ", type( nroi ) )
	nrof = 24.0
	print( "Asignacion de un número real a una variable. El dato asignado es ", nrof, " y su tipo es ", type( nrof ) )

	# Asignamos un entero long
	nroL = 456966786151987643
	print( "\nEl dato asignado a la variable es ", nroL, "y su tipo es ", type( nroL ) )

	# Utilizando notación científica
	nroc = 2e-3
	print( '\n', nroc )
	print( "El tipo de dato asignado con notación científica es ", type( nroc ) )

	# En las versiones actuales no hace falta definir enteros long. Toma tantos bits para almacenar como haga falta

	# Se puede calcular el valor absoluto de un número 
	nro = -3
	print( nro )

	absoluto = abs( nro )
	print( absoluto )

	# Se pueden hacer asignaciones simultáneas:
	nro_1, nro_2, nro_3, nro_4 , var = 0.348, -10.5, 1.5e2, 5, "hola"

	print( nro_1, type( nro_1 ) )
	print( nro_2, type( nro_2 ) )
	print( nro_3, type( nro_3 ) )
	print( nro_4, type( nro_4 ) )
	print( var, type( var ) )

	# Conversión de tipos de datos con las funciones int(), float(), complex()

	h = type( 3.4 )
	o = int( 3.4 )
	l = float( 3 )
	a = float( 3.4 )
	print( h, o, l, a )

	####
	# Operaciones aritméticas

	# Ingresando un número por teclado
	x = float( input( "\nEscriba un número, lo guardaremos en la variable x: " ) )
	print( "El nro ingresado es: ", x )

	print( "Tipo de x:" )
	print( type( x ) ) 
	print( "Valor de la variable x es ", x )

	print( "\nEl resultado de sumarle 1 es ", x + 1 )
	print( "restarle 1 da ", x - 1 )
	print( "multipliicar por dos da", x * 2 )
	print( "hacer x/2, devuelve ", x / 2 )
	print( "si elevamos al cuadrado a x ", x**2 ) 
	print( "\nPara imprimir varios valores en una línea:" )
	print( 1, 2, x, 5 * 2 ) 

	# Operaciones entre números enteros: cociente y resto (o módulo)
	a = 9
	b = 2
	c = a // b  # Cociente entre enteros
	d = a % b  # Resto entre enteros
	print( a )
	print( b )
	print( c )  # Cociente entre enteros- Devuelve 4
	print( d )  # Resto entre enteros- Devuelve 1

	# Otros operadores  =  +=  -=  *=  /=  **=  

	a, b, c, d = 21, 10, 5, 3
	print ( "\nc =", c )
	c += 2
	print ( "c += 2  -> c =", c )
	c *= 10
	print ( "c *= 10  -> c =", c )
	c /= 10 
	print ( "c /= 10  -> c =", c )

	####
	# booleanos (True y False)

	v1 = True
	v2 = False

	print( "\nValor de v1: ", v1, "Su tipo es: ", type( v1 ) )
	print( "Valor de v2: ", v2, "Su tipo es: ", type( v2 ) )

	####
	# Operaciones Lógicas AND, OR y NOT

	print( "\nv1 and v2 = ", v1 and v2 )
	print( "v1 or v2 = ", v1 or v2 )
	print( "v1 negado = ", not v1 )

	####
	# Comparaciones

	print ( "\n3==5", 3 == 5 ) 
	print( "3 !=6", 3 != 6 ) 
	print( "3<5", 3 < 5 ) 


	####
	# Listas 

	lista = [ 1, 3, 5 ]
	print( "\nlista =", lista, "su tipo es ", type( lista ) )

	# Acceso a un elemento de la lista
	print( lista[ 0 ] )

	# Lista con elementos de distinto tipo
	l = [ 1, 2, 'tres' ]
	print( l )
	print( l[ 1 ], 'es el segundo elemento de la lista' )

	####
	# Funciones

	def sumar( a, b ) :
	    return a + b

	c = sumar( 2, 5 )
	print( "\nLa suma es", c )

	lista1 = [ 1, 3, 5 ]
	lista2 = [ 11, 13, 15 ]

	c = sumar( lista1, lista2 )
	print( "\nLa suma es", c )




IDE para Python
^^^^^^^^^^^^^^^

- Descargar `Spyder <https://www.spyder-ide.org/>`_
- Versión actual: 5.3.2
- Instalar para todos los usuarios.

Módulos y paquetes
==================

**Módulo**: Es un archivo Python cuyas utilidades (funciones, clases, etc.) se pueden usar desde otro archivo.

- Supongamos el archivo ``matematicas.py``

.. code-block:: python 

	def sumar( a, b ) :
	    return a + b

	def restar( a, b ) :
	    return a - b

- Podremos utilizar estas funciones de la siguiente manera:

.. code-block:: python

	import matematicas  # Esta línea importa todos los recursos del archivo matematicas.py

	print( matematicas.sumar( 7, 5 ) )
	print( matematicas.restar( 17, 15 ) )

- Si sólo deseamos importar la función ``sumar`` hacemos:

.. code-block:: python

	from matematicas import sumar

	print( sumar( 7, 5 ) )

- Otras alternativas:

.. code-block:: python
	
	from matematicas import sumar, restar

	from matematicas import *


**Paquetes**: Es una carpeta que contiene varios módulos. 

.. code-block:: bash 

	operaciones/
	    |-- __init__.py    # Este archivo indica que la carpeta operaciones es un paquete y no una simple carpeta 
	    |-- matematicas.py
	    |-- matrices.py


- Alternativas para importar

.. code-block:: python
	
	import operaciones.matematicas

	from operaciones import matematicas

	from operaciones.matematicas import sumar



Ejercicio
=========

- Asigne el valor 7 a la variable  x
- Verifique e imprima la veracidad de la siguiente afirmación: ``x**2 + 5 − 2`` igual a ``( x ∗ 5 − 9 ) ∗ 2`` 
- Verifique e imprima que no es cierto si x es -7
- Crear la función ``getMax`` que reciba una lista de números y que devuelva dos valores, el número mayor y el número menor de la lista
- Sabiendo cómo funciona un rectificador de onda completa. Crear la función ``rectificarSecuencia`` que reciba una secuencia de muestras y la devuelva rectificada.



