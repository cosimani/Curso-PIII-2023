.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 09 - PIII 2023
====================
(Fecha: 2 de noviembre)

Exponential Moving Average Filter (media móvil exponencial)
===========================================================

- La ecuación en diferencia para este filtro es la siguiente:

.. figure:: images/formula_ema.png

- alpha está en el intervalo ``[0, 1]`` y es el factor de suavizado
- ``y[n]`` es la salida actual
- ``y[n - 1]`` es la salida anterior
- ``x[n]`` es la entrada actual

Ejemplos con distintos valores de factor de suavizado
-----------------------------------------------------

.. figure:: images/ema_alpha_01.png

.. figure:: images/ema_alpha_02.png

- Se llama exponencial porque el factor de ponderación aplicado a las entradas pasadas disminuye exponencialmente. Lo podemos visualizar sustituyendo las salidas anteriores.

.. figure:: images/sumatoria_ema.png


Ejercicio 1
===========

- Capturar con el micrófono algunos segundos.
- Definir una función en Python que aplique este filtro de media móvil exponencial.
- Suavizar la señal con un factor de 0.6, 0.2 y 0.05 y visualizar el resultado.
- Realizar un análisis del espectro de frecuencias.
- Plotear la señal original y la filtrada superpuestas para notar el suavizado.



Creación de GUI
===============

- QtDesigner para diseñar una interfaz con modalidad Drag and Drop.
- Creación de entorno virtual
- Instalación de bibliotecas
- Características de la biblioteca Qt
- Manejo de eventos
- Gráficos y reproducción

**Información**

- Ir a https://www.qt.io/download para descargar el QtCreator. No hace falta más que el QtCreator que tiene el QtDesigner que nos genera el .ui
- Ver aquí cómo se utiliza el .ui para generar la interfaz. https://www.pythonguis.com/tutorials/pyside-first-steps-qt-designer/


.. code-block:: bash 

	cd C:\Cosas\EntornosVirtuales
	mkdir PIII_con_GUI
	cd PIII_con_GUI

	virtualenv entorno_gui_piii
	.\entorno_gui_piii\Scripts\activate

	pip install numpy==1.19.5
	pip install shiboken6==6.0.0
	pip install PySide6==6.0.0 
	pip install matplotlib==3.3.4 
	pip install scipy==1.7.1
	pip install sounddevice==0.4.2

	python C:\Cosas\PIII\Codigos\GUI\ventana.py


Guía para creación de GUI
=========================

- Carpeta para contener el entorno virtual

.. code-block:: bash 

	cd C:\Cosas\PIII\EntornosVirtuales
	
- Creación y habilitación del entorno virtual

.. code-block:: bash 

	virtualenv entorno01
	.\entorno01\Scripts\activate


- Instalación de bibliotecas

.. code-block:: bash 

	pip install numpy==1.19.5
	pip install shiboken6==6.0.0
	pip install PySide6==6.0.0
	pip install matplotlib==3.3.4
	pip install scipy==1.7.1
	pip install sounddevice==0.4.2

- Creación de la interfaz gráfica con QtDesigner (que pertenece a QtCreator)
- New Project -- Application (Qt for Python) -- Window (UI FIle) -- Abrir el archivo .ui y diseñar

.. figure:: images/gui_parte1.png


- Del proyecto creado con QtDesigner sólo utilizaremos el archivo .ui. Este archivo no es código Python sino que es un script XML el cual debe ser interpretado con la clase QUiLoader.

- Para ejecutar esta interfaz necesitamos un archivo Python como el que sigue:


