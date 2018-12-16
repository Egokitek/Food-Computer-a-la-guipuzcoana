# Instalación del Software MVP III

#### Versión en español(castellano) traducida por Victor Barahona

# MVP III

## Último lanzamiento: 3.1.8

### Registro de cambios: 2018/11/24

 - Arreglar la descarga binaria de CouchDB para modificar la propiedad a couchdb
 - Cambio propiedad de los archivos de registro para solucionar el problema de escritura
 - Agregue un script java a index.html para dar a los gráficos un nombre único (solución al problema de almacenamiento en caché de Chrome)

Registro de cambios: 2018/10/2
 - Se agregó CouchDB binario para solucionar problemas de compilación
 - NOTA: Fuxton no está incluido en este binario.

Registro de cambios: 2018/09/24
 - agregue chmod a Startup.sh para que los registros siempre tengan el permiso de escritura correcto

Registro de cambios: 2018/09/21
  - Corregir error tipográfico en Startup.sh

Registro de cambios: 2018/07/05
  - Importante cambio en la estructura JSON de datos, esta debería ser la versión final.
  - Se eliminó la dependencia SI7021 de smbus, reemplazado con python-periferia
  - Se agregó StartUp.py para verificar el estado de las luces al reiniciar
  - Se agregó HeartBeat.py para comprobar que CouchDB está en funcionamiento o se reiniciará automáticamente para reiniciar el sistema.
  - Eliminado el acceso web a CouchDB
  - Cambios de nombre de archivo y método para mantener la coherencia con el estándar de código de Python
  - Añadir registro estándar de Python
  - Se eliminó la recuperación de información geo debido a la desaprobación de la API

## Introducción

