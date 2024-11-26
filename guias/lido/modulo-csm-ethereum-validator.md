# Módulo CSM - Ethereum Validator

{% hint style="info" %}
El **Módulo de Staking Comunitario (CSM)** es un módulo de Lido en Ethereum con entrada permissionless, lo que permite que cualquier operador de nodos —y especialmente los **stakers de la comunidad**, desde individuales, grupos de amigos, hasta operadores amateurs— pueda operar validadores al proveer un depósito de seguridad en ETH.

Con el objetivo de mejorar la descentralización incorporando una gama más amplia de operadores, el CSM introduce varias implementaciones para hacer que el staking individual sea más atractivo y accesible.
{% endhint %}

{% embed url="https://mirror.xyz/seedlatam.eth/PZxBfqEw-ABCDd2jUi2nTha0-w4-_bbCuEOsohhf06E" %}

{% embed url="https://operatorportal.lido.fi/modules/community-staking-module" %}

### **Pre-requisitos** <a href="#heading-pre-requisitos" id="heading-pre-requisitos"></a>

Tener instalado _**Dappnode**_ con los clientes sincronizados y _**web3signer,**_ en sus versiones para la red de testnet _**Holesky.**_

Si es la primera vez que escuchas de _**Dappnode**_ te recomiendo leer:

{% embed url="https://mirror.xyz/seedlatam.eth/VpuKM5vy2uWpK-H-MVGcbZaCIlRVoC3iTsASDDXIhTY" %}

Y tambien ver la instalación via script de _**Dappnode**_:

{% embed url="https://docs.dappnode.io/docs/user/install/script/" %}

Además vamos a necesitar tener una wallet fondeada con más 2 ETH en Holesky que podemos obtener mediante un faucet, por ejemplo:

{% embed url="https://holesky-faucet.pk910.de" %}

### **Parte 1: Interacción con Ethereum Staking Deposit CLI** <a href="#heading-parte-1-interaccion-con-ethereum-staking-deposit-cli" id="heading-parte-1-interaccion-con-ethereum-staking-deposit-cli"></a>