.. code-block:: python 

	import os, sys

	from PySide6.QtCore import *
	from PySide6.QtWidgets import QWidget, QApplication, QGridLayout
	from PySide6.QtUiTools import QUiLoader

	import numpy as np

	from matplotlib import pyplot as plt
	plt.style.use( 'seaborn-darkgrid' )

	from scipy import signal
	import sounddevice as sd

	class Ventana( QWidget ) :

	    def __init__( self ) :
	        super( Ventana, self ).__init__()

	        loader = QUiLoader()
	        self.gui = loader.load( "panel.ui", None )  # panel.ui debe estar en la misma carpeta

	        # Define un layout en Ventana y coloca allí la interfaz creada con QtDesigner
	        grid = QGridLayout()
	        grid.setContentsMargins( 0, 0, 0, 0 )
	        grid.addWidget( self.gui )
	        self.setLayout( grid )

	        self.setWindowTitle( 'Panel de configuración' )

	        # Conexiones realizadas para capturar los eventos de la interfaz
	        QObject.connect( self.gui.pbSierraPlay, SIGNAL( "pressed()" ), self.slot_sierraPlay )
	        QObject.connect( self.gui.pbSierraPlot, SIGNAL( "pressed()" ), self.slot_sierraPlot )
	        QObject.connect( self.gui.pbCuadradaPlay, SIGNAL( "pressed()" ), self.slot_cuadradaPlay )
	        QObject.connect( self.gui.pbCuadradaPlot, SIGNAL( "pressed()" ), self.slot_cuadradaPlot )
	        QObject.connect( self.gui.pbCerrar, SIGNAL( "pressed()" ), self.slot_cerrarAplicacion )

	    def slot_cerrarAplicacion( self ) :
	        print( 'App cerrada' )
	        self.close()        

	    def slot_sierraPlay( self ) :
	        sample_rate = 44100
	        duracion = 1
	        n = np.linspace( 0, duracion, sample_rate * duracion )
	        plt.xlim( [ 0, 0.005 ] )

	        frecuencia = int( self.gui.leSierra.text() )
	        triangle = signal.sawtooth( 2 * np.pi * frecuencia * n, 0.5)      
	        sd.play( triangle, sample_rate )

	    def slot_sierraPlot( self ) :
	        sample_rate = 44100
	        duracion = 1
	        n = np.linspace( 0, duracion, sample_rate * duracion )
	        plt.xlim( [ 0, 0.005 ] )
	        frecuencia = int( self.gui.leSierra.text() )        
	        triangle = signal.sawtooth( 2 * np.pi * frecuencia * n, 0.5)
	        plt.plot( n, triangle )  
	        plt.show()


	    def slot_cuadradaPlay( self ) :
	        print( 'slot_cuadradaPlay' )

	    def slot_cuadradaPlot( self ) :
	        print( 'slot_cuadradaPlot' )        


	    def keyPressEvent( self, e ) :

	        if e.key() == Qt.Key_Escape :
	            self.close()


	# Función main que se ejecuta al iniciar la aplicación
	if __name__ == '__main__':

	    # Este objeto representa a la aplicación
	    app = QApplication( sys.argv )

	    os.chdir( os.path.dirname( os.path.abspath( __file__ ) ) )

	    # Creamos y visualizamos la Ventana que contiene la interfaz creada en QtDesigner
	    ventana = Ventana()
	    ventana.show()

	    sys.exit( app.exec_() )


- El código anterior lo almacenamos en un archivo ventana.py y debe estar en la misma carpeta que el archivo panel.ui que es la interfaz creada con QtDesigner. 
- No es necesario que estos archivos se encuentren en la carpeta donde fue creado el entorno virtual (y es recomendable que no estén allí).
- Para ejecutar la aplicación hacemos:

.. code-block:: bash 

	python C:\Cosas\PIII\Codigos\GUI\ventana.py

Ejercicio 2
===========

- Diseñar la interfaz anterior y ejecutar la aplicación.
- Agrupar las distintas funciones que se vienen usando y almacenarlas a todas en un único archivo .py




Ejemplo para grabar y repoducir
===============================

.. figure:: images/gui_ejemplo.png


ventana.py
----------

