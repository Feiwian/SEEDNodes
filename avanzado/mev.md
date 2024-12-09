---
icon: robot
---

# MEV

## Artículos previos:

{% embed url="https://mirror.xyz/seedlatam.eth/nU0v23aHxFI7Aml-7JJr0i0VQyhzzLYqkqDnypxVN2Y" %}
November 27th, 2021
{% endembed %}

{% embed url="https://mirror.xyz/seedlatam.eth/pp9tJ0Ovq3f-i0ZwgDloxB4vls91QaGEJQbA2Ow2t1s" %}
December 7th, 2021
{% endembed %}

{% embed url="https://mirror.xyz/seedlatam.eth/C4gjpjtCGqN6tV1d9tpT_yxEPt69blwwA5eK64oL4Zw" %}
March 8th, 2022
{% endembed %}

## Introducción ¿Qué es MEV?

Si analizamos las recompensas que recibe un validador de Ethereum podemos clasificarlo básicamente en tres:

1. Emisión del protocolo (inflación)
2. Tarifas que pagan los usuarios (fees)
3. MEV

<figure><img src="../.gitbook/assets/wilb1.png" alt=""><figcaption><p><a href="https://ultrasound.money/#monetary-premium">https://ultrasound.money/#monetary-premium</a></p></figcaption></figure>

El **Valor Máximo Extraíble (MEV, por sus siglas en inglés)** se refiere al valor adicional que los validadores pueden obtener debido a su capacidad para:

1. Decidir qué transacciones incluir o excluir de un bloque.
2. Determinar el orden en que se ejecutan las transacciones dentro del bloque.

Ambos aspectos tienen implicaciones importantes para el sistema y dan lugar a diferentes tipos de MEV.

En este artículo, abordaremos brevemente el MEV relacionado con el primer punto y exploraremos en profundidad el segundo, donde el reordenamiento de transacciones puede generar valor adicional, habilitando prácticas como el _front-running_ (adelantamiento) o el _sandwiching_ (intercalación).

En resumen, el MEV es el valor que los validadores pueden extraer más allá de las recompensas por bloque y las tarifas de transacción.

## **Inclusion/Exclusion** MEV y EIP-1559, un poco de historia

{% hint style="success" %}
En esta sección hablamos de mineros, porque esto sucedía en la era POW
{% endhint %}

Una de las motivaciones detrás de la EIP-1559, el sistema actual de tarifas de Ethereum, fue mitigar e incluso eliminar parte del MEV relacionado con la inclusión y exclusión de transacciones.

En el sistema anterior, Ethereum utilizaba un mecanismo de subasta para vender el espacio en los bloques, o más específicamente, el gas. Este gas representa el esfuerzo computacional necesario para ejecutar operaciones dentro de Ethereum.

Como compradores de este recurso, los usuarios observaban otras transacciones para estimar cuánto estaban pagando (la tarifa por unidad de gas, conocida como _gas price_), y luego ajustaban su propia oferta ligeramente por encima de las existentes. Este proceso generaba ciertas ineficiencias, como el _auction MEV_ (MEV de subasta) y el _base_ _MEV_.

Para entender estos diferentes tipos de MEV pensemos en un momento de alta demanda en la antigua red de Ethereum. En esas circunstancias, no todas las transacciones enviadas podían ser incluidas en el bloque siguiente; solo aquellas que ofrecían tarifas más altas lograban entrar. Consideremos el siguiente ejemplo, donde 8 transacciones compiten por ser incluidas en el siguiente bloque:

1. **tx1**: paga muy poco, no es incluida.
2. **tx2**: paga muy poco, no es incluida.
3. **tx3**: paga lo necesario, entra en el próximo bloque.
4. **tx4**: paga un poco más de lo necesario, entra en el próximo bloque.
5. **tx5**: paga más que la anterior, entra en el próximo bloque.
6. **tx6**: paga más que la anterior, entra en el próximo bloque.
7. **tx7**: paga más que la anterior, entra en el próximo bloque.
8. **tx8**: paga más que la anterior, entra en el próximo bloque.

<figure><img src="../.gitbook/assets/wilb3.png" alt=""><figcaption><p><a href="https://raw.githubusercontent.com/jordanmmck/crypto-diagrams/master/eip1559/EIP1559.png">https://raw.githubusercontent.com/jordanmmck/crypto-diagrams/master/eip1559/EIP1559.png</a></p></figcaption></figure>