Como se ha visto en [tutoriales anteriores](https://mirror.xyz/seedlatam.eth/NE-iXSkA4_PCZpBWW_r4EhmTxXjilCytX3IxQEKi4KQ), se debe utilizar el **Ethereum Staking Deposit CLI**. Esta herramienta de línea de comandos que proporciona la **Ethereum Foundation** nos ayuda a generar las claves y los datos de depósito necesarios para convertirnos en validadores.

#### **Principales Características del Ethereum Staking Deposit CLI:** <a href="#heading-principales-caracteristicas-del-ethereum-staking-deposit-cli" id="heading-principales-caracteristicas-del-ethereum-staking-deposit-cli"></a>

1. **Generación de Claves**: La herramienta genera las **claves de validador** y las **claves de retiro** necesarias para el staking. La clave del validador se usa para firmar las validaciones y bloques, mientras que la clave de retiro se utiliza para retirar el ETH en el futuro.
2. **Generación de Datos de Depósito**: Crea el archivo de **datos de depósito** que es necesario para depositar ETH en el contrato de staking de Ethereum.
3. **Seguridad**: Te permite generar las claves de forma segura, manteniéndolas fuera de línea durante el proceso de generación. Esto reduce el riesgo de posibles ataques o fugas de claves que podrían comprometer la seguridad de tu validador. _**\***_

_**\***_ Es importante tener en cuenta que, en este tutorial, el procedimiento se realiza _ONLINE_ para la generación de claves. Aunque es posible realizarlo _OFFLINE_, como se menciona, ese no es el enfoque de esta guía.

Si te interesa la creación de claves OFFLINE te recomendamos ver [este video](https://youtu.be/ou_42QD_F4E?si=qsHOH-VigkkiTEZ0)

Para poder encontrar esta herramienta primero debemos dirigirnos hacia el _**GitHub**_ oficial de Ethereum: [https://github.com/ethereum](https://github.com/ethereum) luego, ir a _**repositories**_ ([https://github.com/orgs/ethereum/repositories](https://github.com/orgs/ethereum/repositories)) y buscar “_**staking-deposit-cli**_”, esta es una forma segura de poder acceder al repositorio de la herramienta.

<figure><img src="https://images.mirror-media.xyz/publication-images/x5mW8oSVOAnKKzov_90yz.png" alt="https://github.com/orgs/ethereum/repositories"><figcaption><p>https://github.com/orgs/ethereum/repositories</p></figcaption></figure>

Una vez dentro del repositorio, debes dirigirte a _**Releases**_

<figure><img src="https://images.mirror-media.xyz/publication-images/bn_aEeiFtlHDHkA4K7R_J.png" alt=""><figcaption></figcaption></figure>

Se descarga el archivo correspondiente al sistema operativo que estemos utilizando, en nuestro caso al estar usando _**Ubuntu**_ debemos descargar “_**staking\_deposit-cli-\[version]-amd64.tar.gz**_”

<figure><img src="https://images.mirror-media.xyz/publication-images/xAGuRTa93-7bwEB2t2D5Q.png" alt=""><figcaption></figcaption></figure>

Una vez descargado, descomprimí el archivo utilizando el comando:

```
tar -xzf [nombre_archivo].tar.gz
```

<figure><img src="https://images.mirror-media.xyz/publication-images/VhfQTpC9aOLCxG0xk2-Kv.png" alt=""><figcaption></figcaption></figure>

Para poder empezar a interactuar con el CLI hay que utilizar los comandos que nos brinda [la documentación](https://github.com/ethereum/staking-deposit-cli?tab=readme-ov-file#tutorial-for-users)

<figure><img src="https://images.mirror-media.xyz/publication-images/0E1dBCYymF4YtoeD8AYLz.png" alt=""><figcaption></figcaption></figure>

Utilizamos el comando `new-mnemonic` reemplazando la dirección de retiro por la que nos indica _**Lido**_:

<figure><img src="https://images.mirror-media.xyz/publication-images/R3L1WKYDUHCF6fAu_gZ0G.png" alt=""><figcaption></figcaption></figure>

Creamos las _keys_ con el comando `./deposit new_mnemonic --execution_address WITHDRAWAL_ADDRESS_HOLESKY_LIDO` :

<figure><img src="https://images.mirror-media.xyz/publication-images/vK59E_kyLMDsrC3GXmXoT.png" alt=""><figcaption></figcaption></figure>

Seleccionamos el idioma. En este caso, la opción de inglés con el _3:_

<figure><img src="https://images.mirror-media.xyz/publication-images/Jb4Je-rmxaMZWcM3fiW9L.png" alt=""><figcaption></figcaption></figure>

Reingresamos la dirección para confirmar:

<figure><img src="https://images.mirror-media.xyz/publication-images/vB1U2RFtr0T1HSvGPAKcK.png" alt=""><figcaption></figcaption></figure>

Seleccionamos el idioma para las palabras mnemonicas, _4_ para la opción de inglés:

<figure><img src="https://images.mirror-media.xyz/publication-images/l1nGUYKfAutWCXB2wXxE3.png" alt=""><figcaption></figcaption></figure>

Seleccionamos cuantos validadores vamos a querer correr. En nuestro caso, _1_ :

<figure><img src="https://images.mirror-media.xyz/publication-images/VmoZxEHfEAxp6JFvxab_g.png" alt=""><figcaption></figcaption></figure>

Se selecciona la red que se va a utilizar: _mainnet_ o _testnet_.

En este caso, testnet —> &#x48;_&#x6F;lesky_:

<figure><img src="https://images.mirror-media.xyz/publication-images/0ucGJ4HtZVAXRzvolP9_k.png" alt=""><figcaption></figcaption></figure>

Se ingresa una contraseña para nuestro _keystore (vamos a volver a usarla luego asi que es mejor anotarla en un lugar seguro)_:

<figure><img src="https://images.mirror-media.xyz/publication-images/6JMFGx-wxrlJ5g2GPxmfO.png" alt=""><figcaption></figcaption></figure>

Repite la contraseña:

<figure><img src="https://images.mirror-media.xyz/publication-images/97GLL7EzzfXRORkHTNXIq.png" alt=""><figcaption></figcaption></figure>

Guardamos las palabras:

<figure><img src="https://images.mirror-media.xyz/publication-images/bcuuHYUwFbSmKRQNyF-jq.png" alt=""><figcaption></figcaption></figure>

Escribe las palabras separadas por espacio para confirmar:

<figure><img src="https://images.mirror-media.xyz/publication-images/L7cNo3vP9PKGmwdEYI11I.png" alt=""><figcaption></figcaption></figure>

¡Listo! Ya creaste el keystore y tenemos el path en donde se encuentran.

<figure><img src="https://images.mirror-media.xyz/publication-images/EkPSn0M_D3O-eksb0wfmw.png" alt=""><figcaption></figcaption></figure>

Desde ahí, nos dirigimos a la carpeta indicada para ver los archivos:

<figure><img src="https://images.mirror-media.xyz/publication-images/xJ7N6gBf8IIVnLf9Vr8Sl.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://images.mirror-media.xyz/publication-images/ge0lOIjL8Eg0ykj0X3J5_.png" alt=""><figcaption></figcaption></figure>

### **Parte 2: Interacción con Dappnode** <a href="#heading-parte-2-interaccion-con-dappnode" id="heading-parte-2-interaccion-con-dappnode"></a>

#### **Pre-requisitos: MEV Boost Holesky** <a href="#heading-pre-requisitos-mev-boost-holesky" id="heading-pre-requisitos-mev-boost-holesky"></a>

Para evitar encontrarse con la siguiente advertencia a la hora de cargar las claves, debemos instalar _**MEV Boost Holesky**_, ya que vamos a estar montando un **CSM**

<figure><img src="https://images.mirror-media.xyz/publication-images/StK7IqThWChxEcOdzLeFH.png" alt=""><figcaption></figcaption></figure>

1- Vamos a la pestaña de: _Stakers_ > _Holesky_

<figure><img src="https://images.mirror-media.xyz/publication-images/iZRRbZQwz40r62qgpV8KK.png" alt=""><figcaption></figcaption></figure>

2- Seleccionamos : “_Mev Boost Holesky_”

<figure><img src="https://images.mirror-media.xyz/publication-images/aMlDHWUPEvsKP2yVZPB2Z.png" alt="" height="858" width="645"><figcaption></figcaption></figure>

3- Esperamos a que se descargue

<figure><img src="https://images.mirror-media.xyz/publication-images/yIitIIaWkqNMYAO-phBjl.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://images.mirror-media.xyz/publication-images/-gBpppQzg3JNzxdPpdviH.png" alt="" height="867" width="549"><figcaption></figcaption></figure>

¡Espectacular! Ahora ya tenemos instalado Mev boost!

#### **Cargar el keystore** <a href="#heading-cargar-el-keystore" id="heading-cargar-el-keystore"></a>

En _**web3signer**_ hacemos click en “_Upload Keystores_”

<figure><img src="https://images.mirror-media.xyz/publication-images/BLT6exsiqKLWxHXx_0Qs8.png" alt=""><figcaption></figcaption></figure>

Le damos click en “_IMPORT_”:

<figure><img src="https://images.mirror-media.xyz/publication-images/X10e11bs5m5mI8xDhTXGw.png" alt=""><figcaption></figcaption></figure>

**Importante**: debes subir el _**keystore**_ y NO EL DEPOSIT\_DATA

<figure><img src="https://images.mirror-media.xyz/publication-images/cEfULJCteJQMyZjwmLth1.png" alt=""><figcaption></figcaption></figure>

Si queremos podemos importar _**slashing protection data**_ :

<figure><img src="https://images.mirror-media.xyz/publication-images/7_DDmci197NEnxbQ34v5n.png" alt=""><figcaption></figcaption></figure>

Arrastramos el keystore hacia el apartado gris de la pagina:

<figure><img src="https://images.mirror-media.xyz/publication-images/_PBzbqu7d5rEnye6LV6pl.png" alt=""><figcaption></figcaption></figure>

Ingresamos la contraseña que utilizste al momento de crear el keystore usando el **Ethereum Staking Deposit CLI:**

<figure><img src="https://images.mirror-media.xyz/publication-images/yc5mt3GvQWrc9jgpzoD6Q.png" alt=""><figcaption></figcaption></figure>

Seleccionamos **Lido**:

<figure><img src="https://images.mirror-media.xyz/publication-images/k-ddCEZ4_nKHJahkYVnoR.png" alt=""><figcaption></figcaption></figure>

Observamos que el _fee recipient_ se setea automaticamente

<figure><img src="https://images.mirror-media.xyz/publication-images/MLKoN7flZ3eh0FZXh_eU5.png" alt=""><figcaption></figcaption></figure>

¡Perfecto! Keys importadas. Es importante seguir el orden de instalación de _**Mev Boost**_ y luego estos pasos para no tener problemas al abrir el link para realizar la carga de las keys

<figure><img src="https://images.mirror-media.xyz/publication-images/0gaUz1irsjyYVQ-u1RQu_.png" alt=""><figcaption></figcaption></figure>

Puedes ver cómo se encuentra cargada la key

<figure><img src="https://images.mirror-media.xyz/publication-images/WVhD3RbAeEOntTazvwk2w.png" alt=""><figcaption></figcaption></figure>

### **Parte 3: Registro del node operator** <a href="#heading-parte-3-registro-del-node-operator" id="heading-parte-3-registro-del-node-operator"></a>

#### **Prerequisito: wallet con > 2 ETH** <a href="#heading-prerequisito-wallet-con--2-eth" id="heading-prerequisito-wallet-con--2-eth"></a>

> 2 ETH para el montado del CSM y realizar una transacción.

Nos dirigimos hacia la [web de testnet de **Lido**](https://csm.testnet.fi/)

<figure><img src="https://images.mirror-media.xyz/publication-images/4Ac2bgv0_qUCFa35Bp9wL.png" alt=""><figcaption></figcaption></figure>

Conectamos nuestra wallet, en este caso _**Metamask**_:

<figure><img src="https://images.mirror-media.xyz/publication-images/uDNV5gy-OcnJx0ge47VfW.png" alt=""><figcaption></figcaption></figure>

Hacemos click en el botón _Create Node Operator_:

<figure><img src="https://images.mirror-media.xyz/publication-images/xH2fwt2eslFXpzMPi0e0p.png" alt=""><figcaption></figcaption></figure>

Pegamos el archivo JSON del _deposit data:_

<figure><img src="https://images.mirror-media.xyz/publication-images/c2vQjqO4scL_iyzKn6Xpz.png" alt=""><figcaption></figcaption></figure>

Se abre la ventana de _**Metamask**_ para realizar el deposito de los 2 ETH, le damos click a _Confirmar_ :

<figure><img src="https://images.mirror-media.xyz/publication-images/JOVNNt1ZQs0YD9RgrAtlW.png" alt=""><figcaption></figcaption></figure>

Luego de esperar, podes ir a la parte de _view keys,_ copiar el link de la key y buscarlo en la [web de beaconcha.in](https://holesky.beaconcha.in/) o hacer click directo en el boton azul:

> ¿Qué significa que el Status sea Depositable? Esperando a que su depósito sea finalizado y validado en la Beacon Chain para poder ser activado y comenzar a participar en el proceso de validación y creación de bloques

<figure><img src="https://images.mirror-media.xyz/publication-images/_18piQ5TgNfP3vEs5IS3O.png" alt=""><figcaption></figcaption></figure>

[holesky.beaconcha.inholesky.beaconcha.in](https://holesky.beaconcha.in/validator/0x9740a146f59fcc640916ea6ba508515ac09ba0de2e4855dbc0d8fa5fd911f04c52476f2502d7248fa3bb93c6fbb6e04f)

<figure><img src="https://images.mirror-media.xyz/publication-images/6KoPZDjTJ68OV1mBxEbtu.png" alt=""><figcaption></figcaption></figure>

Despues de unas horas el estado del validador va a pasar de:

_**deposited → pending → active**_

**Esto puede tardar 1 dia aproximadamente, asi que debemos tener paciencia.**

¡Felicidades! Si seguiste todos los pasos de esta guía, ahora deberías tener tu validador CSM funcionando en Dappnode.

Pasaste por la configuración del **Ethereum Staking Deposit CLI** para generar tus claves de validador y depósito, y has interactuado con **Dappnode** para cargar tus claves y completar la configuración de tu nodo validador en la red Holesky.

Este proceso no solo fortalece la red de Ethereum y mejora la descentralización, sino que también te permite participar en el staking de manera activa.

**Mantené tu Dappnode actualizado y revisa periódicamente el estado de tu validador para garantizar un rendimiento óptimo.**

Si deseas seguir aprendiendo más o profundizar en aspectos como la creación de claves offline o la optimización de validadores, asegúrate de consultar los recursos adicionales que hemos mencionado y seguir experimentando con nuevas configuraciones.

Cualquier duda que tengas y quieras consultar o conversar con alguien, estas invitado a unirte a nuestro [Club de Nodos en telegram](https://t.me/SEED_Nodes) o escaneá el QR:

<figure><img src="https://images.mirror-media.xyz/publication-images/dGovLJGXxRxutrxkVAB1a.png" alt=""><figcaption></figcaption></figure>

¡Buena suerte con tu nodo validador y bienvenido al ecosistema de validadores de Ethereum!

### **Recursos** <a href="#heading-recursos" id="heading-recursos"></a>

Guía oficial:

[**Lido CSM | ETH Home Staking Collection**](https://dvt-homestaker.stakesaurus.com/bonded-validators-setup/lido-csm#key-settings-to-note)

[Lido CSM | ETH Home Staking Collectiondvt-homestaker.stakesaurus.com](https://dvt-homestaker.stakesaurus.com/bonded-validators-setup/lido-csm#key-settings-to-note)\