.. code-block:: python

	import os, sys

	from PySide6.QtCore import *
	from PySide6.QtWidgets import QWidget, QApplication, QGridLayout
	from PySide6.QtUiTools import QUiLoader

	import numpy as np

	from matplotlib import pyplot as plt
	plt.style.use( 'seaborn-darkgrid' )

	from scipy import signal
	import sounddevice as sd

	class Ventana( QWidget ) :
	    
	    def __init__( self ) :
	        super( Ventana, self ).__init__()

	        loader = QUiLoader()
	        self.gui = loader.load( "panel.ui", None )  # panel.ui debe estar en la misma carpeta

	        # Define un layout en Ventana y coloca allí la interfaz creada con QtDesigner
	        grid = QGridLayout()
	        grid.setContentsMargins( 0, 0, 0, 0 )
	        grid.addWidget( self.gui )
	        self.setLayout( grid )
	 
	        self.setWindowTitle( 'Panel de configuración' )

	        self.grabacion = 0

	        QObject.connect( self.gui.pbReproducir, SIGNAL( "pressed()" ), self.slot_reproducir )
	        QObject.connect( self.gui.pbGrabar, SIGNAL( "pressed()" ), self.slot_grabarAudio )

	    def slot_reproducir( self ) :
	        
	        frecuencia_muestreo = 44100
	        sd.play( self.grabacion, frecuencia_muestreo )

	        print( len( self.grabacion ) )
	        

	    def slot_grabarAudio( self ) :
	  
	        duracion = 3
	        frecuencia_muestreo = 44100
	          
	        self.grabacion = sd.rec( int( duracion * frecuencia_muestreo ), 
	        						 samplerate = frecuencia_muestreo, 
	        						 channels = 1, 
	        						 blocking = True ) 

	        print( type( self.grabacion ) )
	        print( self.grabacion, self.grabacion.shape )


	    def keyPressEvent( self, e ) :

	        if e.key() == Qt.Key_Escape :
	            self.close()


	# Función main que se ejecuta al iniciar la aplicación
	if __name__ == '__main__':

	    # Este objeto representa a la aplicación
	    app = QApplication( sys.argv )

	    os.chdir( os.path.dirname( os.path.abspath( __file__ ) ) )

	    # Creamos y visualizamos el objeto Ventana que contiene la interfaz creada en QtDesigner
	    ventana = Ventana()
	    ventana.show()

	    sys.exit( app.exec_() )


panel.ui
----------------

