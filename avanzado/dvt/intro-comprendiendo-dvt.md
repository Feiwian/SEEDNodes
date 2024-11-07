---
description: >-
  DVT (Distributed Validator Technologies) es una tecnología con el objetivo de
  mejorar el staking en Ethereum.
---

# Intro - Comprendiendo DVT

{% stepper %}
{% step %}
**Introducción**

Utilizaremos como punto de partida los problemas del staking tradicional y propondremos algunas soluciones.
{% endstep %}

{% step %}
**Implementación en los nodos de Ethereum**

Mencionaremos de forma superficial cómo se podrían implementar las soluciones del punto anterior.
{% endstep %}

{% step %}
**Funcionamiento de DVT**

Explicaremos un poco más en detalle los protocolos necesarios para aplicar DVT en la práctica.
{% endstep %}

{% step %}
**Diferencias entre implementaciones**

Abordaremos las principales diferencias entre OBOL y SSV. También mencionaremos otras implementaciones que actualmente están en la industria.
{% endstep %}

{% step %}
**Propuesta de LIDO**
{% endstep %}

{% step %}
**Conclusiones**
{% endstep %}
{% endstepper %}

## Introducción <a href="#block-b210c51841ca481d9bd9dea38b3d005b" id="block-b210c51841ca481d9bd9dea38b3d005b"></a>

#### _Staking en Ethereum_ <a href="#block-89a6eed49f024c8a86d524eb272d1e09" id="block-89a6eed49f024c8a86d524eb272d1e09"></a>

Básicamente, implica que los validadores bloqueen 32 ETH dentro del protocolo y realicen ciertas tareas. Estas tareas consisten principalmente en producir bloques y atestaciones en momentos específicos. La razón para bloquear el ETH es brindarle al protocolo la herramienta para poder penalizar a los validadores si hacen algo incorrecto.

#### _Riesgos_ <a href="#block-9c3dc0959d6248e6a4dec4ae71560d87" id="block-9c3dc0959d6248e6a4dec4ae71560d87"></a>

Existen diferentes tipos de penalizaciones. Si un nodo está inactivo, solo sufre una pequeña penalización. Si se comporta de manera maliciosa, como firmar en dos cadenas diferentes a la vez, se le da una penalización mayor llamada "slashing".

Sumado a esto, existe el riesgo debido a la gestión de las claves. Este riesgo abarca tanto el robo de la clave como el riesgo que se corre al ceder la clave a un tercero. Al confiar en una sola persona o entidad las claves del validador, se corre el riesgo de una mala configuración o prácticas de seguridad deficientes.

#### _Cómo mitigar estos riesgos_ <a href="#block-d622c2ecb9fa41c0bf7ef4740ec629d5" id="block-d622c2ecb9fa41c0bf7ef4740ec629d5"></a>

Una idea es dividir la clave en varias partes de modo que para controlarla o robarla se necesite controlar múltiples claves distribuidas.

<figure><img src="https://seednode.super.site/_next/image?url=https%3A%2F%2Fassets.super.so%2F232cf1f1-52e8-4e0c-9e18-6bf1f51649a5%2Fimages%2F043dcbd6-28a6-4451-b314-56f0fb839215.png&#x26;w=3840&#x26;q=90" alt=""><figcaption></figcaption></figure>

De esta forma, con un modelo de combinación de firmas, se podría firmar.

<figure><img src="https://seednode.super.site/_next/image?url=https%3A%2F%2Fassets.super.so%2F232cf1f1-52e8-4e0c-9e18-6bf1f51649a5%2Fimages%2Fe3dc13a6-3b04-4ec1-bb41-178955d67057.png&#x26;w=3840&#x26;q=90" alt=""><figcaption></figcaption></figure>

Por otro lado, para protegerse contra las fallas propias del nodo también se pueden implementar varias estrategias. Un nodo fuera de servicio puede ser causado por cortes de energía, fallos de red, fallos de hardware y caídas de software. Para protegerse contra estas fallas se podrían utilizar sistemas redundantes, ejecutando el mismo nodo en múltiples ubicaciones, lo que aseguraría que si una instancia falla las otras puedan mantener el funcionamiento del validador. Esto implica que todas las instancias redundantes deben estar sincronizadas y en consenso para evitar votaciones contradictorias que podrían resultar en una penalización severa.

