# #1 Claves seguras para nodos validadores

{% embed url="https://mirror.xyz/seedlatam.eth/alffxS1TAN1un8Nt0n7zgarc1nvi01KDvz_80pEXTFQ" %}

## Primera parte: <a href="#heading-primera-parte" id="heading-primera-parte"></a>

El presente artículo se divide en las siguientes partes:

1. Introducción: se mencionan las características principales de las claves utilizadas para los validadores.
2. Generación de las claves: aproximación del problema.
3. Claves perdidas: ¿Qué puede pasar en cada caso?
4. Conclusión

En la segunda parte se desarrollará un procedimiento para generar de forma segura un set de claves para un validador.

***

### **1. Introducción** <a href="#heading-1-introduccion" id="heading-1-introduccion"></a>

A diferencia de una cuenta normal de Ethereum, que solo necesita un único par de claves para gestionar sus fondos, y cuando decimos “par de claves” hacemos referencia a una clave pública y a una clave privada; un validador deberá usar dos pares de claves: “signing key” y “withdrawal key”. Al referirse a “BLS keys”, se está hablando de estas claves.

> _La sigla BLS viene del esquema criptográfico utilizado Boneh-Lyn-Shacham, la curva elíptica utilizada es BLS12–381.En muchos textos vamos a encontrar a “signing key” y “validator key” como sinónimos, en este articulo para que no se produzcan dudas los mencionaremos cada uno por su nombre._

<figure><img src="https://images.mirror-media.xyz/publication-images/VCoz9EgQXvfR-82FcJmM5.png" alt=""><figcaption></figcaption></figure>

Antes de explicar la utilidad de estas claves, es importante entender que es “**Withdrawal credentials”** en un validador.

#### **Withdrawal credentials** <a href="#heading-withdrawal-credentials" id="heading-withdrawal-credentials"></a>

El 12/04/2023, se ejecutó con éxito el _hard fork Shanghai & Capella_, lo que habilitó la funcionalidad de retiro para los validadores en la red. Antes de esa fecha, por defecto, no se configuraba ninguna dirección de retiro. Era una funcionalidad pendiente. No se podían retirar los ETH en _stake_, ni se configuraba (al menos por defecto) una dirección de retiro.

Esto quedaba indicado en el registro _Withdrawal credentials_. Este es un campo de 32 bytes que puede empezar con 0x00 o 0x01.

* Si **Withdrawal credentials** comienza en 0x00 significa que **no** hay una dirección de retiro configurada.

<figure><img src="https://images.mirror-media.xyz/publication-images/fwJa7nQyPknFDGHWCwVPN.png" alt=""><figcaption></figcaption></figure>

* Si **Withdrawal credentials** comienza con 0x01 quiere decir que **si** hay una dirección de retiro configurada.

<figure><img src="https://images.mirror-media.xyz/publication-images/b74QnBykqH7bKhLMAz6uP.png" alt=""><figcaption></figcaption></figure>

\*En el siguiente enlace, podrán observar los depósitos de todos los validadores. Podrán notar que entre los validadores activos más recientes, todos tienen **withdrawal credential** comenzando en 0X01, pero aún hoy se pueden encontrar muchos que aún comienzan en 0X00. \*

