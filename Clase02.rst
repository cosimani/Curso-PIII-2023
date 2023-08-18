.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 02 - PIII 2023
====================
(Fecha: 18 de agosto)



Instalación de herramientas
===========================

:Python: 
	- Descargamos e instalamos `Python 3.8.10 <https://www.python.org/downloads/release/python-3810/>`_ 
	- Elegir instalador para Windows 64 bits (archivo python-3.8.10-amd64.exe)
	- Realizamos una instalación customizada (Install for all users - Create shortcuts for installed applications - Add Python to environment variables - Precompile standard library - pip - C:\\Program Files\\Python38) 
	- Verificamos la instalación de Python ejecutando desde consola ``python --version``
	- Verificamos la instalación de PIP ejecutando desde consola ``python -m pip --version``

:Sublime Text:
	- Herramienta de edición de texto y código fuente. Descargar `Sublime Text <https://www.sublimetext.com>`_


Creación de entorno virtual
===========================

.. code-block:: bash 

	# Lo siguiente se ejecuta desde consola, por ejemplo en C:\Users\Usuario>

	pip install virtualenv==20.4.6  # Instala la herramienta para generar entornos virtuales

	pip freeze  # Muestra el listado de paquetes instalados

	# Para crear un entorno virtual, creamos una carpeta donde se estarán todos los entornos virtuales.
	# Creamos por ejemplo la carpeta -> C:\Cosas\PIII\EntornosVirtuales

	cd C:\Cosas\PIII\EntornosVirtuales  # Accedemos a la carpeta

	virtualenv entorno01  # Creamos un entorno virtual llamado entorno01

	.\entorno01\Scripts\activate  # Activamos el entorno virtual

	# El comando anterior nos lleva al entorno virtual -> (entorno01) C:\Cosas\PIII\EntornosVirtuales>
	# También podemos ver que se creó un directorio nuevo -> C:\Cosas\PIII\EntornosVirtuales\entorno01 

	pip freeze  # Ejecutamos esto dentro del entorno virtual para ver los paquetes instalados

	pip install numpy  # Instalamos numpy
	pip install matplotlib  # Instalamos matplotlib
	pip install numpy==1.19.5  # Instalamos numpy en su versión 1.19.5

	deactivate  # Desactivamos el entorno virtual 
	
	# Para borrar el entorno virtual hay que borrar la carpeta donde se creó -> C:\Cosas\PIII\EntornosVirtuales\entorno01 

	# Si desea actualizar la versión de la herramienta pip, ejecutar lo siguiente:
	python.exe -m pip install --upgrade pip


Ejemplo en Google Colab
=======================


- `https://colab.research.google.com/drive/1ZWUXGi-ZOOlEo3b1MGbtj0Wo-zzVGghF?usp=sharing <https://colab.research.google.com/drive/1ZWUXGi-ZOOlEo3b1MGbtj0Wo-zzVGghF?usp=sharing>`_