Entonces, este tipo de solución nos trae un nuevo riesgo: las fallas bizantinas. La protección contra estas fallas se basa en lograr el consenso entre múltiples instancias asegurándose de que ninguna instancia honesta tome decisiones unilaterales.

💡No confundir el consenso de Ethereum con lo que estamos mencionando en este apartado. Aquí hacemos hincapié en el 'consenso interno' que deben cumplir los validadores redundantes para funcionar de forma honesta entre ellos.

## **Implementación en los nodos de Ethereum** <a href="#block-189fdba2a33c412cae2d350f5a7c5b51" id="block-189fdba2a33c412cae2d350f5a7c5b51"></a>

En la arquitectura actual de un validador tenemos dos componentes: el nodo completo (cliente de consenso + cliente de ejecución) y el cliente del validador. El nodo completo se encarga de la red P2P estando directamente expuesto a la red de Ethereum. El cliente del validador, en cierto sentido, es más privado ya que maneja las claves del validador, firma atestaciones y bloques, y solo está conectado al nodo completo para obtener atestaciones y bloques no firmados.

<figure><img src="https://seednode.super.site/_next/image?url=https%3A%2F%2Fassets.super.so%2F232cf1f1-52e8-4e0c-9e18-6bf1f51649a5%2Fimages%2F1e86aac6-0238-44f5-ba40-c8ae1e158907.png&#x26;w=3840&#x26;q=90" alt=""><figcaption></figcaption></figure>

Para descentralizar esta arquitectura lo primero que planteamos fue dividir la clave de staking en varias partes y conectarlas al nodo completo. Esto mejora la seguridad de las claves pero todavía hay un punto único de falla en el nodo completo.

<figure><img src="https://seednode.super.site/_next/image?url=https%3A%2F%2Fassets.super.so%2F232cf1f1-52e8-4e0c-9e18-6bf1f51649a5%2Fimages%2F0225a85a-4d78-499f-a3c7-f78cf71063f2.png&#x26;w=3840&#x26;q=90" alt=""><figcaption></figcaption></figure>

Luego mencionamos que la solución a esto es crear instancias redundantes del nodo, pero cuyo problema es el consenso entre ellos. Una de las soluciones que implementa DVT es insertar un middleware entre las partes redundantes del nodo completo y las piezas que contienen las claves del validador. Este middleware, llamado cliente del validador distribuido (DVC), asegura que todas las instancias firmen de manera consensuada.

💡Todas estas instancias redundantes de nodo se conocen como “cluster”. En nuestro ejemplo el cluster es de 5 operadores de nodo independientes.

<figure><img src="https://seednode.super.site/_next/image?url=https%3A%2F%2Fassets.super.so%2F232cf1f1-52e8-4e0c-9e18-6bf1f51649a5%2Fimages%2F076e0cc8-78dc-48d5-9811-5ab8da3da6ef.png&#x26;w=3840&#x26;q=90" alt=""><figcaption></figcaption></figure>

La red Ethereum interpreta estas firmas como si provinieran de un único validador, sin necesidad de ningún cambio en su protocolo.

<figure><img src="https://seednode.super.site/_next/image?url=https%3A%2F%2Fassets.super.so%2F232cf1f1-52e8-4e0c-9e18-6bf1f51649a5%2Fimages%2Ffe7affe8-2b46-47c4-a1c5-30886b5434ba.png&#x26;w=3840&#x26;q=90" alt=""><figcaption></figcaption></figure>

Al distribuir tanto las funciones operativas como las claves de firma DVT se reduce significativamente los puntos únicos de falla, haciendo el conjunto de validadores más robusto y resistente contra fallas de nodos individuales y posibles ataques. Esta tecnología permite que los grupos de stakers agrupen eficientemente recursos financieros y computacionales, asegurando un entorno de staking estable y seguro.