.. code-block:: python

	<?xml version="1.0" encoding="UTF-8"?>
	<ui version="4.0">
	 <class>Widget</class>
	 <widget class="QWidget" name="Widget">
	  <property name="geometry">
	   <rect>
	    <x>0</x>
	    <y>0</y>
	    <width>626</width>
	    <height>281</height>
	   </rect>
	  </property>
	  <property name="windowTitle">
	   <string>Widget</string>
	  </property>
	  <layout class="QGridLayout" name="gridLayout_2">
	   <item row="4" column="6">
	    <spacer name="horizontalSpacer">
	     <property name="orientation">
	      <enum>Qt::Horizontal</enum>
	     </property>
	     <property name="sizeHint" stdset="0">
	      <size>
	       <width>40</width>
	       <height>20</height>
	      </size>
	     </property>
	    </spacer>
	   </item>
	   <item row="4" column="3">
	    <spacer name="horizontalSpacer_4">
	     <property name="orientation">
	      <enum>Qt::Horizontal</enum>
	     </property>
	     <property name="sizeHint" stdset="0">
	      <size>
	       <width>40</width>
	       <height>20</height>
	      </size>
	     </property>
	    </spacer>
	   </item>
	   <item row="1" column="6" rowspan="3">
	    <widget class="QGroupBox" name="gbResumen">
	     <property name="title">
	      <string>Resumen</string>
	     </property>
	     <layout class="QGridLayout" name="gridLayout">
	      <item row="2" column="0">
	       <widget class="QLabel" name="lAmplMaxima">
	        <property name="text">
	         <string>Amplitud máxima:</string>
	        </property>
	       </widget>
	      </item>
	      <item row="0" column="0">
	       <widget class="QLabel" name="lCantMuestras">
	        <property name="text">
	         <string>Cantidad de muestras:</string>
	        </property>
	       </widget>
	      </item>
	      <item row="1" column="0">
	       <widget class="QLabel" name="lFrecMuestreo">
	        <property name="text">
	         <string>Frecuencia de muestreo:</string>
	        </property>
	       </widget>
	      </item>
	      <item row="3" column="0">
	       <widget class="QLabel" name="lAmplMinima">
	        <property name="text">
	         <string>Amplitud mínima:</string>
	        </property>
	       </widget>
	      </item>
	     </layout>
	    </widget>
	   </item>
	   <item row="0" column="6">
	    <spacer name="verticalSpacer_2">
	     <property name="orientation">
	      <enum>Qt::Vertical</enum>
	     </property>
	     <property name="sizeHint" stdset="0">
	      <size>
	       <width>20</width>
	       <height>40</height>
	      </size>
	     </property>
	    </spacer>
	   </item>
	   <item row="4" column="0" colspan="3">
	    <layout class="QHBoxLayout" name="horizontalLayout_2">
	     <item>
	      <layout class="QVBoxLayout" name="verticalLayout_2">
	       <item>
	        <widget class="QLabel" name="lDesdeLaMuestra">
	         <property name="text">
	          <string>Graficar desde la muestra:</string>
	         </property>
	        </widget>
	       </item>
	       <item>
	        <widget class="QLabel" name="lCantMuestrasGraficadas">
	         <property name="text">
	          <string>Cantidad de muestras graficadas:</string>
	         </property>
	        </widget>
	       </item>
	      </layout>
	     </item>
	     <item>
	      <layout class="QVBoxLayout" name="verticalLayout">
	       <item>
	        <widget class="QLineEdit" name="leDesdeLaMuestra"/>
	       </item>
	       <item>
	        <widget class="QLineEdit" name="leCantMuestrasGraficadas"/>
	       </item>
	      </layout>
	     </item>
	    </layout>
	   </item>
	   <item row="5" column="2">
	    <widget class="QPushButton" name="pbGraficar">
	     <property name="text">
	      <string>Graficar</string>
	     </property>
	    </widget>
	   </item>
	   <item row="3" column="0">
	    <widget class="QCheckBox" name="checkSuavizar">
	     <property name="text">
	      <string>Suavizar</string>
	     </property>
	    </widget>
	   </item>
	   <item row="2" column="2">
	    <spacer name="verticalSpacer">
	     <property name="orientation">
	      <enum>Qt::Vertical</enum>
	     </property>
	     <property name="sizeHint" stdset="0">
	      <size>
	       <width>20</width>
	       <height>40</height>
	      </size>
	     </property>
	    </spacer>
	   </item>
	   <item row="4" column="5">
	    <spacer name="horizontalSpacer_2">
	     <property name="orientation">
	      <enum>Qt::Horizontal</enum>
	     </property>
	     <property name="sizeHint" stdset="0">
	      <size>
	       <width>40</width>
	       <height>20</height>
	      </size>
	     </property>
	    </spacer>
	   </item>
	   <item row="5" column="0">
	    <spacer name="horizontalSpacer_5">
	     <property name="orientation">
	      <enum>Qt::Horizontal</enum>
	     </property>
	     <property name="sizeHint" stdset="0">
	      <size>
	       <width>40</width>
	       <height>20</height>
	      </size>
	     </property>
	    </spacer>
	   </item>
	   <item row="4" column="4">
	    <spacer name="horizontalSpacer_3">
	     <property name="orientation">
	      <enum>Qt::Horizontal</enum>
	     </property>
	     <property name="sizeHint" stdset="0">
	      <size>
	       <width>40</width>
	       <height>20</height>
	      </size>
	     </property>
	    </spacer>
	   </item>
	   <item row="0" column="0" colspan="4">
	    <layout class="QHBoxLayout" name="horizontalLayout">
	     <item>
	      <widget class="QPushButton" name="pbGrabar">
	       <property name="text">
	        <string>Grabar audio</string>
	       </property>
	      </widget>
	     </item>
	     <item>
	      <widget class="QPushButton" name="pbAbrir">
	       <property name="text">
	        <string>Abrir archivo</string>
	       </property>
	      </widget>
	     </item>
	    </layout>
	   </item>
	   <item row="5" column="1">
	    <widget class="QPushButton" name="pbReproducir">
	     <property name="text">
	      <string>Reproducir</string>
	     </property>
	    </widget>
	   </item>
	  </layout>
	 </widget>
	 <resources/>
	 <connections/>
	</ui>



Preparando el proyecto final
----------------------------

- Creación de GUI.
- Creación de ejecutable con `cx_Freeze <https://cx-freeze.readthedocs.io/en/latest/>`_ 
- Interfaz intuitiva y a prueba de errores humanos.
- Botones y campos de texto que se habiliten/deshabiliten oportunamente.


Ejercicio 3
===========

- Preparar una GUI que deshabilite y habilite botones y/o campos de texto oportunamente.
- Colocar los nombres de variables adecuados a los objetos de la GUI.
- Grabar un audio con el micrófono y luego tener la opción de reproducirlo o graficarlo. 





