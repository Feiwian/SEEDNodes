# Consenso en Ethereum - Opciones de staking

## Consenso en Ethereum POS <a href="#block-6a7169fbb59c4b779b757f67023a42b8" id="block-6a7169fbb59c4b779b757f67023a42b8"></a>

Los objetivos del consenso son lograr que todos los nodos independientes y distribuidos alrededor del mundo estén de acuerdo en cuál es el estado de la red y, ademas, que lleguen a dicho acuerdo rápidamente. Esto debe lograrse incluso con una infraestructura de red no confiable.

Con “no confiable” nos referimos a que los nodos pueden ejecutarse en hardware al alcance de cualquier persona en el mundo. Se comunican a través de conexiones a Internet que pueden tener poco ancho de banda, alta latencia, perder paquetes o incluso desconectarse por períodos de tiempo indefinidos. Los operadores de nodos a veces configuran incorrectamente su software o no lo mantienen actualizado. Sumado a esto, existe la posibilidad de que un gran número de usuarios malintencionados ejecuten nodos fraudulentos o intenten manipular las comunicaciones para su propio beneficio. Es evidente entonces, que la infraestructura no es confiable y Ethereum está diseñado para funcionar correctamente en ella.

A lo largo del artículo, se explicará cómo se logra el consenso en Ethereum y que es exactamente el “staking”. Desglosaremos los mecanismos del consenso y los explicaremos por separado, comparándolos con los mecanismos de Bitcoin para proporcionar mayor claridad solo cuando sea necesario.

Luego se detallarán los diferentes mecanismos con lo que los usuarios pueden exponerse al staking.

Los lectores que no estén interesados en la parte mas técnica pueden ir directamente al titulo “Opciones de staking”.

## Introducción <a href="#block-c2d8241a8d8a4f73a3cd5435af7232b1" id="block-c2d8241a8d8a4f73a3cd5435af7232b1"></a>

Este es un buen momento para mencionar que ni POS (prueba de participación) ni POW (prueba de trabajo) son protocolos de consenso en sí mismos. Ganaron merecidamente su popularidad y los referimos como protocolos de consenso, aunque en realidad son solo una parte del mismo. Gracias a POS y/o POW, se pueden aplicar estrategias de teoría de juegos para generar recompensas o penalizaciones en los mecanismos de consenso. Otra tarea fundamental de POW y POS es proporcionar mecanismos de resistencia Sybil que imponen un costo para participar en el protocolo.

Bitcoin es una blockchain que utiliza POW. Su mecanismo de consenso se llama "Consenso de Nakamoto". En sus inicios, Ethereum funcionaba de manera similar: POW + consenso de Nakamoto.

La transición de Ethereum de POW a POS fue un hito significativo, ya que se logró sin interrumpir la red. Mientras Ethereum seguía funcionando normalmente con su cliente "Eth1", se creó en 2020 una red paralela POS llamada "Eth2" o "Beacon Chain". En esta red se probó el nuevo consenso que iba a utilizar Ethereum. Finalmente, en 2022, se fusionaron ambas redes en el evento conocido como "The Merge", y el resultado fue Ethereum POS. Resulta increíble que los usuarios de Ethereum continuaran usando la red sin notar ningún cambio.

Uno de los rastros evolutivos de esta fusión se puede ver en los nodos de Ethereum, donde tenemos dos clientes diferentes: el cliente de ejecución (antes ETH1) y el cliente de consenso (antes ETH2).

El consenso actual de Ethereum POS "une" dos protocolos diferentes. Uno se llama Casper FFG, el otro LMD GHOST. La combinación de ambos dio lugar al nombre del protocolo de consenso de Ethereum POS: Gasper. Veamos qué hacen:

**Casper FFG:** Es un protocolo que marca ciertos bloques como finalizados. Al finalizar bloques, los usuarios de Ethereum pueden estar seguros de que esos bloques forman parte de la cadena principal (o canónica) y no serán revertidos (o eliminados) de ella.

**LMD GHOST:** Es una regla de elección de forks que permite elegir entre muchas cadenas bifurcadas. Por ejemplo, si hay dos forks diferentes en una cadena. Digamos que en el fork:

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/db999d26-9a52-4116-863d-750d594b71b0/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