## Funcionamiento de DVT <a href="#block-a8ac318d72134ae6a4cd737fa4058512" id="block-a8ac318d72134ae6a4cd737fa4058512"></a>

Ya vistas las generalidades de DVT, pasemos a indagar mas en detalle cómo es su implementación. Para esto analizaremos por separada las principales partes que lo componen :

* Firmas BLS:
  * Compartidas como secretos de Shamir
  * Generación Distribuida de Claves (DKG).
  * Diferenciando SSS y DGK
* Threshold signatures (podríamos traducirlo como umbral de firmas)
* Capa de consenso DVT BFT.

### Firmas BLS <a href="#block-2c4f61f712c44ba3be271dba56696e44" id="block-2c4f61f712c44ba3be271dba56696e44"></a>

Los validadores utilizan claves BLS. Este tipo de firmas permite una agregación muy eficiente, esto es en palabras sencillas, las firmas BLS permiten combinar múltiples firmas como si fuera una sola.

En DVT la firma del validador es en realidad la firma BLS combinada de cada operador en el cluster.

💡Nosotros mencionaremos algunas de las primitivas más importantes, aunque hay otras y pueden aparecer nuevas. Cada implementación de DVT podrá recurrir a diferentes métodos para lograr el mismo objetivo.

#### Firmas BLS compartidas como secretos de Shamir \[SSS] <a href="#block-03c6fa0ed5bd4459adc67b249d21706f" id="block-03c6fa0ed5bd4459adc67b249d21706f"></a>

Es un método criptográfico donde un secreto (clave privada del validador) se divide en partes y se reparte a diferentes personas (operadores de nodo del clúster). Con este método un individuo solo no puede regenerar el secreto, sino que deberá contar con la colaboración de los otros miembros.

SSS tiene algunas variantes que solucionan diferentes aspectos específicos. Por ejemplo, existe un problema que se deduce de forma inmediata: los que reciben las partes no saben si lo que tienen es una parte válida o no. Quizás recibieron una parte corrupta que no sirve para firmar. La variante VSSS permite a los participantes verificar que no se están utilizando particiones maliciosas.

<figure><img src="https://seednode.super.site/_next/image?url=https%3A%2F%2Fassets.super.so%2F232cf1f1-52e8-4e0c-9e18-6bf1f51649a5%2Fimages%2F1953a7a2-730c-4c2d-83e2-af933d954e8d.png&#x26;w=3840&#x26;q=90" alt=""><figcaption></figcaption></figure>

Por otro lado, aunque ninguno de los usuarios que tienen una parte de la clave conoce la clave en su totalidad, esta sí fue vista por alguien en el momento de su creación. Es en la génesis del secreto donde hay un punto único de falla.

Otra opción que se utiliza en DVT para solucionar este inconveniente es DKG.

#### **Generación Distribuida de Claves (DKG)** <a href="#block-2d61e37787b64adf9e3af628da5cc66e" id="block-2d61e37787b64adf9e3af628da5cc66e"></a>

A diferencia del caso SSS, en lugar de que una entidad central de confianza produzca una clave privada, la divida y la distribuya, se realiza la ceremonia de Generación de Claves Distribuidas entre los diferentes participantes que van a ser parte del clúster de validación. De esta forma nunca se construye la clave privada completa en ningún momento. Cada representante que forma parte de esta ceremonia posee solo una porción de la clave privada, lo que impide que una sola parte tenga acceso o control directo sobre la clave privada completa.

Para ser claros, en un esquema DKG, las “n” partes participan en un protocolo P2P, y al final de ese protocolo se crea lo siguiente:

* Claves secretas y públicas aleatorias sk, pk.
* Particiones sk\_1,..., sk\_n de sk, donde cada parte posee una de las particiones y ninguna parte conoce sk.

Esto es muy similar a SSS, excepto que nunca hay un único punto de falla. Esto hace que DKG sea un algoritmo de Computación de Múltiples Partes (MPC).

#### Diferenciando SSS y DGK <a href="#block-8353f482cc2646a687c6acd5bb783020" id="block-8353f482cc2646a687c6acd5bb783020"></a>

Las implementaciones de DVT utilizan uno de estos dos mecanismos:

**DVT utilizando SSS:** una entidad central que posee 32ETH crea claves de firma (sk, pk) y claves de retiro de la misma manera que se explicó en nuestros artículos anteriores de home-staker.

Luego, la entidad utiliza un programa que ejecuta un SSS para distribuir de manera segura las particiones de la clave secreta de firma sk entre el cluster.

**DVT utilizando DKG:** En este caso, ninguna entidad central distribuye la clave secreta de firma del validador. En su lugar, el cluster se reúne y ejecuta el protocolo DKG. Como resultado, se crean una clave secreta y una clave pública sk, pk, en n particiones sk\_1,..., sk\_n de sk.

Ahora los miembros del comité interactúan con el launchpad Ethereum para depositar 32ETH y registrar las claves públicas del validador. Cabe destacar que la clave secreta nunca necesita ser registrada en ningún lugar, por lo que nadie dentro o fuera del comité necesita conocer sk para "registrar" el nuevo validador.

El resultado de ambos enfoques es similar: todos los nodos de un comité terminan poseyendo particiones de la clave secreta de firma de un validador de Ethereum.

### Threshold signatures <a href="#block-fee648914b8c40e7b332a2e02a180ec4" id="block-fee648914b8c40e7b332a2e02a180ec4"></a>

Los esquemas mencionados pueden ser ampliados para convertirse en esquemas thereshold (de umbral). Esto es, un esquema donde ya no necesitamos las N firmas de los N firmantes, sino solo un subconjunto M de N. De esta manera, si alguien pierde sus claves o está offline, aún se puede firmar.

Con la siguientes imágenes se puede ver claramente el concepto. Imaginemos que el secreto es la intersección entre línea recta y el eje Y \[El punto (0 , 1)]:

<figure><img src="https://seednode.super.site/_next/image?url=https%3A%2F%2Fassets.super.so%2F232cf1f1-52e8-4e0c-9e18-6bf1f51649a5%2Fimages%2F02240952-3759-4a67-bada-b64a741bec4d.png&#x26;w=3840&#x26;q=90" alt=""><figcaption></figcaption></figure>

Para dividir el secreto le damos las coordeandas de los puntos A, B y C a tres personas diferentes. Ninguna de estas personas puede regenerar el secreto original por si sola. En este ejemplo se necesitan minimo dos personas para volver a regenerar el secreto a partir de los puntos. Esto seria un esquema de 2 de 3.

Por ejemplo solo con A y C se puede regenerar el secreto.

<figure><img src="https://seednode.super.site/_next/image?url=https%3A%2F%2Fassets.super.so%2F232cf1f1-52e8-4e0c-9e18-6bf1f51649a5%2Fimages%2F4b8135ba-a130-49ea-b98a-df74fcac6cbd.png&#x26;w=3840&#x26;q=90" alt=""><figcaption></figcaption></figure>

### Consenso <a href="#block-951be23a5a1041cd8b07dfd7086e323a" id="block-951be23a5a1041cd8b07dfd7086e323a"></a>

Una vez que todos los nodos hayan recibido su parte de la clave secreta, el clúster estará listo para comenzar a atestiguar y proponer bloques. Esto se logra alcanzando un quórum sobre qué bloques atestiguar o proponer mediante un protocolo de consenso conocido como Byzantine Fault Tolerance (BFT). En términos simples, este tipo de protocolo permite que varias partes (en nuestro caso, nodos) lleguen a un acuerdo sobre cómo realizar una tarea específica, incluso si un cierto número f de estas partes son bizantinas, es decir, si están fallando (por ejemplo, están desconectadas) o están tratando activamente de sabotear el correcto funcionamiento del protocolo.

Usemos como ejemplo un hipotético proceso para alcanzar el consenso sobre un bloque en una configuración de DVT. El algoritmo de BFT selecciona un nodo DVT como líder, quien propondrá el bloque y lo compartirá con los demás nodos. Si la mayoría de los nodos están de acuerdo en que el bloque es válido, entonces proceden a firmar. Si el líder se desconecta en una configuración de DVT, el algoritmo BFT reasignará el rol a otro nodo del clúster.

