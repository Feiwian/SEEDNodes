---
icon: globe
cover: ../.gitbook/assets/1500x500-6.jpeg
coverY: 0
---

# Celestia

{% hint style="info" %}
[Celestia](https://celestia.org/build/) es una red de disponibilidad de datos que optimiza la escalabilidad de Ethereum separando la disponibilidad de datos del consenso y la ejecución. Introduce técnicas innovadoras como el muestreo de disponibilidad de datos y los árboles de Merkle con espacios de nombres, que permiten una verificación eficiente de los datos. Aprender a montar un nodo en Celestia desde cero te proporcionará una comprensión profunda de estas técnicas y te permitirá contribuir activamente a la seguridad y la escalabilidad de la red Ethereum.
{% endhint %}

#### Preparación e instalación del nodo. <a href="#heading-preparacion-e-instalacion-del-nodo" id="heading-preparacion-e-instalacion-del-nodo"></a>

### Introducción <a href="#heading-introduccion" id="heading-introduccion"></a>

> #### **¿Qué es Celestia?** <a href="#heading-que-es-celestia" id="heading-que-es-celestia"></a>
>
> Celestia es una [data availability network](https://blog.celestia.org/celestia-a-scalable-general-purpose-data-availability-layer-for-decentralized-apps-and-trust-minimized-sidechains) que se adapta de manera segura al crecimiento de usuarios, posibilitando que cualquier individuo lance su propia cadena de bloques.
>
> En la vanguardia de las arquitecturas blockchain escalables, Celestia introduce el concepto de "[modular blockchains](https://celestia.org/learn)". En este enfoque, la escalabilidad se logra [desvinculando la ejecución del consenso](https://arxiv.org/abs/1905.09274) y presentando una novedosa técnica llamada "[data availability sampling](https://arxiv.org/abs/1809.09044)".
>
> En el primer aspecto, Celestia asume la responsabilidad exclusiva de ordenar las transacciones y garantizar la disponibilidad de los datos, equiparable a [reducir el consenso a una transmisión atómica](https://en.wikipedia.org/wiki/Atomic\_broadcast#Equivalent\_to\_Consensus). Por otro lado, el "[data availability sampling](https://coinmarketcap.com/alexandria/article/what-is-data-availability)" ofrece una solución eficiente al desafío de la disponibilidad de datos al exigir que solo los nodos ligeros, con recursos limitados, realicen muestreos en fragmentos aleatorios de cada bloque para verificar dicha disponibilidad.
>
> La participación de más nodos ligeros en este proceso de muestreo aumenta la capacidad segura de la red para manejar datos, posibilitando un incremento en el tamaño del bloque sin un correspondiente aumento en el costo de la verificación de la cadena.

***

#### **Tipos de Nodo:** <a href="#heading-tipos-de-nodo" id="heading-tipos-de-nodo"></a>

Hay muchas formas en las que podes participar en las [redes](https://docs.celestia.org/nodes/participate) de Celestia. Los operadores de nodos de Celestia pueden ejecutar varias opciones en la red.

**Consensus:**

* [Nodo validador](https://docs.celestia.org/nodes/consensus-node#optional-setting-up-a-validator) : este tipo de nodo participa en el consenso produciendo y votando bloques.
* [Nodo de consenso completo](https://docs.celestia.org/nodes/consensus-node) : un nodo completo de la aplicación celestia para sincronizar el historial de blockchain.

**Data Availability:**

* [Bridge node](https://docs.celestia.org/nodes/bridge-node): este nodo sirve de puente entre la red de disponibilidad de datos y la red de consenso.
* [Full storage node](https://docs.celestia.org/nodes/full-storage-node): este nodo almacena todos los datos pero no se conecta a Consensus.
* [Light Node:](https://docs.celestia.org/nodes/light-node) los clientes ligeros realizan muestreos de disponibilidad de datos en la red de disponibilidad de datos.

<figure><img src="https://images.mirror-media.xyz/publication-images/EJ47O-Bz9FC-I6uRLhCu3.png" alt=""><figcaption></figcaption></figure>

## Parte 1: Preparación <a href="#heading-parte-1-preparacion" id="heading-parte-1-preparacion"></a>

_\*NOTA: La versión de Ubuntu que se utiliza es la Ubuntu 22.04.4 LTS_

**Cómo instalar los componentes esenciales:**

```
sudo apt update && sudo apt -y upgrade 
```

Este comando se encarga de actualizar la lista de paquetes disponibles en los repositorios, y si eso se realiza correctamente, procede a actualizar todos los paquetes instalados en el sistema.

<figure><img src="https://images.mirror-media.xyz/publication-images/RqH6v5Ycrfp9dt0p-7K-_.png" alt=""><figcaption></figcaption></figure>

Instalamos paquetes esenciales:

```
sudo apt install curl tar wget aria2 clang pkg-config libssl-dev jq build-essential \
git make ncdu -y
```

<figure><img src="https://images.mirror-media.xyz/publication-images/Cdq8fLSde5lO__bVJXpWZ.png" alt=""><figcaption></figcaption></figure>

#### **Instalando Golang:** <a href="#heading-instalando-golang" id="heading-instalando-golang"></a>

Celestia-Node está escrito en Golang, por lo que primero debemos instalar Golang para construir y ejecutar nuestro nodo.

Configuramos la versión:

```
ver="1.21.1"
```

Descargamos e instalamos Golang:

```
cd $HOME
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
```

Agregas /usr/local/go/bin en el directorio a $PATH si aún no lo hiciste:

```
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

Verificamos que se instalo correctamente la versión de Go:

```
go version
```

<figure><img src="https://images.mirror-media.xyz/publication-images/bjKTGu4ikkObZaretFke_.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://images.mirror-media.xyz/publication-images/Y669e2lfNQqBS1g0SVtxC.png" alt=""><figcaption></figcaption></figure>

***

## Parte 2: Instalación <a href="#heading-parte-2-instalacion" id="heading-parte-2-instalacion"></a>

Eliminamos cualquier copia existente de celestia-node, clonamos el repositorio y cambie al directorio:

```
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
```

<figure><img src="https://images.mirror-media.xyz/publication-images/IJZc3N7QT8_tBql7rBoic.png" alt=""><figcaption></figcaption></figure>

Chequeamos la versión deseada, según la red que vamos a usar:

```
git checkout tags/v0.12.4
```

_Construimos el Celestia binary. Existen 2 maneras: Standar y Experimental._

*   _Standar_

    ```
    make build
    ```
*   _Experimental_

    Si sos un operador de nodo que se siente cómodo con las funciones experimentales y buscas un rendimiento óptimo con un uso mínimo de RAM, se recomienda esta opción.

    ```
    make build-jemalloc
    ```

Instalamos el binary:

```
make install
```

Construimos la utilidad de cel-key:

```
make cel-key
```

Verificamos que el binary esté funcionando y verificamos también la versión:

```
celestia version
```

El comando mostrará la versión de celestia-node, el hash de confirmación, la fecha de compilación, la versión del sistema y la versión de Golang.

\*NOTA: Ahora que hemos instalado el nodo, es hora de elegir el tipo y ejecutarlo.

#### Iniciando un Light Node: <a href="#heading-iniciando-un-light-node" id="heading-iniciando-un-light-node"></a>

Lo primero es crear una instancia (o inicializar) el nodo, que significa configurar un almacén en nuestra máquina, que es donde se almacenarán los datos y nuestras claves.

* **Mainnet:** `celestia light init`
* **Arabica:** `celestia light init --p2p.network mocha`
* **Mocha:** `celestia light init --p2p.network arabica`

Esto mostrará la ubicación de su almacén de nodos y su configuración. También mostrará la confirmación de que el almacén de nodos se ha inicializado.

\*NOTA: En este paso se crea una frase nemotécnica (resguardarla).

**Conectando a un public core endpoint:**

Conexión al Punto Final gRPC del Nodo Validador, por defecto puerto 9090, es esencial para acceder a la capacidad de obtener y enviar información relacionada con el estado. Esto incluye la facultad de enviar transacciones PayForBlob o consultar el saldo de la cuenta del nodo.

```
celestia light start --core.ip <URI>
```

\*NOTA: No es necesario declarar una red para Mainnet Beta.

_Ejemplo Mainnet:_

```
celestia light start --core.ip consensus.lunaroasis.net
```

<figure><img src="https://images.mirror-media.xyz/publication-images/RC8n7egMpzNLIwiEKHGlQ.png" alt=""><figcaption></figcaption></figure>

_Usando un RPC propio, o uno de la_ [_lista en la página de prueba de Mocha_](https://docs.celestia.org/nodes/mocha-testnet#rpc-endpoints) _o_ [_la lista en la página de devnet de Arábica_](https://docs.celestia.org/nodes/arabica-devnet#rpc-endpoints) _, iniciamos el nodo:_

_Ejemplo en Mocha:_

```
celestia light start --core.ip rpc-mocha.pops.one --p2p.network mocha
```

<figure><img src="https://images.mirror-media.xyz/publication-images/tjG_oyQbchUDAn73jlWks.png" alt=""><figcaption></figcaption></figure>

_Ejemplo en Arabica:_

```
celestia light start --core.ip validator-1.celestia-arabica-11.com \
    --p2p.network arabica
```

<figure><img src="https://images.mirror-media.xyz/publication-images/WYzMFiV94vsoNjp6g5Kqn.png" alt=""><figcaption></figcaption></figure>

#### **Keys y wallets:** <a href="#heading-keys-y-wallets" id="heading-keys-y-wallets"></a>

Podemos crear una clave para el nodo ejecutando el siguiente comando con la utilidad cel-key en el directorio celestia-node:

```
./cel-key add <key-name> --keyring-backend test \
    --node.type light --p2p.network <network>
```

Podemos iniciar nuestro light node con la clave creada anteriormente ejecutando el siguiente comando:

```
celestia light start --keyring.accname my_celes_key \
    --core.ip consensus.lunaroasis.net
```

Una vez que iniciamos el light node, se generará una clave para nuestra wallet. Deberemos fondearla esa dirección con tokens de testnet para pagar las transacciones de PayForBlob.

Podemos encontrar la dirección utilizando la CLI de RPC o ejecutando el siguiente comando en el directorio celestia-node:

```
./cel-key list --node.type light --keyring-backend test \
    --p2p.network <network>
```

#### **Testnet tokens** <a href="#heading-testnet-tokens" id="heading-testnet-tokens"></a>

Existen dos redes para obtener tokens de testnet:

* [Arábica Devnet](https://docs.celestia.org/nodes/arabica-devnet#arabica-devnet-faucet)
* [Mocha Testnet](https://docs.celestia.org/nodes/mocha-testnet#mocha-testnet-faucet)

También se accede a ellos ingresando al [Discord](https://discord.gg/ajuUUrRU) de Celestia y solicitándolos de la siguiente manera:

_$request_&#x20;

_Donde es la dirección celestia1\*\*\*\*\*\* generada cuando creaste la billetera._

<figure><img src="https://images.mirror-media.xyz/publication-images/tExAUUwhgnqUMVF6nIv9_.png" alt=""><figcaption></figcaption></figure>

#### **Full storage node:** <a href="#heading-full-storage-node" id="heading-full-storage-node"></a>

Iniciamos con el siguiente comando:

* **Mainnet:** `celestia full init`
* **Arabica:** `celestia full init --p2p.network arabica`
* **Mocha:** `celestia full init --p2p.network mocha`

**Conectando a un public core endpoint:**

Conexión al Punto Final gRPC del Nodo Validador, por defecto puerto 9090, es esencial para acceder a la capacidad de obtener y enviar información relacionada con el estado. Esto incluye la facultad de enviar transacciones PayForBlob o consultar el saldo de la cuenta del nodo.

```
celestia full start --core.ip <URI>
```

\*NOTA: No es necesario declarar una red para Mainnet Beta.

_Ejemplo en Mocha:_

```
celestia full start --core.ip rpc-mocha.pops.one --p2p.network mocha
```

_Ejemplo en Arabica:_

```
celestia full start --core.ip validator-1.celestia-arabica-11.com \
    --p2p.network arabica
```

<figure><img src="https://images.mirror-media.xyz/publication-images/ZEvWbLFIHrihqbJavXSWN.png" alt=""><figcaption></figcaption></figure>

#### **Bridge Node:** <a href="#heading-bridge-node" id="heading-bridge-node"></a>

Iniciamos con el siguiente comando:

* **Mainnet:** `celestia bridge init`
* **Arabica:** `celestia bridge init --p2p.network arabica`
* **Mocha:** `celestia bridge init --p2p.network mocha`

**Conectando a un public core endpoint:**

Conexión al Punto Final gRPC del Nodo Validador, por defecto puerto 9090, es esencial para acceder a la capacidad de obtener y enviar información relacionada con el estado. Esto incluye la facultad de enviar transacciones PayForBlob o consultar el saldo de la cuenta del nodo.

```
celestia bridge start --core.ip <URI>
```

\*NOTA: No es necesario declarar una red en Mainnet Beta.

_Ejemplo en Mocha:_

```
celestia bridge start --core.ip rpc-mocha.pops.one:26657 --p2p.network mocha
```

_Ejemplo en Arabica:_

```
celestia bridge start --core.ip validator-1.celestia-arabica-11.com \
  --p2p.network arabica
```

***

Todo el material que se presenta aquí es de acceso público. Para obtener información detallada y seguir los pasos en inglés, se puede visitar la [web oficial](https://docs.celestia.org/nodes/overview).

\
\