_Pueden ordenar la tabla desde las columnas para observar esto rápidamente:_ [_Beaconcha_](https://beaconcha.in/validators/deposits)

Al día de la fecha, se aconseja que todos los validadores nuevos, en su configuración inicial, agreguen su dirección de retiro, y los antiguos deben actualizar el campo _Withdrawal credentials_ y agregarla.

Una vez configurada la dirección de retiro, esta es IMPOSIBLE de modificar.

#### **Signing Key:** <a href="#heading-signing-key" id="heading-signing-key"></a>

La función de esta clave es firmar operaciones on-chain (proponer y validar bloques). Dado que esta operación debe realizarse al menos una vez por epoch (cada 6.5 minutos), es el cliente (software) el que debe custodiar la clave. Por su funcionalidad se observa que esta clave privada debe ser gestionada, al menos en un primera instancia, por una HOT WALLET.También tiene la potestad de fimar el “retiro voluntario” de los 32 ETH en stake en caso de existir una dirección de retiro.

#### **Withdrawal key** <a href="#heading-withdrawal-key" id="heading-withdrawal-key"></a>

Esta clave tiene la capacidad de firmar la actualización de “Withdrawal credentials” si aún no se ha realizado. Como se explicó, esto, en última instancia, implica tener la potestad de controlar los fondos de un validador (los 32xN ETH en garantía y excedentes).

#### **Frase mnemónica** <a href="#heading-frase-mnemonica" id="heading-frase-mnemonica"></a>

Se puede generar una frase mnemónica de la lista de palabras [BIP39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki), popularmente conocida como “las 24 palabras”, que derive en una seed para generar tanto el _“signing key”_ como el _“withdrawal key”_. Esto significa que ambas claves pueden derivarse de las mismas 24 palabras (EIPs [2333](https://eips.ethereum.org/EIPS/eip-2333) y [2334](https://eips.ethereum.org/EIPS/eip-2334)).

Sin entrar en detalles, en el EIP 2334 se explica cómo deben ser las rutas de derivación (derivation path) para ambas claves:

\*El path para el **withdrawal key** es \*`m/12381/3600/i/0` donde`i` indica `i-esimo` par de las validator keys.

\*El path para el **signing key** es \*`m/12381/3600/i/0/0` donde nuevamente, `i` indica el `i-esimo` par de las validator keys.

Para aquellos que no están familiarizados con las rutas de derivación, lo más importante es observar que estas crecen de izquierda a derecha. \
Para ejemplificar, podemos decir que m/1/0 y m/1/1 son ramas derivadas de m/1.

Observar que “signing key” es una rama derivada del path del “withdrawal key”.

_\*NOTA: El número 12381 se tomó arbitrariamente en el EIP2334 como referencia a la curva elíptica BLS12–381. El número 3600 también se tomó arbitrariamente (60\*60), ya que el path de una wallet de usuario de Ethereum es en la mayoria de las implementaciones_ `m/44/60/0/0` (BIP44, siendo 60 el número asignado para ETH). Para más detalles ver el EIP2334.

Podemos observar la cantidad “ilimitada” de validator key que podemos generar por cada seed. En el [link](https://iancoleman.io/eip2333/) que les dejamos aquí podrán hacer esta prueba.\
\
Las capturas que compartimos a continuación tienen la intención de ilustrar este mismo punto: que con la misma frase mnemónica podemos manejar cantidad “ilimitada” de validadores.

<figure><img src="https://images.mirror-media.xyz/publication-images/DKBMAEw8-BgDPW7WZ80ia.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://images.mirror-media.xyz/publication-images/pwMjQrxZ3FTfWLCQ6LTEM.png" alt=""><figcaption></figcaption></figure>

Cabe mencionar que, al momento de escribir este artículo, no hay HW que cree las claves BLS.

#### **Keystores** <a href="#heading-keystores" id="heading-keystores"></a>

Llegado a este punto, ya logramos entender que necesitamos el conjunto de claves del “validator key”. Hay varias herramientas que pueden generar estas claves a partir de la frase mnemónica. \
\
Algunas de estos generadores de clave son:

* [https://github.com/ethereum/staking-deposit-cli/](https://github.com/ethereum/staking-deposit-cli/)
* [https://wagyu.gg/](https://wagyu.gg/)
* [https://github.com/wealdtech/ethdo](https://github.com/wealdtech/ethdo)
* [https://ava.do/](https://ava.do/)

En la segunda parte de este artículo exploraremos las buenas prácticas para tal tarea utilizando staking-deposit-cli.

Estos generadores de contraseñas crean archivos de almacenamiento seguro que los clientes de los validadores utilizan para intercambiar las claves. \
Durante el proceso de creación del nodo en el [Launchpad](https://launchpad.ethereum.org/en/), se nos pedirá que subamos estos archivos. Archivos que luego quedarán almacenados de forma local en la máquina del validador.

Tales archivos son:

1. Keystore file \[JSON]
2. Deposit data file \[JSON]

El “keystore file” va a gestionar la capacidad del validador para firmar transacciones, quiere decir que este archivo tiene el “_signing key_”. Este archivo debe ser importado al validador, comportándose este como HOT WALLET. El keystore file DEBE estar encriptado con una clave FUERTE. Pueden ser almacenados y transferidos al validador de “manera segura”. Nunca se debe almacenar la clave en el mismo lugar que el archivo.

Por otro lado el “Deposit data file” contiene la clave pública asociada al validador. Aquí esta la “identidad” del validador. Esta información NO es secreta.

_Podemos ver que el “withdrawal key” no se guarda en el keystore, por lo tanto para recuperarlo si o si necesitamos la frase mnemónica utilizada._

#### **Withdrawal address (dirección de retiro)** <a href="#heading-withdrawal-address-direccion-de-retiro" id="heading-withdrawal-address-direccion-de-retiro"></a>

Es la cuenta de Ethereum usada para retirar el ETH en stake. El dueño del validador debe tener control absoluto sobre esta cuenta.

***

### **2. Generación de las claves: aproximación del problema.** <a href="#heading-2-generacion-de-las-claves-aproximacion-del-problema" id="heading-2-generacion-de-las-claves-aproximacion-del-problema"></a>

#### 1. Frase mnemónica <a href="#heading-1-frase-mnemonica" id="heading-1-frase-mnemonica"></a>

Es una frase BIP39. Por lo tanto podemos utilizar cualquier generador de entropía conocido del ecosistema siempre y cuando sea OFFLINE.

* Se podría generar directamente la herramienta de generación de claves.
* Se podría generar usando una HW.
* Podríamos generar nuestra propia entropía, por ejemplo lanzando una moneda 256 veces y la ultima palabra (el checksum) generándola desde alguna herramienta OFFLINE.

NO SE RECOMIENDA UTILIZAR UN HOT WALLET PARA GENERAR LA FRASE MNEMÓNICA.

#### 2. Keystore <a href="#heading-2-keystore" id="heading-2-keystore"></a>

En este punto, no importa qué tan segura sea nuestra frase mnemónica; si la utilizamos en una computadora con conexión a internet, pasa a ser vulnerable. Por eso, la recomendación es utilizar este cliente de forma totalmente offline e importar los archivos de forma segura. En la segunda parte se brindará un tutorial paso a paso de cómo realizarlo.

NUNCA ESCRIBIR LA FRASE MNEMÓNICA EN UNA MAQUINA CON CONEXION A INTERNET.

#### **3. Cuenta de Ethereum para withdrawal address** <a href="#heading-3-cuenta-de-ethereum-para-withdrawal-address" id="heading-3-cuenta-de-ethereum-para-withdrawal-address"></a>

Es indispensable saber que una vez asociada esta dirección, NO se puede modificar. Aquí son válidas todas las recomendaciones generales para la seguridad de una cuenta de Ethereum. Las opciones más seguras son mantener la frase mnemotécnica offline, utilizar una billetera de hardware u otra opción de seguridad comprobada, como una dirección Gnosis Safe.

NO UTILIZAR UNA CUENTA DE EXCHANGE.

***

## **3. Claves perdidas.** <a href="#heading-3-claves-perdidas" id="heading-3-claves-perdidas"></a>

Para intentar abarcar la mayor cantidad de posibilidades se responden alguna de las preguntas mas comunes que podrían aparecer.

#### **¿Qué sucede si se compromete la frase mnemónica?** <a href="#heading-que-sucede-si-se-compromete-la-frase-mnemonica" id="heading-que-sucede-si-se-compromete-la-frase-mnemonica"></a>

El validador deja de ser completamente nuestro. Es por eso que es crucial tener configurada la dirección de retiro (ya que esta operación es irreversible y no se puede cambiar). Además, existe el riesgo de sufrir un ataque que resulte en un “slashing”, lo que conllevaría a la pérdida de 1 o más ETH y/o la expulsión forzosa de la red. En este caso, intentar salvaguardar los fondos se convierte en una acción urgente.

#### ¿**Qué sucede si se compromete el archivo keystore-\*.json y la contraseña del mismo?** <a href="#heading-que-sucede-si-se-compromete-el-archivo-keystore-json-y-la-contrasena-del-mismo" id="heading-que-sucede-si-se-compromete-el-archivo-keystore-json-y-la-contrasena-del-mismo"></a>

Un potencial usuario malintencionado puede realizar un ataque que resulte en un “slashing”, lo que podría provocar la pérdida de 1 o más ETH y/o la expulsión forzosa de la red.Si solo se ha comprometido el archivo pero no la contraseña, el riesgo, aunque menor, debe considerarse igualmente grave. En ambos casos, es crucial actuar de inmediato.

#### **¿Qué sucede si se compromete el archivo deposit\_data-\*.json?** <a href="#heading-que-sucede-si-se-compromete-el-archivo-deposit_data-json" id="heading-que-sucede-si-se-compromete-el-archivo-deposit_data-json"></a>

Se exponen algunos datos privados (no secretos), como la clave pública del validador y todo lo asociado con ella. Esto representa más un problema de privacidad que otra cosa.

#### **¿Qué sucede si se compromete la cuenta asociada a “withdrawal address?** <a href="#heading-que-sucede-si-se-compromete-la-cuenta-asociada-a-withdrawal-address" id="heading-que-sucede-si-se-compromete-la-cuenta-asociada-a-withdrawal-address"></a>

Esta dirección NO se puede cambiar. En el momento en que retiremos el ETH en stake, estaremos expuestos a cualquier potencial usuario malintencionado que haya obtenido acceso a esta cuenta. Se podría perder todo lo retirado.

#### **¿Qué sucede si se pierde la frase mnemónica?** <a href="#heading-que-sucede-si-se-pierde-la-frase-mnemonica" id="heading-que-sucede-si-se-pierde-la-frase-mnemonica"></a>

Existen varias posibilidades:

1. En caso de tener seteadada una “withdrawal adress” (_Withdrawal credentials_ \*\*\*\*comienza en 0x01), de tener el archivo keystore-\*.json y la contraseña del mismo se pueden retirar los ETH en stake e incluso se puede seguir usando el validador. Lo recomendable seria empezar un nuevo inmediatamente y resguardar la frase mnemónica.
2. En caso de no tener seteada una “withdrawal address” (_Withdrawal credentials_ comienza en 0x00), se puede seguir operando el validador, pero los 32 ETH en stake estan perdidos para siempre.
3. Si se ha configurado una “withdrawal address” pero no se dispone del archivo keystore-\*.json (es decir, no se cuenta con una copia local del validador ni con una copia de seguridad), se considera una pérdida total de los ETH en stake.

#### **¿Qué sucede si se pierde el archivo keystore-\*.json?** <a href="#heading-que-sucede-si-se-pierde-el-archivo-keystore-json" id="heading-que-sucede-si-se-pierde-el-archivo-keystore-json"></a>

Se puede recuperar desde la frase mnemónica.

#### **¿Qué sucede si se pierde el archivodeposit\_data-\*.json?** <a href="#heading-que-sucede-si-se-pierde-el-archivodeposit_data-json" id="heading-que-sucede-si-se-pierde-el-archivodeposit_data-json"></a>

Se puede recuperar desde la frase mnemónica.

#### **¿Qué sucede si se pierde la cuenta asociada a “withdrawal address?** <a href="#heading-que-sucede-si-se-pierde-la-cuenta-asociada-a-withdrawal-address" id="heading-que-sucede-si-se-pierde-la-cuenta-asociada-a-withdrawal-address"></a>

Nunca vamos a poder recuperar el ETH en stake.

***

## **4. Conclusiones** <a href="#heading-4-conclusiones" id="heading-4-conclusiones"></a>

Recapitulando lo visto hasta ahora, tenemos:

* Es crucial generar y guardar una o idealmente dos frases mnemónicas de manera segura, una para el retiro y otra para la clave del validador.
* Debemos utilizar un software para crear la clave del validador a partir de una frase mnemónica.
* El archivo keystore-\*.json, que contiene la clave de firma, debe almacenarse en una máquina conectada a Internet para el funcionamiento normal del validador.

**Te invitamos a seguir leyendo la segunda parte para obtener una guía completa sobre la generación y gestión segura de claves para validadores de Ethereum.**

En esta [segunda parte](https://mirror.xyz/seedlatam.eth/NE-iXSkA4\_PCZpBWW\_r4EhmTxXjilCytX3IxQEKi4KQ) exploraremos estos puntos en detalle, desarrollaremos las formas más simples de realizar estas tareas de manera segura y mencionaremos otras alternativas.

Abordar la seguridad en profundidad requiere un análisis extenso debido a sus diversas facetas. Estos artículos no tienen la intención de ser una guía definitiva ni establecer un único enfoque para abordar la seguridad. Su objetivo principal es exponer el problema de la gestión de claves para validadores de Ethereum y ofrecer una alternativa segura para afrontarlo.

Es importante recordar que la seguridad es un proceso continuo que requiere atención constante.