El número máximo tolerable f de partes bizantinas en relación con el número total n de partes depende de cada protocolo, y generalmente tenemos n=2f+1 o n=3f+1.

## Diferencias entre implementaciones <a href="#block-31086e95ee6a4f67bc994c8f34c4561e" id="block-31086e95ee6a4f67bc994c8f34c4561e"></a>

Las dos implementaciones más grandes de DVT en Ethereum son OBOL y SSV. Ambas comparten los mismos principios y tienen algunas metodologías en común. En esencia, ambos sistemas priorizan la Tolerancia a Fallos Bizantinos (BFT) para garantizar robustez y seguridad contra acciones adversas. También aseguran que los usuarios mantengan el control y la soberanía sobre sus activos.

Hay mucho en común entre SSV y Obol, pero para comprenderlos mejor, es importante destacar las diferencias.

Según sus páginas web, el lema de SSV es "Infraestructura de Validador Distribuido para Desarrolladores" y el de Obol "Validadores Distribuidos para Ethereum". Podemos ver que la principal diferencia es el enfoque de SSV en "Infraestructura para Desarrolladores".

Para lograr este objetivo de convertirse en una infraestructura descentralizada para aplicaciones de staking, SSV separa el staking en asignación de capital (los stakers proporcionan ETH) y servicios de validación (operadores que realizan tareas de validación).

Por otro lado, Obol pone un gran énfasis en la no correlación, asegurando que los diferentes clústeres estén completamente aislados entre sí.

A continuación, vamos a analizar por separado el enfoque de cada uno y sus principales decisiones de diseño.

### Obol <a href="#block-bd2a325ec2224ceda9799cd72fe0b401" id="block-bd2a325ec2224ceda9799cd72fe0b401"></a>

**Obol** es una implementación del tipo DVT middleware, similar a la que usamos para explicar el concepto de DVT. El cliente que utiliza se denomina **Charon**.

Charon se sitúa entre el cliente del validador y el nodo de consenso, coordinando el consenso dentro del clúster antes de cualquier interacción con la beacon chain.

<figure><img src="https://seednode.super.site/_next/image?url=https%3A%2F%2Fassets.super.so%2F232cf1f1-52e8-4e0c-9e18-6bf1f51649a5%2Fimages%2F3d13cf09-33ed-4c66-bc33-5da9c6fc4b33.png&#x26;w=3840&#x26;q=90" alt=""><figcaption></figcaption></figure>

Obol no maneja directamente las operaciones de firma del validador. En su lugar, coordina el consenso dentro del clúster.

Los operadores de nodos en Obol necesitan formar y financiar sus propios clústeres. Los rewards entre los participantes en cada clúster pueden determinarse entre ellos mismos, de la forma que elijan.

En OBOL, primero se genera una clave distribuida (DKG). Al final de este proceso, cada nodo tiene una parte de la clave privada. Charon conecta estos nodos entre sí y cumple el rol del consenso cada vez que hay una tarea de validación, decidiendo el hash a firmar. Cada nodo del clúster verifica que no se hayan violado reglas antes de firmar con su parte de la clave privada.

El middleware comparte las firmas entre los nodos. Una vez que se reúnen suficientes firmas (por ejemplo, cuatro de cinco en un clúster de cinco nodos), se combinan en una firma válida completa y se envían a la red. Esto permite tolerancia a fallos, ya que no todos los nodos deben estar en línea al mismo tiempo, permitiendo alta disponibilidad, reinicios escalonados y reemplazo de hardware sin tiempo de inactividad.

### SSV <a href="#block-d2ee10671dc94ff8a932a1d2c1c6fac3" id="block-d2ee10671dc94ff8a932a1d2c1c6fac3"></a>

SSV consiste en una red pública de operadores en la que cualquiera puede ofrecer sus servicios de validación interactuando con los contratos inteligentes de SSV.

El aspecto positivo de que SSV tenga una red pública es que los stakers pueden proporcionar su ETH y los operadores de nodo pueden ofrecer sus servicios sin ninguna comunicación y coordinación previa. Este enfoque ofrece gran flexibilidad, ya que los operadores no necesitan coordinarse para formar clústeres y los stakers no necesitan comunicarse con los operadores ni elegir clústeres predefinidos. Pueden elegir cualquier operador y el clúster se crea automáticamente.