Contiene el código e instrucciones necesarias para construir el 'cerebro' del Food Computer MVP. Es principalmente una colección de código en Python que se ejecuta en un Raspberry Pi. Vea los [**foros OpenAg**](http://forum.openag.media.mit.edu/) para obtener información sobre los distinots temas involucrados.

El MVP (producto mínimo viable) es una versión simplificada del Food Computer desarrollado por OpenAg MIT.

## Antes de empezar

 - Sigue las instrucciones para construir un sistema Raspbian (Noobs).
 - Configurar el entorno (ver más abajo). Activa VNC, esta es la forma más fácil de ver múltiples Raspberry Pis sin necesidad de teclados y monitores separados para cada uno. Puedes ver todas los MVPs en tu red local a través de un solo Raspberry, o descargar VNC en un PC y acceder a ellos a través de un PC.

## Construyendo múltiples MVPs

Una vez que has configurado una tarjeta SD, desde el menú principal, use la aplicación de copia de tarjeta SD para replicar el sistema para los otros MVP. Solo asegúrate de ejecutar lo siguiente para que se cree una ID única para cada MVP.
  
  > python /home/pi/MVP/python/Environment.py

## Arquitectura:

El cerebro de MVP se compone en su mayoría de scripts de Python que utilizan cron como el programador horario. Python y cron están integrados en el sistema operativo Raspbian, y la biblioteca Raspberry para manipular los pines GPIO ya está cargada.

El Python es modular, por lo que las adiciones y los cambios se pueden hacer fácilmente sin afectar a todo el sistema.

- Control de programación (cron)
  - Captura de imágenes (webcam.sh)
  - Sensores de registro (LogSensors.py)
  - Encender las luces (LightOn.py)
  - Apagar las luces (LightOff.py)
  - Comprobar la temperatura (termostato.py)
  - Actualizar gráficos e imágenes para la interfaz de usuario (Render.sh)

CouchDB es el principal sistema de almacenamiento de datos y proporcionará una fácil replicación en la nube en el futuro.

Para obtener más información sobre Cron [ver aquí:](https://docs.oracle.com/cd/E23824_01/html/821-1451/sysrescron-24589.html)

## Descripción del hardware:

**Ventilador:**
Hay dos ventiladores, uno para circulación y otro para eliminar el exceso de calor. Estos funcionan con una fuente de 12V, el primero va conectado directamente y el segundo pasará a través de la tarjeta de relés.

**Sensor de temperatura / humedad:**
El sensor SI7021 usa el bus I2C para medir la temperatura y la humedad. Mas información en [Instrucciones](https://learn.adafruit.com/adafruit-si7021-temperature-plus-humidity-sensor/overview) para saber sobre su uso y el cableado.

**Cámara web:**
Se usa una cámara USB estándar para obtener imágenes (aunque la cámara Raspberry Pi es una opción). Consulte [aquí](https://www.raspberrypi.org/documentation/usage/webcams/) para obtener instrucciones.

**Tarjeta de Relés:**
Se utiliza un conjunto de relés controlados por los pines GPIO para encender y apagar las luces (220 V) y el ventilador de extracción (12 V). Suele ser una tarjeta de 4 relés. El diseño estandar del MVP emepla solo 2, los otros dos son de repuesto. En mi caso emplearé un tercero para bombeo de agua y nutrientes, está en fase de desarrollo y pronto quedaré implementado aquí.

## Asignación de Pines:

Consulta el siguiente [diagrama](https://docs.particle.io/datasheets/raspberrypi-datasheet/#pin-out-diagram) para ver la asignación de pines del Raspberry:

El código se ha desarrollado de acuerdo a la convención indicada arriba.

- 3  SDA a SI7021
- 5  SCL a SI7021
- 29 relé de luz (relé # 4)
- 31 (reservado para el relé # 3)
- 33 (reservado para el relé # 2)
- 35 Control de ventilador GPIO13 (relé # 1)

## Instalación

### Asegurarse de tener lo siguiente:

1. Instalación NOOB de Raspbian en Raspberry Pi
2. El sistema Raspbian ha sido configurado (mediante **raspi-config**)
    - para localización (hora, zona horaria)
    - El wifi está establecido y conectado.
    - I2C ha sido habilitado
    - VNC está habilitado para un acceso remoto
    - La resolución de la pantalla está configurada en Modo 16 para una correcta visualización de VNC
    
2. Tarjeta SD 32G para guardar datos
3. Los sensores y el relé están conectados al Pi. Si intentas ejecutar el código sin sensores, se producirá un error (error de E / S, en la función **get_tempC ()**). Esto se trasladará hasta el error de la tarea cron para **LogSensor.py**.

### Pasos a seguir:

Los scripts de construcción forman parte de la documentación. Si quieres construir cosas por ti mismo, sigue y/o modifica los scripts (/ home / pi / MVP / setup).

**El script de instalación principal** no se encuentra en Github, ya que  extrae los archivos de Github y cambia los permisos en **/home/pi/MVP/setup/releaseScript.sh** . Para crearlo debes seguir los siguientes pasos:

- Desde tu Raspberry Pi, corta y pega las siguientes líneas de código en un archivo de texto y asígnale el nombre **buildScript.sh** . 
- Luego, cambia los permisos para hacerlo ejecutable (desde un Administrador de archivos, haz clic derecho en el archivo y selecciona "Propiedades". 
- En la pestaña "Permisos", debajo de "Ejecutar", selecciona "Cualquiera" y luego haz clic en "Aceptar"). 
- Luego abre una ventana de shell-terminal y escribe **ruta de acceso a su archivo/buildScript.sh** .

**A continuación el texto del script que debes copiar/pegar:**


```
#!/bin/sh

# Part 1
# Semi-generic script to get and install github archive
# Author: Howard Webb
# Date: 10/02/2018

# This script assumes you are running on your Raspberry Pi with (Stretch) Raspbian installed.
# Internet is connected
# You have configured the local environment (keyboard, timezone)
# You have adjusted the Pi Preferences (Configuration)
#   Enable the camera interface
#   Enable I2C
#   Enable VNC
#   Set screen resolution to Mode 16
#   Optionally (suggested) enable SSH, VCN and 1-Wire

# Get the release from Github
# Extract it to the proper directory
# Make the build script executable
# Run the release specific build script

###### Declarations #######################


RED='\033[31;47m'   # Define red text
NC='\033[0m'        # Define default text

EXTRACT=/home/pi/unpack    # Working directory for download and unzipping
TARGET=/home/pi/MVP       # Location for MVP
RELEASE=mvp             # Package (repository) to download 
VERSION=v3.1.8         # github version to work with
ZIP_DIR=3.1.8
GITHUB=https://github.com/futureag/$RELEASE/archive/$VERSION.zip    # Address of Github archive

echo $EXTRACT
echo $TARGET
echo $RELEASE
echo $GITHUB

###### Error handling function ###################

PROGNAME=$(basename $0)

error_exit()
{
	echo ${RED} $(date +"%D %T") "${PROGNAME}: ${1:="Unknown Error"}" ${NC} 1>&2
	tput sgr0
	exit 1
}

####### Start Build ######################
echo "##### Update Existing System #####"
sudo apt-get update

echo "##### Starting to build directories #####"
# Build target directory
mkdir -p $TARGET || error_exit "Failure to build target directory"
echo $(date +"%D %T") $TARGET" built"

echo "##### Starting download of MVP from Github #####"
# Download MVP from GitHub and install
# Build extraction directory
mkdir -p $EXTRACT || error_exit "Failure to build working directory"
echo $(date +"%D %T") "Directory built"
cd $EXTRACT

# Download from Github
wget -N $GITHUB -O $VERSION.zip || error_exit "Failure to download zip file"
echo $(date +"%D %T") "MVP Github downloaded"

cd $EXTRACT

# Unzip the files, overwrite older existing files without prompting
unzip -uo $EXTRACT/$VERSION.zip || error_exit "Failure unzipping file"
echo $(date +"%D %T") "MVP unzipped"

cd $EXTRACT/$RELEASE-$ZIP_DIR/MVP || error_exit "Failure moving to "$EXTRACT/$RELEASE"-"$ZIP_DIR

# Move to proper directory
mv * $TARGET
echo $(date +"%D %T") "MVP moved"

########################################
echo "##### Relsease Specific Build #####"
# Complete the release specific build - this is the CouchDB extract

# Set permissions on script
chmod +x $TARGET/setup/releaseScript.sh || error_exit "Failure setting permissions on release script (check file exists in MVP/scripts)"
echo $(date +"%D %T") "Run permissions set"

# Run script in download
bash $TARGET/setup/releaseScript.sh || error_exit "Failure running release specific script"

# Clean up temporary extraction directory
rm -r $EXTRACT
echo $(date +"%D %T") $EXTRACT" removed"

echo $(date +"%D %T") "Install Complete"

```

## Instalación paso a paso

Los siguientes scripts (en / home / pi / MVP / scripts) pueden ejecutarse por separado y en secuencia si se encuentran errores. Busca dentro de los scripts para comandos individuales.

- **releaseScript.sh** llama a los siguientes scripts:
- **releaseScript_DB.sh** instala el código CouchDB.
- **releaseScript_Local.sh** construye bibliotecas, inicia la base de datos e inicializa procesos.
- **releaseScript_Test.sh** llama a **Validate.sh** para probar el sistema
- **releaseScript_Final.sh** configura el inicio y carga cron

## Desarrollo futuro (con prioridad):

- Agregar un **watchdog** perro guardián al Raspberry.
- Corregir las notificaciones de correo electrónico cron.

## Desarrollo futuro (sin prioridad):

- Interfaz GUI para configurar variables persistentes (podría ser local)
- Agregar una bomba para cuando tenga que estar ausente por un tiempo y se tenga que rellenar el depósito.
- Control de luz para LEDs controlables.