El LMD GHOST elegirá uno de ellos. Este mecanismo proporciona una especie de ley o regla para evaluar la cadena principal.

A continuación, se muestran los mecanismos que conforman el consenso en Ethereum POS:

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/4b93cd1a-9b37-4509-91ce-8931ce548166/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

A partir de ahora, exploraremos más en detalle cuál es la infraestructura para que esto funcione y cuál es la teoría de juegos que incentiva el comportamiento honesto.

## Funcionamiento de Gasper <a href="#block-942881605bd64755a8af2dfa0cd0b308" id="block-942881605bd64755a8af2dfa0cd0b308"></a>

Gasper crea un mecanismo para que todos los nodos validadores en la red de Ethereum acuerden un bloque de transacciones para añadir a la cadena. La forma en que estos validadores deciden sobre el siguiente bloque es mediante la emisión de votos, en forma de "attestations".

**¿Qué son las attestations?** Los validadores no deben crear bloques para votar, sino que "atestiguarán", es decir, "darán fe" de que el bloque creado por otro es el correcto. Ese será su voto.

### Validadores y atestaciones <a href="#block-21251bfeb0cc4f78a58a8d00440937d6" id="block-21251bfeb0cc4f78a58a8d00440937d6"></a>

Los validadores son nodos completos (full nodes) que ejecutan 3 softwares distintos: el cliente de ejecución, el cliente de consenso y el cliente de validación.