**Eje X (horizontal):** Representa el tamaño total del bloque en términos de gas consumido. Observamos que **tx7** requiere más gas que las demás, lo cual puede suceder cuando una transacción implica mayor esfuerzo computacional, como por ejemplo mintear un dominio es mas costoso que simplemente enviar ETH.

**Eje Y (vertical):** Indica el precio por unidad de gas pagado por cada transacción.

Cada transacción en el bloque tiene tres componentes diferenciados por colores:

**Amarillo:** Representa el costo base de la transacción, es decir, el esfuerzo computacional puro necesario para ejecutarla. No tiene en cuenta la demanda de los usuarios.

**Magenta:** Indica el _Base MEV_, el sobreprecio que los usuarios pagan debido al congestionamiento de la red. Esto corresponde a la diferencia entre el costo base y el precio justo para incluir la transacción en el bloque.

**Violetita:** Representa el _Auction MEV_, que es el sobreprecio adicional pagado por los usuarios debido al modelo de subasta. Este sobreprecio no es esencial para ser incluidos en el bloque y podría ser reducido con mecanismos más eficientes.

Entonces:

**tx1 y tx2:** No lograron ser incluidas debido a sus tarifas bajas.

**tx3:** Pagó el precio justo para entrar al bloque, absorbiendo solo el _Base MEV_.

**tx4 a tx8:** Pagaron tarifas por encima de lo necesario, generando ademas el _Auction MEV_, una ineficiencia del sistema de subasta que beneficiaba a los mineros.

El proceso ciego de ofertar hacía que los usuarios tendieran a pagar de más. Por ejemplo, la tx8 pago mucho más que tx3 cuando podría haber pagado solo un poco más, o incluso lo mismo, para obtener el mismo resultado.

Por otro lado los mineros no necesitaban este ingreso adicional para estar correctamente incentivados.

#### Implementación de la EIP-1559

La **EIP-1559** introdujo un nuevo mecanismo para abordar las ineficiencias del sistema de tarifas anterior. En lugar de una subasta de primer precio, se implementó un sistema que combina una **tarifa base (**_**base fee**_**)**, que todos los usuarios deben pagar para incluir sus transacciones, y una **propina opcional (**_**tip**_**)**, destinada a los constructores de bloques para incentivar la prioridad en la inclusión.

$$
fee = (baseFee + priorityFee) * gasUsed
$$

* La tarifa base (base\_fee) es un valor ajustado por el protocolo para mantener el uso de la red a un nivel constante
* El tip (pririty\_fee) es una cantidad adicional pagada al validador (cometa) para incentivarlo a incluir tu transacción.

La siguiente imagen muestra una vista en tiempo real del precio del gas. En condiciones normales, la tarifa base (_base fee_) suele ser significativamente mayor que la tarifa de prioridad (_priority fee_). Esto se debe a que, en general, incluir transacciones no presenta mayores dificultades, ya que los bloques suelen tener suficiente espacio disponible. La mayoría de los validadores simplemente toman todas las transacciones visibles en la red y las incluyen en el bloque. Sin embargo, las tarifas de prioridad pueden presentar una variabilidad extrema, especialmente en escenarios de congestión.

<figure><img src="../.gitbook/assets/wilb4.png" alt=""><figcaption><p><a href="https://www.blocknative.com/gas-estimator">https://www.blocknative.com/gas-estimator</a></p></figcaption></figure>

Por lo tanto la mejora del EIP-1559 eficientizo los dos principales tipos de MEV presentes en el sistema anterior:

1.  **Auction MEV (MEV de subasta):**

    Aunque este tipo de MEV no puede eliminarse por completo, su impacto se redujo de manera considerable.
2.  **Base MEV (MEV base):**

    En el nuevo sistema, la tarifa base se quema en lugar de destinarse a los constructores de bloques. Esto redistribuye el valor a favor de los holders de ETH, beneficiando al ecosistema al reducir la oferta total de la criptomoneda, en lugar de enriquecer exclusivamente a los mineros.

Si bien este nuevo modelo eficientizo la situación descripta, el desafío relacionado con el orden específico de las transacciones persiste. Actualmente, Ethereum carece de un sistema dentro del protocolo para regular este aspecto.

## **Specific Ordering MEV -** La Importancia del Orden de las Transacciones