Esto permite un staking exclusivamente a través de contratos inteligentes, haciéndolo utilizable para DAOs, dApps y otras soluciones automatizadas.

<figure><img src="https://seednode.super.site/_next/image?url=https%3A%2F%2Fassets.super.so%2F232cf1f1-52e8-4e0c-9e18-6bf1f51649a5%2Fimages%2F4cf011fb-75be-427f-a7fc-8badd5568e13.png&#x26;w=3840&#x26;q=90" alt=""><figcaption></figcaption></figure>

En SSV, los nodos se comunican mediante un protocolo pub-sub sobre una red P2P (similar a la beaconchain).

Como ya hemos mencionado, los nodos de SSV utilizan un protocolo de consenso BFT como su capa de consenso para coordinarse y acordar qué datos deben firmar en cada epoch.

SSV utiliza un conjunto de contratos inteligentes como su capa de disponibilidad de datos; todos los validadores están registrados y mantenidos allí, junto con los registros de los operadores de nodo. Esto resulta en una red de operadores en forma de malla que los usuarios pueden elegir libremente y sin coordinación para ejecutar un validador de Ethereum.

<figure><img src="https://seednode.super.site/_next/image?url=https%3A%2F%2Fassets.super.so%2F232cf1f1-52e8-4e0c-9e18-6bf1f51649a5%2Fimages%2F78ead6cd-b20d-4815-9d9e-05633b15b9ac.png&#x26;w=3840&#x26;q=90" alt=""><figcaption></figcaption></figure>

SSV es un protocolo que decide quién firma, donde los nodos actúan en 2 pasos.

Paso 1: Cada nodo obtiene localmente los datos sobre lo que debe firmar. Mientras tanto, se selecciona un "nodo líder" para iniciar una ronda de consenso BFT. Durante esta ronda, todos los nodos deciden sobre qué deberían firmar.

Paso 2: Una vez que todos los nodos deciden qué deberían firmar, cada uno utiliza un fragmento de la clave del validador asignado para ellos para firmar. Se recopilan firmas parciales, se reconstruye la firma total y se transmite a la beaconchain.

La magia detrás de estos 2 pasos aparentemente simples es el uso de un protocolo completo de consenso BFT junto con "threshold signatures" para crear un protocolo tolerante a fallos que puede ser seguro y activo incluso con el ⅓ de los nodos siendo defectuosos (maliciosos, sin conexión, mala conectividad, etc.).

Por último, vamos a mencionar que la red ssv.network utiliza su token nativo SSV para facilitar los pagos entre stakers y operadores de nodos. Los operadores mantienen a los validadores y deben llegar a un consenso con grupos de otros operadores para realizar todas las tareas de red en la beaconchain, generando así recompensas de staking en Ethereum para los stakers.

<figure><img src="https://seednode.super.site/_next/image?url=https%3A%2F%2Fassets.super.so%2F232cf1f1-52e8-4e0c-9e18-6bf1f51649a5%2Fimages%2F0a91d1a3-a932-4bfd-bbd4-ecf5286d8d34.png&#x26;w=3840&#x26;q=90" alt=""><figcaption></figcaption></figure>

### **Otras implementaciones** <a href="#block-eecf9f3710a144baae1586581b47ea03" id="block-eecf9f3710a144baae1586581b47ea03"></a>

DVT es una tecnología, y como tal, hay muchas formas de aplicarla. Ya hemos visto los dos mayores exponentes que son OBOL y SSV, pero no son los únicos. Y al ser un tema en pleno crecimiento, van a ir apareciendo con el tiempo más soluciones.

Hasta la fecha, podemos mencionar proyectos como DIVA, que es una solución de staking líquido completamente integrada con DVT, o SafeStake.

## Simple LIDO-DVT <a href="#block-b48de90cf243485a8a11b79ed3ec37ac" id="block-b48de90cf243485a8a11b79ed3ec37ac"></a>

