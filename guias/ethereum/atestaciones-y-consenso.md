# Atestaciones y consenso

{% embed url="https://mirror.xyz/seedlatam.eth/9_U994MzJXTDcbmGIDRPsel2KJky0kD1RjaJwJrFx2w" %}

### Introdución <a href="#heading-introducion" id="heading-introducion"></a>

¿Cuál es el pilar de la seguridad de Ethereum? ¿Los nodos, su ejecución o las atestaciones?

Antes de continuar, los invitamos a leer un artículo previo sobre el Proof of Stake [“PoS - De Full Node a Validator Node](https://mirror.xyz/seedlatam.eth/ddwMgu\_S0Ie5T7V0KWxSjx\_-AsXm7mQGQ4-2jXFRIrg)”, donde encontrarán una clara explicación sobre el proceso de una transacción realizada por nodo validador.

Para lo que respecta a este artículo, profundizaremos en la perspectiva de un validador respecto al estado de la cadena de bloques. Comprender el papel del validador es esencial, ya que este no sólo valida la información sino que también se encarga de difundir bloques con datos ya verificados. Esta es la esencia de su punto de vista, donde la atestación juega un papel crucial. Al final de este artículo, tendrán un entendimiento claro de qué es una atestación, cómo funciona, por qué es necesaria, y cuál es su importancia en la seguridad de la cadena.

## ¿Consensuar o no consensuar? <a href="#heading-consensuar-o-no-consensuar" id="heading-consensuar-o-no-consensuar"></a>

El mecanismo de Proof of Stake es parte del consenso de la red, permitiendo a los validadores acordar la veracidad de las transacciones y la creación de nuevos bloques en la cadena.

Más allá del Proof of Stake, existen otros aspectos del consenso que no abordaremos en detalle aquí, aunque es crucial entender que PoS ofrece resistencia a ataques Sybil, no siendo exactamente un protocolo de consenso. Los protocolos de consenso son LLMD Ghost y Casper FFG, que juntos formaron “Gasper”.

Siguiendo, el consenso en Ethereum se lleva a cabo por el algoritmo, Gasper. que detalla el funcionamiento de los incentivos en la red, cómo se añaden nuevos bloques a la cadena, y cómo los nodos determinan la cadena canónica (la cadena con mayor cantidad de bloques). Este algoritmo también establece los intervalos temporales para eventos en la cadena dividiendo el tiempo en "slots" de 12 segundos y _epochs_ de 32 slots, y cómo un validador asume el rol de proponedor de bloques.

En este artículo, nos enfocaremos específicamente en el papel del cliente validador, encargado de avalar el bloque actual en el bloque header de la cadena y los bloques “checkpoints”, que marcan el último bloque al inicio de la época.

## El punto de vista del validador <a href="#heading-el-punto-de-vista-del-validador" id="heading-el-punto-de-vista-del-validador"></a>

Para entender el punto de vista del validador, tenemos que entender qué significa atestar.

Atestar es la acción que llevan a cabo los validadores de un comité, en cada slot, para confirmar que un bloque es válido y puede ser incorporado a la cadena. Cada atestación se emite por un slot específico y se propone por época. Su objetivo es respaldar el punto de vista del validador. ¿Qué implica esto? Significa que el validador confirma que el estado de la cadena refleja la misma información que el bloque verificado, y la atestación permite que otros validadores voten a favor o en contra de esta percepción, apoyando así el punto de vista del validador. Al lograr un consenso entre los demás validadores, se confirma el bloque propuesto.

Las atestaciones de cada validador individual se recopilan inicialmente dentro de subredes, antes de su propagación más amplia, debido a la carga que supone compartir esta información. Por ello, cada atestación incluye datos de consenso, su firma y la de otros validadores que coinciden con la información. El "agregador", un validador seleccionado de cada subred en cada época, recolecta atestaciones con datos de la “gossip network” y luego difunde la atestación agregada (attestation aggregate) a una red más amplia. Posteriormente, estas atestaciones son recopiladas por el “creador de bloques" (block proposer) en cada slot, formando un "paquete" de datos.

Una agregación está compuesta de la siguiente forma:

**aggregation\_bits**: una lista de bits de validadores donde la posición marca el index del validador en su comité seguido del valor (0/1) que indica si está de acuerdo con la información.

**signature**: una firma BLS que suma la firma de validadores individuales.

**data**: los siguientes detalles de la atestación

<figure><img src="https://images.mirror-media.xyz/publication-images/2yJXjYOy89syDkNjuZFop.png" alt=""><figcaption></figcaption></figure>

**Detalles:**

**slot**: el número de slot de la atestación

**index**: el número correspondiente al comité del validador según el slot.

**beacon\_block\_root**: Root hash del bloque que el validador ve al bloque header de la cadena.

**source**: Parte de la finalidad del voto que indica que el validador ve el bloque justificado más reciente.

**target**: Parte de la finalidad del voto que indica que el validador ve como el primer bloque en la época actual.

El validador que atesta primero debe construir la data con la información detallada anteriormente, una vez que la data es construida el validador puede cambiar el index del bit de “aggregation\_bits” correspondiente a su validador para demostrar que han participado en el voto.

Finalmente el validador firma la atestación y la propaga a la red.

## El ciclo de vida de la atestación <a href="#heading-el-ciclo-de-vida-de-la-atestacion" id="heading-el-ciclo-de-vida-de-la-atestacion"></a>

**Generation**: Los validadores crean atestaciones usando los datos especificados previamente.

**Propagation**: Estas atestaciones se difunden a los agregadores a través de la red de difusión (gossip network).

**Aggregation**: El agregador combina su firma con las de otros validadores en la atestación.

**Propagation**: La atestación agregada se distribuye a través de la gossip network y es recolectada por el validador que produce los bloques (block producer). Cada productor recopila varias atestaciones y formará un bloque con las mismas.

**Inclusión**: El block proposer agrega el bloque finalizado a la cadena canónica.

<figure><img src="https://images.mirror-media.xyz/publication-images/W0b6g1X9GIpEN5psRdh8E.png" alt=""><figcaption></figcaption></figure>

**El triple propósito:**

* Casper FFG voto
* Estabilizar la opción de fork cortoplacista de bloque por bloque al votar en el bloque header actual
* Dividir la votación de los bloques

**Escenario de atestaciones:**

* **Oportunidad de voto perdida**

Los validadores tienen un máximo de una época para asumir su atestación. Si la pierden en la época 0, pueden subirla con inclusión delay en la época 1.

1. **Agregadores perdidos**

Validadores random se suscriben a dos subredes como backup en caso de que se pierda alguno de los 16 agregadores que hay por época en total.

1. **Productores de bloques perdidos**

Si el productor de bloques se pierde, el siguiente productor será quien elija la atestación agregada y la incluya en el siguiente bloque. Esto provocara que la inclusion delay aumente 1 punto.

#### No pain No gain <a href="#heading-no-pain-no-gain" id="heading-no-pain-no-gain"></a>

Los validadores son recompensados por sus atestaciones, con la cantidad de recompensa variando según las banderas de participación (source, target, head), la base de recompensa y la tasa de participación.

Cada bandera de participación puede ser verdadera o falsa, dependiendo de la presentación en la atestación y el retraso de inclusión. El escenario ideal es cuando las tres banderas son verdaderas.

Inclusion Delay: Es el lapso entre el momento esperado de la atestación y cuando efectivamente se incluye en la cadena.

Si el bloque header de la cadena es el “bloque n”, y la atestación es incluida en el “bloque n+1” por lo que la inclusión es 1 en su mayoría de veces. Si por alguna razón, la tardanza se duplica, la recompensa será menor.

**Esto es particularmente relevante para los nodos validadores en Latinoamérica, donde la escasez de nodos puede incrementar el retraso de inclusión (inclusion delay) y, por ende, disminuir las recompensas. Esta es una de las razones por las cuales es importante contar con más nodos validadores en LATAM, ya que con más nodos se puede mejorar la comunicación entre los mismos alrededor del mundo y reducir la inclusión delay.**

Así como hay posibilidades de ganar, también existen riesgos de perder. Un validador puede ser penalizado, o "slasheado", perdiendo una parte de su participación económica si realiza atestaciones contradictorias.

#### BLS Signature <a href="#heading-bls-signature" id="heading-bls-signature"></a>

BLS (Boneh-Lynn-Shacham) es una firma digital utilizada para firmar las transacciones de los usuarios.

El proceso de esta firma está compuesto por 4 piezas de información:

**La llave secreta**: Cada validador tiene una llave secreta, a veces llamada llave privada. Esta llave es utilizada para firmar los mensajes propagados y mantenerlos en secreto utilizando criptografía.

**La llave pública**: Esta llave deriva de la llave secreta. Esta llave representa la identidad de un validador y es conocida por todo el mundo.

**El mensaje**:

**La firma**: Es el resultado del proceso de firma. La firma es creada al combinar el mensaje con la llave secreta.

Algo importante de este proceso es que el mensaje, su firma y una llave pública permiten que verifiques el validador que firmó el mensaje ya que la llave pública deriva de la llave privada por lo que nadie más podría haber firmado ese mensaje.

#### Además de la atestación <a href="#heading-ademas-de-la-atestacion" id="heading-ademas-de-la-atestacion"></a>

**Firmas**

En la cadena Beacon, los únicos mensajes que son firmados son “hash tree roots” de objetos llamados “signing roots”

<figure><img src="https://images.mirror-media.xyz/publication-images/dQyU5Lhd1ofpn6wzXtRz9.png" alt=""><figcaption></figcaption></figure>

(Un validador aplica su llave secreta a un mensaje que genera una firma digital única)

**Verificación**

Para verificar una firma necesitamos conocer la llave pública del validador que la firmó. Cada validador tiene su llave pública guardada en el estado de la beacon chain y puede ser fácilmente consultada con el índice del validador.

La verificación de firmas puede ser tratada como una caja negra: enviamos el mensaje, la llave pública y la firma del verificador, luego de magia criptográfica las firmas coinciden con la llave pública y el mensaje por lo que lo declaramos válido. Si esto no sucede, la firma está corrompida, la llave secreta fue errónea o el mensaje no fue lo que se firmó;

<figure><img src="https://images.mirror-media.xyz/publication-images/ASSuj2Qj2FMgp68achKaV.png" alt=""><figcaption></figcaption></figure>

La verificación volverá como válida sólo si la firma corresponde a la llave pública y al mensaje, si no volverá como falso.

A continuación podemos encontrar una representación de todo el proceso:

<figure><img src="https://images.mirror-media.xyz/publication-images/l0uzfiEwrIDQJH9oRFPvc.png" alt=""><figcaption></figcaption></figure>

## Penalidades & Slashing <a href="#heading-penalidades--slashing" id="heading-penalidades--slashing"></a>

Penalidades

Los validadores reciben recompensas por su contribución a la seguridad de la cadena y enfrentan penalizaciones por acciones que van en contra de esta. Aunque estas sanciones suelen ser moderadas, cumplen con el propósito de incentivar a que los validadores mantengan un desempeño óptimo en sus operaciones.

Estas penalizaciones se descuentan de los saldos de los validadores en la beacon chain y se eliminan de la circulación, contribuyendo así a la reducción de la emisión total de la cadena.

Importante: Las penalidades no son lo mismo que el slashing

El rango para que un validador mantenga un equilibrio entre ganancias y penalizaciones es aproximadamente del 43%.

#### **Tipos de Penalizaciones:** <a href="#heading-tipos-de-penalizaciones" id="heading-tipos-de-penalizaciones"></a>

**Penalizaciones por Atestación**: Se aplican a las atestaciones ausentes, tardías o incorrectas. Las penalizaciones incluyen votos perdidos en el marco de Casper FFG, específicamente votos de fuente o objetivo. No se penaliza la ausencia de un voto de head.

**Penalizaciones del Comité de Sincronización**: Aquellos en el comité de sincronización que no firmen el bloque líder correcto, o no participen en absoluto, enfrentan una penalización idéntica a la recompensa que habrían recibido por actuar correctamente.

_**Nota**_\*: No existen penalizaciones específicas dirigidas a los proponentes de bloques en cuanto a la inclusión de depósitos de la cadena Eth1, aunque omitir depósitos conocidos por la red invalida el bloque propuesto, sirviendo esto como incentivo para su inclusión.\*

**Slashing**

El Slashing se aplica a acciones que violan reglas críticas del protocolo, potencialmente amenazando la seguridad de la cadena. La expulsión implica la pérdida significativa de participación y la exclusión del protocolo, representando más un castigo severo que una simple penalización. Los validadores pueden evitar la expulsión tomando medidas preventivas básicas.

Las acciones que pueden llevar a la expulsión incluyen:

* Atestar de manera contradictoria para el mismo punto de control objetivo.
* Proponer múltiples bloques para la misma altura o atestar por distintos bloques de cabeza con los mismos puntos de control de fuente y objetivo, acciones que pueden ser interpretadas como intentos de manipulación o ataque.

Estas infracciones están relacionadas con la "equivocación", es decir, cuando un validador emite información contradictoria a lo previamente anunciado a la red.

Al igual que con las penalizaciones menores, los fondos retirados de los validadores por expulsión son eliminados de la circulación, afectando la emisión total de la cadena de balizas.

## Conclusión <a href="#heading-conclusion" id="heading-conclusion"></a>

Finalmente, al analizar el rol de los validadores en el mecanismo de consenso y las atestaciones podemos darnos cuenta de que son fundamentales para mantener la seguridad y la integridad de la red de Ethereum.

Las atestaciones no sólo facilitan la verificación y validación de los bloques dentro de la cadena, si no que también sostienen el consenso alrededor del estado actual de la red, reforzando su resistencia contra ataques y manipulaciones.

Las sanciones y penalizaciones, terminan siendo cruciales para incentivar a los validadores a mantener un alto nivel de rendimiento y compromiso. Es importante destacar la distinción entre ambas acciones para que exista una diferencia entre quien comete un error o corre su nodo validador en un estado de bajas condiciones (mal internet, alta inclusión delay, cortes de luz, etc.). No es lo mismo tener una conducta responsable en un “mal ambiente” que una gran negligencia por sobre cómo debemos actuar con nuestro nodo.

La efectividad de PoS y la seguridad de Ethereum dependen en gran medida de la precisión, puntualidad y honestidad (por sobre todo) de las atestaciones producidas por los validadores.

A través de un sistema de incentivos, la red puede asegurar que sus participantes actúen de manera que promueva el bienestar de la comunidad y el ecosistema.

## Fuentes <a href="#heading-fuentes" id="heading-fuentes"></a>

[How does the NEW Ethereum work?When Ethereum swapped out Proof-of-Work consensus with Proof-of-Stake, we called it "the Merge." Ever since, Ethereum hasn't nee…www.preethikasireddy.com](https://www.preethikasireddy.com/post/how-does-the-new-ethereum-work)

[Entity metrics drill-down | Rated DocsDefinitions of variables that appear in the Pool, Operator, Addresses and Validator drill-down pages of the Rated Explorer.docs.rated.network](https://docs.rated.network/explorer/ethereum/network-views/pools-operators-and-validators/entity-metrics-drill-down#inclusion-delay)

[Attestations | ethereum.orgA description of attestations on proof-of-stake Ethereum.ethereum.org](https://ethereum.org/en/developers/docs/consensus-mechanisms/pos/attestations/#what-is-an-attestation)

[annotated-spec/phase0/beacon-chain.md at master · ethereum/annotated-specVitalik\&#39;s annotated eth2 spec. Not intended to be \&quot;the\&quot; annotated spec; other documents like Ben Edgington\&#39;s https://benjaminion.xyz/eth2-annotated-spec/…github.com](https://github.com/ethereum/annotated-spec/blob/master/phase0/beacon-chain.md#attestationdata)

[Pragmatic signature aggregation with BLSTLDR: We suggest using BLS signature aggregation as a pragmatic medium-term solution to the signature verification bottleneck of…ethresear.ch](https://ethresear.ch/t/pragmatic-signature-aggregation-with-bls/2105)

[Upgrading Ethereum | 2.3.1 PreliminariesA technical handbook on Ethereum's move to proof of stake and beyond.eth2book.info](https://eth2book.info/capella/part2/consensus/preliminaries/)

[The Beacon Chain Ethereum 2.0 explainer you need to read firstEthereum’s Beacon Chain and Proof-of-Stake from validators to finality. Illustrated with examples at the right level to make you…ethos.dev](https://ethos.dev/beacon-chain)