En el estado actual de Ethereum los constructores de bloques tienen la libertad de ordenar las transacciones según lo deseen, ya que el protocolo no ofrece un mecanismo oficial para que los usuarios expresen sus preferencias sobre el orden relativo de sus transacciones.

Sin embargo, en un entorno financiero como Ethereum, donde millones de dólares pueden depender de pequeñas diferencias en el orden de ejecución, esta falta de control puede tener consecuencias significativas. El orden de las transacciones puede ser la diferencia entre obtener ganancias o sufrir pérdidas, especialmente en escenarios de alta volatilidad.

Para abordar esta necesidad, surgió una solución fuera del protocolo llamada **MEV-Boost**, un sistema que permite a ciertos usuarios expresar y pagar por sus preferencias sobre el orden de las transacciones. Esto ha permitido un mecanismo más estructurado para gestionar el **Specific Ordering MEV** (MEV por orden específico), un tipo de MEV que va más allá de las simples tarifas y está relacionado directamente con actividades financieras dentro de Ethereum, tales como:

* _Front-running_ (adelantamiento de operaciones).
* _Back-running_ (seguimiento de operaciones).
* _Sandwiching_ (intercalación).
* _Generalized front-running_ (adelantamiento generalizado).

En la práctica, esta falta de control sobre el orden dentro del protocolo ha dado lugar a un mercado no oficial, donde algunos usuarios y constructores negocian directamente por el lugar de sus transacciones en el bloque.

## Construcción estándar de bloques - _Vanilla Block Building_

#### Lo que ve el usuario que envía una transacción

El proceso para enviar una transacción en Ethereum es relativamente simple: la billetera (_wallet_) genera la transacción, el usuario la firma y esta se transmite a un nodo de Ethereum, ya sea un nodo propio o a través de un servicio intermediario como Infura o Alchemy. Estos nodos comparten la transacción con otros nodos conectados, formando una red de transacciones pendientes conocida como el _mempool_ público.

Este _mempool_ es accesible para cualquier participante de la red, lo que proporciona transparencia sobre el estado actual de las transacciones pendientes. Se puede ver el estado en tiempo real del _mempool_ público en el siguiente enlace:

{% embed url="https://www.ethernow.xyz/mempool/all" %}

El usuario queda a la espera de que un _block builder_ tome su transacción del _mempool_ y la incluya en el próximo bloque.

#### Lo que hace el validador

En el sistema _Proof of Stake_ (PoS) de Ethereum, se elige aleatoriamente a un validador para actuar como _block proposer_(proponente de bloque).

A día de hoy (agosto de 2024), hay más de 1.000.000 de validadores activos. Esta métrica se puede consultar en el siguiente enlace:

{% embed url="https://dune.com/hildobby/eth2-staking" %}

Esto implica que, para un validador individual, ser seleccionado como _block proposer_ es una tarea poco frecuente. En el caso que estamos analizando, conocido como _vanilla block building_, el validador, que será el _block builder_, debe tener instalados en su nodo los siguientes clientes estándar de Ethereum:

1. **Cliente de ejecución.**
2. **Cliente de consenso.**
3. **Cliente de validación.**

Con estos clientes estándar, al momento de elegir transacciones del _mempool_ público para insertarlas en un bloque, solo se consideran aspectos básicos como el _base fee_ y el _priority fee_. Una vez creado el bloque, este se envía a la red. Bajo este modelo, el validador recibe recompensas únicamente por las tarifas (_fees_) pagadas por los usuarios y el subsidio del protocolo (emisión).

A continuación, se explicará cómo es el proceso para seleccionar y ordenar transacciones en Ethereum para que los validadores puedan obtener ganancias a través del _MEV_ (Valor Máximo Extraíble).

## La llegada de Flashbots

En la situación descrita anteriormente, no es difícil imaginar que los validadores con herramientas avanzadas para monitorear el _mempool_ público puedan utilizar la información de las transacciones de los usuarios para su propio beneficio. Este escenario plantea serios desafíos para la equidad y la descentralización dentro del ecosistema de Ethereum.

Es aquí donde entra en escena Flashbots, una organización de investigación y desarrollo enfocada en mitigar los problemas asociados al _MEV_.

{% embed url="https://www.flashbots.net" %}