Lido es el protocolo con mayor peso en el staking de Ethereum. Controla casi el 30% de todo el ETH en stake. Si esta participación crece solo un par de puntos porcentuales, superando el umbral del 33% requerido para bloquear una supermayoría del 67% de los validadores, los cortes en la red o la mala conducta deliberada en Lido podrían tener enormes repercusiones para Ethereum en su conjunto. Este es un riesgo bien conocido acerca de las soluciones de staking líquido en general y su tendencia a centralizar capital.

<figure><img src="https://seednode.super.site/_next/image?url=https%3A%2F%2Fassets.super.so%2F232cf1f1-52e8-4e0c-9e18-6bf1f51649a5%2Fimages%2F72f7f09d-abe9-423c-99f9-b0560835c836.png&#x26;w=3840&#x26;q=90" alt=""><figcaption></figcaption></figure>

Al momento de escribir estas líneas las propuestas de DVT antes descritas, por sí solas, tienen una adopción realmente baja. Según una estimación de Obol menos del uno por ciento del ETH staked está controlado por validadores basados en DVT. Esto realmente puede cambiar con LIDO entrando en juego.

En noviembre del 2023 Lido dio un primer paso hacia la transición a DVT con la introducción de su "DVT-Simple Module". Lido recibe depósitos de usuarios y los distribuye entre operadores de validadores de terceros. Con el nuevo módulo DVT que está siendo probado en asociación con Obol y SSV, los validadores de terceros de Lido pueden volverse descentralizados, disminuyendo la capacidad de Lido a ejercer presión indebida sobre ellos.

<figure><img src="https://seednode.super.site/_next/image?url=https%3A%2F%2Fassets.super.so%2F232cf1f1-52e8-4e0c-9e18-6bf1f51649a5%2Fimages%2Fd0ab5d8b-6b4f-49b2-9776-4973fd2d740b.png&#x26;w=640&#x26;q=90" alt=""><figcaption></figcaption></figure>

Simple DVT se está ejecutando de manera muy profesional. Existe una gestión de riesgos implícita, ya que DVT sigue siendo una tecnología emergente dentro de un entorno muy cambiante. Inicialmente solo un pequeño porcentaje de las participaciones en la plataforma Lido se gestionará usando esta tecnología (los seleccionados por LIDO). Esta fase de prueba permite a la DAO y a los operadores de nodos monitorear cómo funciona el sistema y realizar ajustes según sea necesario.

## Conclusiones <a href="#block-66f18426f6184cde8d2d497f4c8f01c0" id="block-66f18426f6184cde8d2d497f4c8f01c0"></a>

**DVT** ofrece numerosas ventajas sobre los solo-stakers:

* Seguridad: Al distribuir el proceso de validación entre múltiples nodos, se dificulta que una entidad única tome el control de la red y la manipule maliciosamente.
* Descentralización: Distribuir el proceso de validación ayuda a evitar puntos de control centralizados.
* Tolerancia a fallos: Una red distribuida de validadores puede proporcionar una mejor resistencia a fallos. Si un nodo se desconecta, el validador sigue funcionando.
* Resistencia regulatoria: Si una zona geográfica deja de operar, el validador seguirá funcionando.
* Transparencia y confianza: Ninguna entidad única tiene control completo sobre la validación de transacciones.
* Diversidad de clientes: Cada nodo del clúster puede ejecutar diferentes clientes.

Los riesgos y aspectos a tener en cuenta son:

* Aumento de la complejidad: Este es el riesgo clásico de todas las nuevas tecnologías en el ecosistema blockchain. En términos generales, implica la incorporación de nuevos protocolos entre "el dueño del activo (ETH)" y "la actividad económica en sí (stake)". Cada línea de código de estos nuevos protocolos será analizada constantemente para detectar posibles vulnerabilidades y atacarlas.
* Latencia: DVT introduce saltos de red adicionales mediante el mecanismo de consenso y el intercambio de mensajes entre los nodos del clúster.
* Costos operativos: Se requiere la participación de múltiples nodos en lugar de solo uno, lo que implica mayores costos operativos y de hardware.
