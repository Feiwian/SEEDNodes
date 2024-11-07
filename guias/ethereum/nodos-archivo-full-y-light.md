# Nodos: Archivo, Full y Light

{% embed url="https://mirror.xyz/seedlatam.eth/lxUIkT6NrZBl5Hbdcdy2Opnt0tFOGrZRCeYaFwWKbSI" %}

### **Introducción** <a href="#heading-introduccion" id="heading-introduccion"></a>

En primer lugar, vamos a explicar puntos claves como: qué son los _nodos_, por qué son importantes y qué papel fundamental desempeñan en Ethereum.

Los Nodos son computadoras que corren un software específico (_cliente_) que permite la conexión con la red de Ethereum. Dando acceso de esta forma a la conexión con otros nodos y en consecuencia a la formación de una red. \
El rol fundamental que cumplen es el registro, validación de las transacciones y participando en el consenso de la red.

Los nodos son fundamentales para la salud, la seguridad y la resiliencia operativa de la red. Cuantos más nodos haya, mayor diversidad y robustez tendrá la red.

Esto es esencial para lograr descentralización, que es el objetivo de toda red para crear un sistema confiable y resistente a la censura.

Previo a la ejecución de un nodo es necesario saber que la red de Ethereum se compone de diversos tipos de nodos, específicamente 3, cada uno con sus propias características y requisitos, cumpliendo un rol único en el mantenimiento de la cadena de bloques. Estos son los Nodo Archivo, Nodo Completo y Nodo Ligero.

### **Full Node** <a href="#heading-nodo-completo" id="heading-nodo-completo"></a>

Los nodos completos son los nodos más completos y confiables de la red. Realizan una validación completa de cada bloque de la cadena de bloques, incluyendo la descarga y verificación del cuerpo del bloque y los datos de estado de cada una.

Existen dos tipos distintos de nodos completos:

* **Nodos completos desde el bloque génesis:** que son los que comienzan desde el bloque génesis contando con la verificación de todos los bloques y verifican, por otro lado, los que inician su verificación en un bloque más reciente en el cual se confía como válido (por ejemplo, la 'snap sync' de Geth).
* **Nodos completos con sincronización rápida:** Independientemente de dónde comience la verificación, los nodos completos solo mantienen una copia local de datos relativamente recientes (por lo general, los 128 bloques más recientes), lo que permite eliminar datos más antiguos para ahorrar espacio del disco. Los datos más antiguos que no encuentren en los últimos 128 bloques se pueden regenerar mediante el minado (computación) aunque puede ser un proceso de extrema demanda al alejarnos progresivamente del último bloque.

### **Nodo Archivo** <a href="#heading-nodo-archivo" id="heading-nodo-archivo"></a>

Los Nodos Archivo son una variante de los Nodos Completos que verifican cada bloque desde el génesis y nunca eliminan ninguno de los datos descargados.

¿Qué quiere decir esto? Quiere decir que los nodos archivos son un archivo histórico de estados. \
Al momento de compararlo con el Nodo Completo, el mayor beneficio de este tipo de nodo es la rápida accesibilidad a datos históricos sobre los estados de cada bloque debido a que se encuentran almacenados todos los estados desde el principio y no únicamente de los últimos 128 bloques. Por ejemplo, es beneficioso y práctico en el caso de precisar acceder a balances de billeteras, código de contratos o datos acerca del consenso.\
\
Consecuentemente, los datos representan unidades de terabytes, lo que resulta menos atractivo para el usuario promedio, y de extrema utilidad para servicios como exploradores de bloques, proveedores de billeteras y análisis de cadena.

### Light <a href="#heading-nodo-ligero" id="heading-nodo-ligero"></a>

Los nodos ligeros son los nodos más sencillos y accesibles de la red.\
\
En lugar de descargar cada bloque, los nodos ligeros solo descargan encabezados de bloque. Estos encabezados contienen información resumida sobre el contenido de los bloques, una estampa del tiempo _time stamp_ y el _hash_ del bloque previo. Cualquier otra información que el nodo ligero requiera se solicita a un nodo completo. \
\
Los Nodos Ligeros se encargan de la validación de datos de la cadena de bloques al no poder participar de la validación de bloques ni de la ejecución de contratos inteligentes. No pueden verificar de manera independiente los datos que reciben contra las raíces de estado en los encabezados de bloque. \
\
La diferencia con los Nodos Completos radica al no participar en el consenso. Es decir, no pueden ser mineros/validadores, no obstante pueden acceder a la cadena de bloques de Ethereum con la misma funcionalidad y garantías de seguridad que un Nodo Completo. Otra diferencia con estos mismos nodos es que no efectúan una verificación completa de cada bloque. Consecuentemente, en el caso de que únicamente haya nodos ligeros en una red, haría a la red susceptible de ataques de validadores. \
\
Los nodos ligeros permiten a los usuarios participar en la red Ethereum sin la necesidad de hardware potente o un ancho de banda elevado indispensable para ejecutar nodos completos. Eventualmente, los nodos ligeros podrán ser ejecutados en teléfonos móviles o dispositivos integrados.

**Ahora que sabemos que los nodos completos validan cada bloque, los nodos archivo almacenan todos los bloques y los nodos ligeros solo descargan los encabezados de bloque, es importante conocer los requerimientos computacionales necesarios para ejecutarlos.**

### Requerimientos de Hardware: <a href="#heading-requerimientos-de-hardware" id="heading-requerimientos-de-hardware"></a>

**Nodo Completo**

* Una CPU rápida con 4 o más núcleos.
* 16 GB o más de RAM.
* Una unidad SSD rápida con al menos 1 TB de espacio (la capacidad de almacenamiento aumentará con el tiempo).
* Ancho de banda de 25 MBit/s.

**Nodo Archivo**

* Una CPU rápida con 4 o más núcleos.
* 16 GB o más de RAM.
* El almacenamiento variará según el software del cliente (a partir de marzo de 2023, el modo de archivo en Geth ocupa aproximadamente 13.5 TB, y Erigon ocupa alrededor de 2 TB (se recomienda contar con 3 TB)).
* Ancho de banda de 25 MBit/s.

**Nodo Ligero**

* 400 MB de almacenamiento.
* Acceso a Internet.

### Conclusión <a href="#heading-conclusion" id="heading-conclusion"></a>

Los nodos en la red Ethereum desempeñan roles cruciales que van desde la validación de bloques hasta la preservación de datos históricos de la red. La diversidad de nodos, como los completos, ligeros y de archivo, permiten una mayor versatilidad, abriendo las puertas a la participación tanto de usuarios con recursos limitados hasta aquellos que requieren acceso a datos históricos detallados. Es indispensable contemplar previo a correr un nodo, cuál es el tipo de nodo que queremos correr y los recursos con los que contamos para tener una mejor experiencia al momento de ejecutar uno.