Cualquier nodo puede convertirse en un validador realizando todas las configuraciones previas necesarias y depositando 32 ETH en el "contrato de depósito". [Este contrato especial está designado para recoger y rastrear a los validadores](https://ethereum.org/es/staking/deposit-contract/). Cuando nos referimos a los ETH en stake, hacemos referencia justamente a esto ETH depositados.

💡Los ETH en stake son recursos escasos que son difícil de replicar, característica que comparte con el hash del POW.

La razón por la que realmente es necesario realizar el staking de ETH es para que Gasper pueda aplicar el slashing (sanciones). El slashing implica quemar parte del stake de los validadores si se descubre que están comportándose mal.

La razón por la cual cada validador debe stakear 32 ETH (y no otra cantidad aleatoria) es que permite que todos los validadores sean tratados de igual manera. Dado que los métodos de consenso descentralizado son modelos de votación, con todas las firmas siendo iguales, facilita el conteo de votos.

#### Cola de activación: <a href="#block-8e96be2c64024878a24a897fafeb5969" id="block-8e96be2c64024878a24a897fafeb5969"></a>

Después de depositar ETH en el contrato de depósito, el usuario se une a una "cola de activación" donde básicamente está en fila para convertirse en un validador.

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/a9f9f274-e8ed-4a1c-b737-1304a678c6d3/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/3323b88e-8e7d-4b14-9c31-4eacd75270e5/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

[https://www.validatorqueue.com/](https://www.validatorqueue.com/)

El propósito de la "cola de activación" es garantizar el correcto funcionamiento de la red. Para obtener más detalles sobre cómo funciona el mecanismo que controla las colas de entrada y salida, pueden leer: [https://liquidcollective.io/eth-activations-and-exits/](https://liquidcollective.io/eth-activations-and-exits/)

Una vez que un validador es activado, comienzan sus responsabilidades como tal.

#### Gestión del tiempo <a href="#block-3958cae4e25c43ee938c02a910a3cb87" id="block-3958cae4e25c43ee938c02a910a3cb87"></a>

Cada algoritmo de consenso necesita una línea de tiempo para ordenar eventos en la red. Ethereum POS divide el tiempo en "slots" y "epochs". Los slots tienen una duración de 12 segundos, y cada epoch consta de 32 slots (por lo tanto, 6.4 minutos).

#### Block proposer <a href="#block-5349586d0eb143d48db47d2eee036796" id="block-5349586d0eb143d48db47d2eee036796"></a>

En cada slot, se selecciona aleatoriamente a un validador para ser un "**block proposer**". Este es responsable de construir un nuevo bloque a partir de transacciones pendientes. Luego, envía el bloque a otros validadores en la red, quienes emiten votos (attestations) sobre si ese bloque es válido.

#### Gestión de Attestations <a href="#block-223b892a55584f078f89453f27dba0ca" id="block-223b892a55584f078f89453f27dba0ca"></a>

No todos los validadores votan por cada bloque propuesto; este trabajo se divide para aliviar la congestión de la red y sin perder seguridad. El funcionamiento es el siguiente: en cada epoch, a los validadores se les asigna un 'comité'. Cada comité se asigna aleatoriamente a un slot donde deben votar para determinar si el bloque propuesto es válido.

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/62808e28-4f5d-4222-8023-cfc99e0fce2f/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Si el validador designado para proponer el bloque no propone ningún bloque, los demás miembros del comité simplemente votan por el bloque anterior.

Además de atestiguar por el bloque actual, llamado "head", los validadores también atestiguan otros dos bloques denominados "**checkpoint**". Cada época tiene un bloque checkpoint que identifica el inicio de esa época. El checkpoint del mismo bloque en el que se encuentra el head se conoce como “target”, y el checkpoint del epoch anterior se conoce como “source”.

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/784528de-43bc-4f4a-9bdc-619d3cf1c9c1/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Una vez que el validador atestigua por Head, Target, Source e incluye toda la información necesaria, firma la “attestation” con su clave privada.

Existiría una sobrecarga considerable en la red si cada validador transmitiría todos los datos de su attestation. No discutiremos en este artículo el detalle de cómo se logra solucionar esto, pero mencionaremos los mecanismos necesarios para lograrlo:

1. Existen **agregadores:** Es un grupo de validadores seleccionados en cada época, que reciben y procesan las atestaciones con múltiples firmas de validadores individuales. Cada agregador combina la información en una sola firma y genera la atestación agregada. El proceso de agregación reduce la carga de datos en la red al reducir el número de atestaciones individuales que se transmiten al block proposer.
2. Realizar esta agregación de firmas es posible gracias al esquema criptográfico utilizado en Ethereum POS: “BLS: **Boneh–Lynn–Shacham”**. Al ser un tipo de clave diferente a la que generan la mayoría de las Hardware Wallets, hace que la gestión de la misma sea un desafio para usuarios con poca experiencia técnica.

{% hint style="info" %}
_**En resumen las responsabilidades de un validador incluyen lo siguiente:**_

* _Atestar en un bloque (una vez por época)_
* _Agregar atestaciones de validadores en el mismo comité (ocasionalmente cuando son seleccionados como agregadores)_
* _Crear bloques cuando son seleccionados (raramente cuando son seleccionados como validadores_
{% endhint %}

_Como resultado de estas atestaciones, la cadena de bloques llega a un consenso._

_Nota: un pequeño conjunto de validadores también es elegido aleatoriamente para unirse a los comités de sincronización, los cuales son diferentes de los comités mencionados anteriormente. Estar en un comité de sincronización requiere que los validadores ayuden a los clientes ligeros a sincronizarse y determinar el head de la cadena._

### Finalidad <a href="#block-45665390a72a455e963f532b43eb5836" id="block-45665390a72a455e963f532b43eb5836"></a>

Los validadores votan por los bloques que crea el block proposer si creen que son válidos. Pero, ¿en qué momento determina el protocolo que dicho bloque debe agregarse a la cadena?

Esto es a lo que nos referimos cuando hablamos de "finalidad". Si un bloque alcanza "finalidad", no puede ser revertido a menos que haya ocurrido un fallo crítico en el consenso.

En Bitcoin, no existe esta característica, pero hay algo similar. Si se construyen seis bloques encima del bloque actual, asumimos que el bloque actual está finalizado. La probabilidad de que un bloque sea reorganizado después de seis confirmaciones es tan pequeña que es estadísticamente despreciable. Por eso hablamos de que en Bitcoin la finalidad es heurística.

En Gasper, determinar un bloque finalizado, o "finality", implica una lógica diferente a la regla de seis confirmaciones en Bitcoin. Casper FFG utiliza la siguiente lógica:

* Si ⅔ del ETH total stakeado vota a favor de ese bloque, entonces se vuelve "justificado". Los bloques justificados son poco probables de revertirse (pero no imposible).
* Cuando otro bloque se justifica encima de un bloque justificado, el primero se actualiza a "finalizado". Finalizar un bloque es un compromiso para incluirlo en la cadena canónica. No puede ser revertido a menos que un atacante destruya millones de ETH.

Los bloques no alcanzan los estados "justificados" y "finalizados" en cada slot. Sino que ocurren en el primer bloque de cada epoch, que se llama bloque "de control" (el checkpoint antes mencionado).

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/d8812f3a-59b5-410f-b1bf-0a296f80f257/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Cuando un bloque de control se actualiza a justificado, debe tener un enlace al bloque de control anterior. Es decir, ⅔ del ETH total en staking deben votar que el bloque de control B es el descendiente correcto del bloque de control A. Como resultado, los bloques del epoch anterior se finalizan y los bloques del epoch más reciente se justifican.

En la siguiente imagen podemos ver que cuando el epoch 275245 pase a justificado, el 275244 pasará a finalizado.

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/b2b914b3-888a-4986-87ea-901a85287fa2/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Por lo tanto, un atacante no puede crear una cadena alternativa finalizada sin:

* Poseer o manipular ⅔ del ETH total en staking
* Destruir al menos ⅓ del ETH total en staking (porque si ⅔ del ETH total se utilizara para finalizar una cadena diferente, eso significa que ⅓ del ETH votó de manera maliciosa, lo que sería penalizado).

### Regla de elección del fork. <a href="#block-3503c7275c154f6dbfce40cb079c6269" id="block-3503c7275c154f6dbfce40cb079c6269"></a>

Debido a los problemas en la confiabilidad de la infraestrucutra de Ethereum mencionados en la introducción de este artículo, es probable que ocurran bloques en simultáneo, mutuamente excluyentes, pero que ambos cumplan con todas las reglas. ¿Cómo se ponen de acuerdo los nodos para elegir el fork correcto o canónico?

En Gasper, la regla de elección de fork se llama "LMD-GHOST," que proviene de “**latest message-driven greedy heaviest observed sub-tree**.", en español "subárbol observado más pesado impulsado por el mensaje más reciente". Esto significa que selecciona el fork con el mayor peso acumulado de votos para que sea el canónico.

Veamos esto con los siguientes ejemplos:

* En el siguiente caso, el fork A es el canónico con 3 votos a favor y 1 en contra.

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/f9a7f529-abe7-42f8-8625-2d770b4bc9f5/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

* En el siguiente caso, el fork B es el canónico con 3 votos a favor y 2 en contra. Este es un claro ejemplo de que la cadena más larga no tiene por qué ser la canónica.

{% hint style="info" %}
Hasta ahora, entendemos cómo se votan los bloques, cómo se vuelven "finalizados" y cómo los validadores saben en qué fork construir. Esto es lo que hace Ethereum, donde un conjunto descentralizado de validadores en una infraestructura poco confiable puede crear y ponerse de acuerdo sobre qué bloque agregar a la blockchain.
{% endhint %}

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/e13e98dd-d869-46de-bf44-989b491c2653/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

## Recompensas y penalizaciones <a href="#block-6b7da8c072e9451fa039cb857bfb5104" id="block-6b7da8c072e9451fa039cb857bfb5104"></a>

Dado que el mecanismo de consenso opera en un entorno descentralizado, no podemos depender de que todos los validadores cumplan sus funciones de manera honesta. Por ello, Ethereum POS incorpora incentivos para premiar el buen comportamiento y penalizar el mal comportamiento.

### Recompensas <a href="#block-d6975f50443f46dd9e3bfd46bbd77808" id="block-d6975f50443f46dd9e3bfd46bbd77808"></a>

| Acción                                    | Frecuencia de Recompensa | Comentarios                                                                                                                                                                                                                                                       |
| ----------------------------------------- | ------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Realizar Attestations                     | Cada época               | Las attestations ocurren regularmente en cada época y representan la mayoría de la recompensa total esperada para un validador. Nota: Una attestation contiene tres votos (Head, Target, Source). Cada voto es elegible para una recompensa sujeta a condiciones. |
| Proponer un nuevo bloque                  | Varía de época en época  | Los validadores son seleccionados al azar para proponer bloques, por lo que estas recompensas varían de época en época.                                                                                                                                           |
| Participar en el comité de sincronización | Una vez cada \~22 meses  | Las recompensas del comité de sincronización ocurren con menos frecuencia, ya que los validadores, en promedio, solo forman parte de un comité de sincronización cada 22 meses. Por lo tanto, estas recompensas varían de época en época.                         |

A largo plazo, la proporción esperada de recompensas obtenidas por cada actividad se puede ver desglosada en el siguiente gráfico.

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/def4867f-9772-4813-93b1-f7d683c2b54a/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

[https://eth2book.info/capella/part2/incentives/rewards/](https://eth2book.info/capella/part2/incentives/rewards/)

### Penalizaciones <a href="#block-6581c60112e84e4d9a5fe881e2735277" id="block-6581c60112e84e4d9a5fe881e2735277"></a>

Como discutimos, las responsabilidades de un validador son hacer atestaciones, proponer bloques y participar en comités de sincronización. Por lo tanto ante fallas en una de estas responsabilidades los validadores pueden ser penalizados:

**Penalizaciones por Attestation:**

Los validadores son penalizados por attestation faltantes, tardías o incorrectas. La penalización es igual a la recompensa que habrían recibido si hubieran participado correctamente.

**Penalizaciones para los block proposers:**

* No tienen penalizaciones explícitas. Si no proponen el bloque, pierden las recompensas.

**Penalizaciones para el Comité de Sincronización:**

* Los validadores del comité de sincronización son recompensados en cada slot en el que realizan su tarea. Si firman el head incorrecto o no participan, son penalizados en la misma cantidad que habrían sido recompensados si estuvieran activos.

{% hint style="info" %}
Todas las penalizaciones se restan de los saldos de los validadores en la cadena Beacon y se queman efectivamente, reduciendo la emisión neta de ETH.
{% endhint %}

### **Slashing (recorte)** <a href="#block-c6099c09c4a748138f6da1d967410782" id="block-c6099c09c4a748138f6da1d967410782"></a>

El Slashing es una forma especial de penalización. Si un validador se involucra en un comportamiento deshonesto, su stake es slasheado, lo que implica que debe abandonar la red y se le aplica una penalización severa.

Existen tres formas en las que el stake de un validador puede ser recortado:

* Siendo block proposer: Proponer y firmar dos bloques diferentes para el mismo slot.
* Siendo atesttator: Certificar un bloque que puentea a otro (cambiando completamente la historia).
* Realizar una "doble votación" al certificar a dos candidatos para el mismo bloque.

Si se detectan estas acciones, el validador es slasheado. Esto significa que se quema el 1/32 de su ETH en stake (hasta un máximo de 1 ether) inmediatamente, y luego comienza un período de expulsión de 36 días de la red. Durante este período, el stake del validador disminuye gradualmente. En el punto medio (día 18), se aplica una penalización adicional que se calcula en función del total de ETH en stake de todos los validadores que han sido penalizados en los 36 días anteriores al evento en cuestión. Esto significa que si se penaliza a más validadores, la magnitud del recorte aumenta. El recorte máximo es el saldo efectivo completo de todos los validadores recortados (es decir, si hay muchos validadores que son recortados, podrían perder todo su stake). Por otro lado, un solo evento de recorte aislado solo quema una pequeña parte del staking del validador. Esta penalización de punto medio, que se prorratea con el número de validadores slasheados, se conoce como "penalización de correlación".

## Opciones de staking <a href="#block-5df47853f768406798d777e0b77a9bb2" id="block-5df47853f768406798d777e0b77a9bb2"></a>

Hay diferentes formas de exponerse a los rewards del stake. A continuación vamos a revisar cada una de ellas:

### Solo home staking: <a href="#block-1d7460d0759842ad916faec7206c6448" id="block-1d7460d0759842ad916faec7206c6448"></a>

El staking individual desde casa, también conocida como solo home staking, implica validar transacciones en la red Ethereum desde un hardware y configuración personal, sin delegar esta responsabilidad a terceros. Los stakers individuales son fundamentales para mantener la descentralización y seguridad de la red.

**Ventajas** La participación individual ofrece varias ventajas. En primer lugar, los stakers reciben recompensas completas por su ETH en stake, sin necesidad de compartirlas con un operador de servicios. Además, este método no requiere confiar los ETH a terceros, lo que reduce riesgos asociados a la seguridad y la deshonestidad de los operadores externos.

**Desventajas** Sin embargo, existen algunas desventajas. Los participantes deben poseer al menos 32 ETH para el depósito, además de lo necesario para cubrir los fees. También es necesario contar con una computadora dedicada que esté conectada a internet las 24 horas del día, los 7 días de la semana. Adicionalmente, se requieren conocimientos técnicos para configurar y mantener el validador, aunque cada vez existen más herramientas que simplifican este proceso.

**Educación desde Seednode** En Seednode, nos esforzamos por educar a los usuarios a través de la creación de guías, workshops y webinars. Nuestro objetivo es proporcionar las mejores prácticas y soporte para aquellos que deseen realizar solo home staking.

### **SaaS: Staking as a service** <a href="#block-f7ab2268b4a24d738a2410d28412166d" id="block-f7ab2268b4a24d738a2410d28412166d"></a>

En español puede traducirse como Staking como Servicio.

SaaS es una categoría de servicios de staking en la que se depositan 32 ETH para la validación, delegando las operaciones del nodo a un tercero. A cambio, se proporciona guía en la configuración inicial, incluida la generación de claves y el depósito, y luego se deben cargar las claves de firma al operador. Esto permite que el servicio maneje el validador en nombre del usuario, generalmente a cambio de un porcentaje de los rewards.

**Ventajas** Utilizar SaaS para stakear ofrece varias ventajas. Primero, brinda soporte en la configuración inicial, lo que puede ser especialmente útil para usuarios sin experiencia técnica. Además, delegar las operaciones del nodo a un tercero ahorra tiempo y esfuerzo, ya que el usuario no tiene que gestionar el validador por sí mismo. Finalmente, la mayoría de los proveedores de SaaS ofrecen soporte técnico continuo y actualizaciones, lo que garantiza que el validador funcione sin problemas.

**Desventajas** Sin embargo, hay algunas desventajas importantes a considerar. Todas las opciones de SaaS requieren una mayor confianza en terceros en comparación con el stake solo desde casa. Además, estas opciones pueden utilizar código adicional para los clientes de Ethereum que no es abierto ni auditable, lo que puede generar problemas de seguridad. La centralización es otra preocupación, ya que SaaS puede tener un efecto perjudicial en la descentralización de la red.

### **Staking pools** <a href="#block-db758dbe124c43bb9d694d8af1caaa20" id="block-db758dbe124c43bb9d694d8af1caaa20"></a>

Los staking pools permiten a muchas personas con pequeñas cantidades de ETH reunir los 32 ETH necesarios para activar un validador. Dado que el protocolo de Ethereum no permite esta funcionalidad por defecto, diferentes organismos han creado soluciones para satisfacer esta necesidad.

Algunos pools operan utilizando contratos inteligentes, que gestionan y rastrean el stake de los usuarios de manera confiable y emitiendo un token que representa el valor aportado. Otras pools pueden funcionar offchain, mediando las operaciones de forma manual.

**Ventajas:**

* **Baja barrera de entrada:** La mayoría de los pools permiten a los usuarios participar con cualquier cantidad de ETH, a diferencia del stake individual que requiere 32 ETH.
* **Facilidad de participación:** Unirse a una pool es tan sencillo como un swap de tokens. No es necesario preocuparse por la configuración del hardware ni el mantenimiento del nodo. Los operadores de nodos gestionan los validadores y las recompensas se distribuyen a los contribuyentes, descontando por supuesto una comisión.
* **Token de staking liquido (LST):** Muchos pools emiten un token que representa una garantia sobre el ETH stakeado y las recompensas generadas. Esto permite usar el LST como cualquier otro token dentro de la red, incluso como colateral en aplicaciones DeFi.

**Desventajas:**

Aunque estos servicios emiten los LST, estos tokens pueden no ser tan seguros ni fiables como tener el ETH directamente. Dependiendo del diseño del contrato inteligente o del modelo de operación, los usuarios podrían enfrentar problemas de liquidez o devaluación del token en ciertas circunstancias.

**Lido:**

Lido es la opción más conocida y utilizada dentro de esta categoría.

Para obtener más información sobre Lido y sus protocolos, se puede consultar la documentación:

[**Introduction | Lido Docs**This documentation is intended to introduce the general user to the project, as well as to serve as a guide for anyone who may be developing software using Lido.docs.lido.fi](https://docs.lido.fi/)

### **Exchanges Centralizados** <a href="#block-1106881a6bd24957ac3b89df4abc5d9c" id="block-1106881a6bd24957ac3b89df4abc5d9c"></a>

Muchos exchanges ofrecen servicios de stake. Estos pueden ser útiles para usuarios que aún no se sienten cómodos con la autocustodia. Estos servicios permiten obtener rendimiento de los ETH con un mínimo de esfuerzo.

**Ventajas** La principal ventaja de estos servicios es la facilidad de uso para el usuario.

**Desventajas y Riesgos** Sin embargo, hay riesgos importantes a considerar. La consolidación de grandes cantidades de ETH en un solo proveedor centralizado crea un gran blanco y un punto de fallo, lo que hace que la red sea más vulnerable a ataques. Esto puede afectar negativamente a la seguridad y la descentralización de la red Ethereum.

Por otro lado también están los riesgos propios del exchange (not your keys not your coins)

#### Comparación de Opciones de Participación <a href="#block-7f7c7f6ea0e74f449a04a2a99ca3c5f3" id="block-7f7c7f6ea0e74f449a04a2a99ca3c5f3"></a>

Aquí se resumen algunos de los riesgos, recompensas y requisitos de las diferentes maneras en que puede realizar staking.

| **Opción**                  | **Recompensas**                                                                                                                                      | **Riesgos**                                                                                                                                                 | **Requisitos**                                                                                                                                                  |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Staking solo**            | -Recompensas máximas.                                                                                                                                | - Hay penalizaciones por desconectarse - Un comportamiento malicioso puede ocasionar slashing de grandes cantidades de ETH y la expulsión forzada de la red | - Depositar 32 ETH - Mantener hardware que ejecute los clientes de Ethereum mientras esté conectado a internet - Conocimientos técnicos para seguir el proceso. |
| **SaaS**                    | - Se restan recompensas para pagar una comisión por el servicio                                                                                      | - Los mismos riesgos que en la participación individual - Riesgo de la contraparte proveedora del servicio                                                  | - Depositar 32 ETH y generar sus claves con ayuda                                                                                                               |
| **Staking Pools**           | - Los LST representan los ETH stakeados y las recompensas obtenidas - Puede guardar los LST en su wallet, usarlos en DeFi o venderlos en el mercado. | - Riesgos de smart contracts - Riesgos de liquidez                                                                                                          | - Solo el ETH que quiera utilizar                                                                                                                               |
| **Exchanges Centralizados** | - Recompensas moderadas                                                                                                                              | - Dependencia total del exchange para la seguridad y usabilidad de los fondos                                                                               | - Solo el ETH que quiera utilizar                                                                                                                               |

## Conclusiones <a href="#block-7ff230e8cfa44d0bb93ef1fa969501fd" id="block-7ff230e8cfa44d0bb93ef1fa969501fd"></a>

Una forma grafica e interesante de ver como los incentivos están bien alineados en Ethereum es mediante este grafico:

<figure><img src="https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/9f8fde69-949f-4ef3-a4f9-c25c0252603a/Untitled/w=3840,quality=90,fit=scale-down" alt=""><figcaption></figcaption></figure>

Donde podemos verificar que se esta creando un bloque cada 12 segundos, tal cual esta pensando que debería pasar.

Podemos concluir que Ethereum ha logrado construir un sistema de consenso sofisticado y resistente que continúa evolucionando para satisfacer las demandas de una infraestructura descentralizada y segura.
