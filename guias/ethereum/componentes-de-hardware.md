# Componentes de Hardware

{% embed url="https://mirror.xyz/seedlatam.eth/c7sL9rh1mIy6MLygNdjB15HvsTmmqi-NoYDnp-48z1M" %}

En el gran ecosistema de la tecnología blockchain los nodos juegan un papel fundamental en la seguridad y la descentralización de las redes. Estos nodos actúan como puntos de conexión que validan y transmiten transacciones, manteniendo la integridad y la confiabilidad del sistema en su conjunto. Si bien existen numerosas opciones de software para ejecutar un nodo, ¿alguna vez te preguntaste cómo sería construir un nodo desde cero utilizando componentes de hardware? En este artículo, nos sumergiremos en el fascinante mundo de la construcción de nodos físicos, explorando los fundamentos, los desafíos y los pasos prácticos necesarios para crear tu propio nodo.¡Prepárate para entrar en el emocionante viaje de construir tu propia infraestructura descentralizada!

Primero, debemos tener en cuenta que tipo de nodo queremos tener, en este caso vamos a cubrir la forma más económica de poder crear un nodo (y divertida IMO): armar componente por componente! para esto, debemos tener en cuenta los requisitos recomendados:

Especificaciones recomendadas

* 16 - 32 GB de RAM
* SSD de 2 TB (preferiblemente DDR4)
* CPU de cuatro núcleos (o doble núcleo hiper-procesado)
* Conexión a internet por cable
* Sistema operativo Linux

### **Selección de Componentes:** <a href="#heading-seleccion-de-componentes" id="heading-seleccion-de-componentes"></a>

Para construir un nodo desde cero, es crucial seleccionar cuidadosamente cada componente para garantizar un rendimiento óptimo y una estabilidad confiable. Aquí hay una guía paso a paso para elegir los componentes principales de hardware:

#### **Placa Base (Motherboard):** <a href="#heading-placa-base-motherboard" id="heading-placa-base-motherboard"></a>

La placa base es el cimiento de cualquier sistema informático. Para un nodo, busca una placa que tenga el “socket” compatible con el tipo de procesador que planeas utilizar y tenga suficientes puertos de conexión para tus dispositivos de almacenamiento y periféricos. Pero, ¿Qué es el socket? El socket es el punto de conexión entre el procesador y la placa base, permite que el procesador se integre eléctrica y mecánicamente con el resto del sistema.

<figure><img src="https://images.mirror-media.xyz/publication-images/csNewrz8YhcT_h20MYKge.png" alt="" height="439" width="740"><figcaption></figcaption></figure>

También debemos tener en cuenta que si vamos a utilizar un disco NVMe SSD la motherboard debe tener la entrada para que podamos conectarlo:

<figure><img src="https://images.mirror-media.xyz/publication-images/OgNpgcyTafvQ5aAeGP9pT.png" alt="" height="335" width="667"><figcaption></figcaption></figure>

#### **Procesador (CPU)** <a href="#heading-procesador-cpu" id="heading-procesador-cpu"></a>

La elección del procesador depende en gran medida de las necesidades de rendimiento de tu nodo. Se recomienda un procesador potente con múltiples núcleos y capacidad de procesamiento multitarea. Se recomienda fuertemente una CPU que obtenga una puntuación de al menos 6000 o mejor en Passmark. Para los tiempos de sincronización inicial, el rendimiento de un solo hilo es mejor que tener muchos núcleos. A la hora de identificar la generación de procesadores Intel Core nos guiaremos de la siguiente imagen:

<figure><img src="https://images.mirror-media.xyz/publication-images/7O42JlYgZHTc0cuULEL3F.png" alt="" height="179" width="671"><figcaption></figcaption></figure>

**Ejemplo de 13a Generación**

* Intel® Core™ procesador i7-13 700K es de 13a Generación porque el número **13**aparece después de i7.

#### **Memoria RAM** <a href="#heading-memoria-ram" id="heading-memoria-ram"></a>

La memoria RAM es esencial para el rendimiento general del sistema y la capacidad de respuesta. Asegúrate de elegir suficiente RAM para manejar la carga de trabajo del nodo y cualquier otra tarea que puedas ejecutar simultáneamente, se recomienda un mínimo de 16 GB.

#### **Almacenamiento (SSD/NVMe)** <a href="#heading-almacenamiento-ssdnvme" id="heading-almacenamiento-ssdnvme"></a>

El almacenamiento rápido y confiable es fundamental para garantizar tiempos de carga rápidos y una operación sin problemas. Tanto los SSD tradicionales como los discos NVMe ofrecen excelentes velocidades de lectura/escritura, lo que los hace ideales para almacenar la cadena de bloques y otros datos del nodo. Se recomienda un disco de 2 TB o más para ayudar con la longevidad del mismo.

#### **Tarjeta Gráfica (GPU)** <a href="#heading-tarjeta-grafica-gpu" id="heading-tarjeta-grafica-gpu"></a>

Si bien los nodos generalmente no requieren una tarjeta gráfica dedicada, será o no será necesaria dependiendo si nuestro procesador tiene gráfica integrada, si no lo tiene, vamos a necesitar una GPU para poder dar imagen a nuestro Nodo y asi hacer las configuraciones del software, con una GPU de 1GB ya es suficiente, ya que solamente es para brindar imagen a nuestro Nodo. Si el procesador tiene gráfica integrada, no debemos preocuparnos ya que conectaremos la entrada de imagen (por ejemplo: HDMI) directo a la motherboard.

#### **Fuente de Alimentación (PSU)** <a href="#heading-fuente-de-alimentacion-psu" id="heading-fuente-de-alimentacion-psu"></a>

Una fuente de alimentación confiable y de alta calidad es esencial para garantizar la estabilidad del sistema y proteger los componentes de posibles daños por sobretensión o fluctuaciones de energía. Es importante que la fuente tenga una certificación de eficiencia como las del siguiente cuadro, donde se recomienda por lo menos una eficiencia de 80 plus bronze.

<figure><img src="https://images.mirror-media.xyz/publication-images/wCElRKapIGWk0nHqh0rtZ.jpeg" alt="" height="116" width="433"><figcaption></figcaption></figure>

La cantidad de watts dependerá de todos los componentes de nuestro Nodo, debemos consultar las especificaciones de la Motherboard para ver su consumo y tener un aproximado del tipo de fuente que debemos adquirir.

Un tip para generar más estabilidad a nuestro nodo es conectarlo a un estabilizador de tensión.

Es recomendable comprar una fuente más grande de la cantidad de watts que necesitamos para ir sobrados de energía, ya que no hay mucha diferencia de precios para el tipo de fuente que vas a necesitar, los componentes más caros económicamente serán el CPU y la memoria SSD, donde si seguimos los requisitos recomendados nos ahorraremos problemas futuros.

Una vez que hayas seleccionado tus componentes, el siguiente paso es ensamblarlos cuidadosamente en tu gabinete (verificá que el tamaño sea compatible con tu Motherboard!) y comenzar a configurar el software.