Flashbots no tiene como objetivo eliminar el _MEV_ por completo. En cambio, busca transformar la forma en que se gestiona el MEV a través de los siguientes pilares fundamentales:

1. **Iluminar** (_Illuminate_): Exponer la actividad del _MEV_, proporcionando visibilidad sobre cómo y dónde ocurre, para que pueda ser comprendida por la comunidad.
2. **Democratizar** (_Democratize_): Permitir que todos los constructores de bloques (_block builders_) participen en la red, eliminando la necesidad de conexiones exclusivas o pertenecer a grupos privilegiados.
3. **Distribuir** (_Distribute_): Asegurar que los ingresos generados por el _MEV_ se repartan de manera más equitativa entre los participantes.

Este enfoque representa un avance significativo hacia un sistema más justo y transparente dentro de Ethereum, mitigando los efectos más nocivos del _MEV_ y promoviendo la descentralización.

MEV-Boost es una **herramienta específica** creada por Flashbots como parte de su solución al problema del _MEV_ previamente descrito.

A continuación, explicaremos cómo funciona.

## MEV - Boost

Es un cliente que los nodos validadores deben instalar para poder obtener recompensas adicionales gracias al MEV.

MEV-Boost conecta a los validadores con todo un sistema paralelo al protocolo de Ethereum.

{% hint style="info" %}
El flujo operativo sigue la filosofía de _Proposer-Builder Separation_ (PBS), que establece que las entidades encargadas de construir y proponer bloques en Ethereum deben ser diferentes.
{% endhint %}

Este sistema tiene un objetivo muy concreto: dar garantías de seguridad al validador proponente del bloque para que pueda subastar la creación del bloque al mejor postor. Este sistema está compuesto por tres actores principales: **proposers**, **builders** y **relay**.

<figure><img src="../.gitbook/assets/adadsf.png" alt=""><figcaption></figcaption></figure>

1. **Proposer**: Son los nodos validadores encargados de cumplir todos los requisitos de la _beacon chain_ para proponer bloques en Ethereum. En cada _slot_, un validador específico será responsable de proponer un bloque, mientras que el resto de los validadores se encargan de atestiguar que dicho bloque es honesto.
2. **Builder**: Son las entidades responsables de construir bloques optimizando las ganancias provenientes del MEV.
3. **Relay**: Son los intermediarios que facilitan la comunicación entre los _Builders_ y los _Proposers_.

#### Builders y Searchers

El flujo de funcionamiento de MEV comienza con los usuarios de Ethereum cuando envían sus transacciones. Eventualmente, estas transacciones llegan al _mempool_ público.

Aquí es donde entran en juego unas entidades llamadas _Searchers_. Los _Searchers_ son bots, es decir, computadoras que ejecutan programas muy específicos. Estos programas están diseñados para correr algoritmos que identifican oportunidades de MEV en el _mempool_, con el objetivo de maximizar ganancias.

<figure><img src="../.gitbook/assets/asdfasdfa.png" alt=""><figcaption></figcaption></figure>

**Ejemplo:**

Supongamos que un usuario decide comprar un token. Un _Searcher_ podría detectar esta transacción en el _mempool_ y buscar formas de extraer valor de ella.

Por ejemplo, podría ejecutar un ataque de _sandwiching_ (intercalación), que consiste en construir un paquete de transacciones (_bundle_) con el siguiente orden:

1. Comprar el mismo token antes de que el usuario realice su compra (_front-running_).
2. Incluir la transacción del usuario (su compra).
3. Vender el token inmediatamente después de la compra del usuario (_back-running_).

Con este mecanismo, el _Searcher_ puede generar ganancias al aprovechar el impacto que la transacción del usuario tiene sobre el precio en el mercado. Sin embargo, para que estas ganancias sean efectivas, el _bundle_ debe ser incluido en el próximo bloque.

{% hint style="info" %}
Lo que sucede aca es que cuando se ejecuta la compra del searcher, el precio del activo sube y el usuario termina comprando menos tokens de lo esperado. Desde el punto de vista del usuario, una forma de protegernos de este tipo de MEV es ajustando el _slippage_ en los dex.
{% endhint %}

El _Searcher_ incluye un soborno (_bribe_) dentro de este _bundle_ para convencer al _Builder_ (constructor de bloques) de procesar el paquete de transacciones exactamente como está ordenado.

A continuacién se ve una victima de este tipo de MEV:

<figure><img src="../.gitbook/assets/jhnkl.png" alt=""><figcaption><p><a href="https://www.reddit.com/r/UniSwap/comments/17ytq0j/what_happened_here_i_am_so_confused/?rdt=49214">https://www.reddit.com/r/UniSwap/comments/17ytq0j/what_happened_here_i_am_so_confused/?rdt=49214</a></p></figcaption></figure>

**Hash de la Transacción**:

0x11ee1c1fd6cb5e7b0b7716082cacb461dc6f2bfa2f3ed119205822d1dc04ba55

<figure><img src="../.gitbook/assets/knlm.png" alt=""><figcaption><p><a href="https://etherscan.io/tx/0x11ee1c1fd6cb5e7b0b7716082cacb461dc6f2bfa2f3ed119205822d1dc04ba55">https://etherscan.io/tx/0x11ee1c1fd6cb5e7b0b7716082cacb461dc6f2bfa2f3ed119205822d1dc04ba55</a></p></figcaption></figure>

En el bloque se puede ver claramente porque le llamamos sandwiching, donde los MEV bot son los panes:

<figure><img src="../.gitbook/assets/jbkbkjbkbkjb.png" alt=""><figcaption><p><a href="https://etherscan.io/txs?block=18604963&#x26;ps=100&#x26;p=3">https://etherscan.io/txs?block=18604963&#x26;ps=100&#x26;p=3</a></p></figcaption></figure>

#### **Builders: los que construyen los bloques**

Los _Builders_ reciben paquetes de transacciones (_bundles_) enviados por los _Searchers_, los organizan en un bloque y buscan maximizar los ingresos asociados a ese bloque. Estos ingresos provienen de dos fuentes principales:

1. **Ingresos por tarifas estándar:** Las comisiones habituales que los usuarios pagan para incluir sus transacciones en un bloque.
2. **Valor extraído de MEV:** Los incentivos (_bribes_) ofrecidos por los _Searchers_ dentro de los _bundles_.

Además, los _Builders_ complementan el bloque añadiendo transacciones normales si es necesario. Por lo general, configuran su propia dirección como la dirección _coinbase_ (destinataria de las recompensas), lo que les permite recibir tanto las tarifas estándar como los incentivos relacionados con el MEV.

Sin embargo, los _Builders_ no tienen la capacidad de proponer bloques directamente en la red Ethereum. El protocolo de Ethereum es el que determina qué validador actuará como _block proposer_ (proponente de bloque) en cada _slot_.

Por esta razón, los _Builders_ deben ofrecer un incentivo atractivo al _proposer_ para que acepte su bloque. Esto ocurre porque el _proposer_ no actúa como la dirección _coinbase_ cuando incluye el bloque construido por el _Builder_, lo que significa que no recibe directamente las tarifas ni los incentivos contenidos en ese bloque.

#### **Relays: los intermediarios**

Los _relays_ actúan como intermediarios que facilitan la comunicación entre _builders_ y _proposers_, desempeñando un rol clave en el flujo de bloques:

1. **Conexión con múltiples&#x20;**_**Builders**_**:** Un _relay_ se conecta a varios _Builders_ para recopilar los encabezados de los bloques junto con el soborno (_bribe_) ofrecido al _proposer_.
2. **Selección del mejor bloque:** El _relay_ analiza las ofertas recibidas y selecciona el encabezado del bloque que proporciona el soborno (_bribe_) más alto, enviándolo luego al _proposer_.

En su esencia, los _relays_ son bienes públicos. No generan ingresos y su función principal es centralizar y facilitar el acceso a la información de los _Builders_, evitando que los validadores tengan que conectarse directamente a múltiples _Builders_. Esto simplifica el proceso y refuerza la seguridad del sistema.

#### **Flujo general del MEV**

Con todas las partes explicadas, podemos detallar el flujo general desde la perspectiva del _relay_ en unos pocos pasos:

1. **Análisis del mempool:** Los _Searchers_ identifican oportunidades en el _mempool_ y envían paquetes de transacciones (_bundles_) a los _Builders_.
2. **Recepción de ofertas:** Los _Builders_ crean bloques optimizados y envían sus propuestas al _relay_, que actúa como un subastador confiable.
3. **Selección del mejor bloque:** El _relay_ evalúa las ofertas de los _Builders_, selecciona el encabezado del bloque con el soborno (_bribe_) más alto y lo envía al _proposer_ (validador designado).
4. **Firma del bloque:** El _proposer_ firma digitalmente el encabezado seleccionado usando su clave privada. Esto asegura que no pueda firmar otro bloque y da confianza al _Builder_ de que su bloque será propuesto.
5. **Liberación del bloque:** Con la firma del _proposer_ en mano, el _Builder_ libera el bloque completo. Este bloque regresa al _relay_, que lo entrega al _proposer_ para su inclusión en la cadena de bloques.

Este flujo asegura que todas las partes involucradas trabajen de manera eficiente y confiable, maximizando las ganancias de MEV mientras se mantiene la seguridad y el orden en la red Ethereum.

<figure><img src="../.gitbook/assets/sdfgsfd.gif" alt=""><figcaption></figcaption></figure>

En la siguiente imagen podemos ver como se ve “realmente” este flujo:

<figure><img src="../.gitbook/assets/xcvbxcv.png" alt=""><figcaption><p><a href="https://mevboost.pics/">https://mevboost.pics/</a></p></figcaption></figure>

En la parte inferior de la imagen donde están los _proposers_, solo vemos las entidades principales (que tienen una porción muy significativa del stake). Los _solo-stakers_ no figuran en el grafico.

#### **Protección contra comportamientos deshonestos**

Los _relays_ son esenciales para garantizar la integridad y seguridad del flujo de bloques:

1. **Prevención de robo de bloques:** Si un _Builder_ enviara directamente su bloque al _proposer_, este último podría intentar robarlo copiando la transacción del soborno (_bribe_) o desempaquetando las transacciones de MEV para aprovecharse del valor capturado.
2. **Garantía contra modificaciones:** Los _relays_ aseguran que el _proposer_ no pueda alterar el bloque recibido, ya que cualquier intento de firmar encabezados alternativos será detectado por el sistema Ethereum, generando un conflicto.
3. **Penalización por conflicto:** En caso de que un _proposer_ firme dos encabezados diferentes, será penalizado (_slashing_), lo que puede resultar en la pérdida parcial o total de sus 32 ETH en _staking_. Este mecanismo protege a los _Builders_ y desincentiva cualquier intento de manipulación.

De esta manera, los _relays_ fortalecen la confianza y la transparencia en el proceso de construcción y propuesta de bloques dentro de la red Ethereum.

## Conclusiones

El mecanismo de separación entre proposers y builders que trae _Mev Boost_ ha sido todo un éxito en términos de adopción, con cerca del 90% de los bloques de Ethereum usando este sistema después de "The Merge" en septiembre de 2022.

<figure><img src="../.gitbook/assets/cxvbxcvbxcvb.png" alt=""><figcaption><p><a href="https://mevboost.pics/">https://mevboost.pics/</a></p></figcaption></figure>

Esto muestra que los validadores prefieren delegar la producción de bloques a constructores especializados para ganar más recompensas.

<figure><img src="../.gitbook/assets/vxcbxcvbxvbxcv.png" alt=""><figcaption><p><a href="https://mevboost.pics/">https://mevboost.pics/</a></p></figcaption></figure>

Pero, aunque funciona bien, no es perfecto; todavía hay desafíos y cosas por resolver.

Más adelante, en otros artículos, hablaremos sobre nuevas ideas y soluciones para mejorar el manejo del MEV, cómo este sistema puede seguir evolucionando, y abordaremos temas como la conformidad con las regulaciones de OFAC (OFAC compliance) y su impacto en el ecosistema.

Además, exploraremos cuestiones filosóficas relacionadas con el MEV: ¿es bueno o malo? ¿Es inevitable en un sistema descentralizado? También profundizaremos en los detalles de los diferentes tipos de MEV, analizando sus particularidades, beneficios y desafíos, para entender mejor cómo afectan a los usuarios y validadores en el ecosistema blockchain.

## Referencias

{% embed url="https://medium.com/etherscan-blog/rapid-rise-of-mev-in-ethereum-9bcb62e53517" %}

{% embed url="https://blog.bcas.io/mev-examination-and-putting-a-stop-to-micas-article-92-nonsense" %}

{% embed url="https://paragraph.xyz/@chaskin/private-transactions-where-mev-and-the-public-mempool-go-to-die" %}

{% embed url="https://ethereum.org/en/developers/docs/mev/" %}
