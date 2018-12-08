# Food-Computer-a-la-guipuzcoana

## El contexto

El concepto surgió al visitar Fukushima y ver que nunca se podría cultivar allí debido a la contaminación de las tierras. De ahí se vió que hay muchos contextos en los que el cultivo resulta díficil y una tecnología de este tipo podría ayudar.

**El propósito es garantizar el cultivo de aproximadamente el 30% de alimentos para consumo humano de forma local**, sean cuales sean las condiciones climáticas o del suelo. De este modo se podría cultivar en lugares como Fukushima (con tierras contaminadas radiactivamente), la Antártida, o incluso en el espacio. Y sin ir tan lejos, se pueden utilizar azoteas y espacios urbanos actualmente en desuso. El aporte directo y local de estos alimentos, normalmente muy perecederos, permite enriquecer la dieta de las personas que los utilizan.

Hay que tomar en cuenta el incremento de la población mundial que alcanzará los 9000 millones de personas en torno a 2040. Y al tiempo la destrucción y erosión de tierras de cultivo. Habrá que administrar las tierras de tal forma que los campos queden para cultivos extensivos o que requieren espacio, mientras que los cultivos mas pequeños y de ciclo corto se puedn cultivar localmente mediante estas técnicas, incluso de forma vertical.

## Diseño y detalles constructivos

El concepto FC utiliza el cultivo hidropónico, concretamente **DWC (Deep Water Culture)** que permite cultivar verduras de ensalada de pequeño tamaño y ciclo corto. El MIT está ensayando ahora otros cultivos que emplean técnicas alternativas e incluso cultivo con suelo para plantones de avellano, etc.

Aunque el diseño original es cerrado y no recibe luz exterior, nada nos impide hacerlo parcialmente transparente para que tenga una iluminación mixta. Es así como estoy diseñando mis modelos, de esta manera reduciremos el consumo eléctrico del equipo, utilizando la luz artificial solo cuando hace falta.

Una vez definidos una serie de características del equipo veamos las escalas de tamaño a las que el MIT se ha propuesto trabajar:

- **Food Computer** Es la unidad mas pequeña, de un tamaño de 80x50x80cm aproximadamente, para ser colocada sobre un escritorio. Capacidad de cultivo muy reducida y solo para propósitos de investigación y educación.
- **Food Server** Tiene el tamaño de un contenedor marítimo y la capacidad de producir alimentos para una familia o bien para abastecer a un pequeño restaurante
- **Food Data Center** Tiene el tamaño de una fábrica, el tamño aquí es indeterminada ya que a esta escala no se ha desarrollado todavía.

En este contexto tenemos también el concepto de **Climate Recipe o Receta Climática**: Es un fichero de formato plano que contiene los datos necesarios para que un Food Computer pueda reproducir un determinado clima apto para un cultivo. Estandarizar este formato permitiría que se puedan homologar experimentos de cultivo en todo el mundo.

El equipo tiene los siguientes **sensores**:
- Temperatura y humedad del aire
- Temperatura del agua de cultivo
- Niveles de CO2 en el aire
- EC o conductividad del agua (indicativo de concentración de sales de cultivo)
- PH , indicativa de la capacidad de absorción de nutrientes disueltos
- imágenes mediante cámara, se hace un timelapse que con técnicas de **machine learning** permite analizar el crecimiento de la planta y si este es correcto.

Y por otro lado, tiene los siguientes **actuadores:**
- **water chiller** para controlar la temperatura del agua de cultivo: hay que mantenerla lo bastante baja para mantener altos los niveles de oxígeno disuelto asi como la proliferación de hongos y/o bacterias en el agua. Al tiempo que la temperatura del aire puede ser diferente por completo.
- Bomba de aire para crear burbujas y oxigenar el agua del cultivo
- Ventilador para mantener corrientes de aire en la cámara y evitar condensaciones o puntos donde proliferen los hongos o plagas.
- Ventilador exterior que ventea la cámara si esta alcanza una temperatura inadecuada, o para equilibrar niveles de CO2. Puede emplearse un módulo Peltier para calentar o enfriar el aire.
- Iluminación LED : se regula su tiempo de encendido. En los últimos desarrollos del MIT se está controlando también el espectro luminoso(mayor o menor proporción de rojo, IR etc)
- Bombas peristálticas que inyectan nutrientes al agua, asi como reguladores del ph.

**Las versiones EDU y MVP** son mas sencillas y disponen de menos sensores y actuadores para facilitar la construcción, uso y mantenimiento. Asimismo tienen un precio bastante mas asequible que permitirá su rápida extensión.

## Un poco de historia...

**El proyecto es liderado por Caleb Harper del MIT.**

**El proyecto Food Computer en su forma original desarrolló dos versiones, la V1 y la V2**, bastante similares entre si. Era un equipo caro con un coste de materiales en torno a los **2500$**, al que había que añadir la mano de obra. Era complicado de construir y con muchos problemas tanto hardware como software. Contaba con mucha instrumentación y estaba orientado a centros de I+D.

La comunidad tenía dificultades para construirlo. Debido a la imposibilidad de dar soporte a los problemas técnicos por parte de **Open AG (MIT)** y a que ellos estaban desarrollando otros proyectos, **finalmente decidieron parar  su desarrollo en Marzo de 2018**. La información sigue estando disponible en Internet pero ya no cuenta con soporte.

Ellos se han centrado en el desarrollo del **Food Server** , del tamaño de un contenedor marítimo, **para cultivos de mayor cantidad o incluso cultivos experimentales como el algodón o el avellano.**

Han estado en contacto con la comunidad, particularmente con la educativa, ya que desean que esta tecnología llegue a la escuelas. Y finalmente han sacado un  modelo nuevo llamado **PFC EDU 3.0** Este es mas pequeño y sencillo, costando en torno a los 500$. Ha  salido en Noviembre 2018 y de momento las unidades que se han fabricado están en manos exclusivas del MIT, que ha liberado los planos para que la comunidad pueda hacerlo, aunque es pronto todavía. 

Quizás la mayor dificultad es la fabricación de la placa base, de unos 30x30cm, donde insertará una placa **Beaglebone Black Wireless** en sustitución de la **Raspberry Pi 3** .

Desde el año 2017, en la comunidad se ha ido construyendo una alternativa llamada **MVP**, similar al **EDU** y que está en la banda de los 300$. Es la base sobre la que se ha construido todo este proyecto ya que reune la mayoría de los requisitos necesarios.

















