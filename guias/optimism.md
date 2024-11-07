---
icon: circle
cover: ../.gitbook/assets/optimism-img-1_11zon.jpg
coverY: 0
---

# Optimism

## Introducción <a href="#heading-introduccion" id="heading-introduccion"></a>

{% embed url="https://mirror.xyz/seedlatam.eth/8jW2E12CajwBNWWLgFyvjg6X3YxZyIGkC4SYT7jdV8Q" %}

{% hint style="info" %}
[Optimism](https://www.optimism.io/) es un rollup optimista de Ethereum desarrollado con el propósito de mejorar la escalabilidad y usabilidad. La reciente implementación de Bedrock marca la primera versión oficial del OP Stack, un conjunto de componentes que potencia blockchains como OP Mainnet. A diferencia de Ethereum, que opera con un consenso de prueba de participación, Bedrock utiliza la derivación de bloques. En esta estructura, el nodo rollup es esencial.
{% endhint %}

Con la introducción de Bedrock, el software del nodo ha experimentado mejoras. Ahora, múltiples transacciones se pueden ejecutar en un solo "bloque" de rollup, a diferencia del modelo anterior. Este cambio permite que el costo de las actualizaciones del Merkle tree se distribuya entre varias transacciones, reduciendo el crecimiento del estado en aproximadamente 15 GB por año.

El rendimiento del nodo se ha mejorado adicionalmente eliminando características obsoletas del protocolo anterior y actualizando el software del nodo para una mejor consulta de datos desde L1.

Antes de seguir, es importante comprender los agentes principales en la arquitectura del rollup, en especial el Nodo OP:

<figure><img src="https://images.mirror-media.xyz/publication-images/o624nogluGGk5QVQzGy4Y.jpg" alt=""><figcaption></figcaption></figure>

Cuando un usuario envía una transacción, el primer receptor es un nodo verificador que tiene la capacidad de comprobar la validez de la transacción. Una vez comprobada, la transacción es propagada por la red p2p hasta que es tomada por el secuenciador.

La transacción es recopilada tal y como si surgiera de una mempool por el **secuenciador** para que éste las ordene, ejecute e incluya en el bloque siendo construido.

Minutos después, el “**batcher”** toma el rol de procesar los bloques utilizando métodos de compresión a modo de batches y publicarlas en la L1, para luego el “**proposer”** leer las transacciones publicadas y estados resultantes por el “**batcher”** y emitir los “**state root**” que luego también se publican en la L1 para el conjunto de bloques y batches involucrados. El “**state root”** es una representación del estado actual de la cadena L2 y dictamina los puntos de referencia para confirmar validez de las transacciones y habilitar más tarde comunicación entre L2 y L1, tales como los retiros. Por último, el verificador como observante de la red, lee los "**state roots**" que el “**proposer”** ha publicado y es capaz de detectar inconsistencias. Idealmente, puede actuar como un "**challenger**" para cuestionar lo que el “**proposer**” ha publicado en caso de detectar alguna irregularidad, utilizando las pruebas de fraude, aunque esta última pieza  todavía no está implementada en la versión de OP Mainnet.

Este proceso garantiza que las transacciones se procesen de manera eficiente y segura en la cadena L2, optimizando el rendimiento y la escalabilidad mientras mantiene la seguridad y la integridad de la red.

## Nodo OP <a href="#heading-nodo-op" id="heading-nodo-op"></a>

#### El Papel del Nodo Rollup <a href="#heading-el-papel-del-nodo-rollup" id="heading-el-papel-del-nodo-rollup"></a>

Como comentamos anteriormente, el nodo rollup, también conocido como verificador, tiene un papel fundamental en verificar la validez de las transacciones. Ayuda a la propagación de las transacciones para ser incluídas en el próximo bloque, una función comparable a un nodo completo de Ethereum  (Si no sabes cómo este funciona, chequea el artículo que escribimos sobre los nodos de Ethereum).. Es este nodo el que se asegura de que los **state roots** en L2 concuerden con lo reflejado en L1.

Dentro del nodo, el **controlador rollup** es responsable de:

* Seguir el bloque principal de L1.
* Monitorear la sincronización de la cadena L2.
* Progresar en la derivación con nuevas entradas.

Adicionalmente, en un futuro el nodo OP incluirá el módulo de participación como challenge para activar pruebas de fraude, lo que proporciona la última pero más importante pieza para la seguridad del rollup. .

#### Funcionalidades del Nodo Rollup <a href="#heading-funcionalidades-del-nodo-rollup" id="heading-funcionalidades-del-nodo-rollup"></a>

El nodo rollup posee su método RPC propio, llamado **optimism\_outputAtBlock**, que devuelve un hash de 32 bytes correspondiente a la raíz de salida L2.

También, cuenta con un servicio de red peer-to-peer (P2P) opcional que optimiza la latencia entre secuenciadores y el resto de la red, cuando es posible, sin depender de un solo punto central.

Este nodo prioriza siempre a L1 y ajusta su organización para alinearse con la cadena canónica. Los datos L2 obtenidos a través de la interfaz P2P se consideran una extensión especulativa o cadena "insegura".

Los nodos rollup de Optimism emplean un proceso de descubrimiento **Discv5** con registros de nodo ENR para encontrar y conectarse con otros nodos. Esto facilita el intercambio de información y la actualización sobre transacciones y bloques recientes en la red.

#### Cliente de Ejecución <a href="#heading-cliente-de-ejecucion" id="heading-cliente-de-ejecucion"></a>

Un cliente de ejecución es esencialmente el sistema que determina el estado de la cadena L2 canónica. Gestiona el procesamiento de transacciones entrantes, la comunicación peer-to-peer y la administración del estado del sistema para responder a consultas.

El mismo está separado del nodo rollup.

Bedrock permite que el OP Stack reutilice las especificaciones del cliente de ejecución de Ethereum. En este contexto, Bedrock ha implementado una modificación mínima de go-ethereum, con una diferencia menor a 2000 líneas de código.

Las principales diferencias son:

**Manejo de transacciones depositadas**: Introduce un nuevo tipo de transacción para representar transacciones depositadas en el rollup, conforme al estándar EIP-2718.

**Cobro de tarifas de transacción**: Las tarifas se dividen en dos categorías. Las tarifas del secuenciador se calculan con la tabla de gas de Ethereum y el algoritmo EIP-1559. Por otro lado, las tarifas de disponibilidad de datos cubren el costo de las transacciones de lotes en L1. En Bedrock, el cálculo de esta última tarifa se basa en la información de un contrato en el rollup llamado GasPriceOracle, que continuamente actualiza sus valores para proporcionar el nivel de gas más reciente en L1.

Ahora que entendes el rol del nodo en Optimism, es momento de que sepas cómo levantar tu propio nodo de Optimism. ¡Comencemos!

### **Requerimientos de Hardware:** <a href="#heading-requerimientos-de-hardware" id="heading-requerimientos-de-hardware"></a>

Recomendamos los siguientes requisitos para correr Bedrock:

*   **op-node**:

    \- Minimo 2CPUs, 4GB RAM.

    \- No es necesario contar con almacenamiento
*   **op-geth**:

    \- Minimo 4 CPUs, 8GB RAM.

    \- **Almacenamiento**: Al menos 600GB para mainnet, preferiblemente SSD. Para nodos de archivo, los requisitos son mayores

**Nota**: En operación, el uso de almacenamiento puede alcanzar aproximadamente 800GB.

<figure><img src="https://images.mirror-media.xyz/publication-images/kOKBaALVb-Su6AsmP9U2d.png" alt=""><figcaption></figcaption></figure>

## **Comandos pre requisitos:** <a href="#heading-comandos-pre-requisitos" id="heading-comandos-pre-requisitos"></a>

<figure><img src="https://images.mirror-media.xyz/publication-images/7XzznG6LJfzxMD6L8KSOS.jpg" alt=""><figcaption></figcaption></figure>

1. Descarga de Ubuntu: [Ubuntu Desktop](https://ubuntu.com/download/desktop)
2. Instalación de Node.js: Sigue estos pasos para instalar Node.js desde cero según la [guía oficial de NodeSource](https://github.com/nodesource/distributions#installation-instructions).

## **Construyendo el software** <a href="#heading-construyendo-el-software" id="heading-construyendo-el-software"></a>

Crea un directorio llamado "nodo" para guardar los archivos y scripts relevantes que indicaremos a lo largo de la guía.

<figure><img src="https://images.mirror-media.xyz/publication-images/cavalxAjd1I1tS-BQLiqt.jpg" alt=""><figcaption></figcaption></figure>

#### **Construyendo el monorepo de Optimism:** <a href="#heading-construyendo-el-monorepo-de-optimism" id="heading-construyendo-el-monorepo-de-optimism"></a>

1. Navega al directorio "nodo".
2.  Clona el [monorepo de Optimism](https://github.com/ethereum-optimism/optimism):

    ```
    git clone https://github.com/ethereum-optimism/optimism.git
    ```
3.  Instala las módulos requeridos:

    ```
    cd optimism
    pnpm install
    ```
4.  Construye los distintos paquetes dentro del Monorepo de Optimism con:

    ```
    make op-node
    pnpm build
    ```

Este proceso toma tiempo, puedes proceder con la siguiente sección en paralelo.

Para `op-geth`:

1. En una nueva terminal, navega de nuevo al directorio "nodo".
2.  Clona [op-geth](https://github.com/ethereum-optimism/op-geth):

    ```
    git clone https://github.com/ethereum-optimism/op-geth.git
    ```
3.  Compila op-geth con:

    ```
    cd op-geth 
    make geth
    ```

### **Op Mainnet:** <a href="#heading-op-mainnet" id="heading-op-mainnet"></a>

#### **Redes Migradas vs. No Migradas:** <a href="#heading-redes-migradas-vs-no-migradas" id="heading-redes-migradas-vs-no-migradas"></a>

* Las redes Migradas (OP Mainnet y OP Goerli) operaban antes de la actualización de Bedrock.
* Las redes No-Migradas (OP Sepolia) se lanzaron tras la actualización.
* El manejo de la ejecución de transacciones varía entre pre y post Bedrock.
* Redes migradas: Iniciar nodos con un directorio de datos.
* Redes no-migradas: Inicialización directa desde el archivo de génesis.

#### **Sincronización de la Blockchain:** <a href="#heading-sincronizacion-de-la-blockchain" id="heading-sincronizacion-de-la-blockchain"></a>

1.  Descarga la red (aproximadamente 303GB), dependiendo de la conexión a internet será el tiempo que tarde, con:

    ```
    wget https://datadirs.optimism.io/mainnet-bedrock.tar.zst
    ```

    Enlace obtenido de la [documentación de Optimism](https://community.optimism.io/docs/developers/nodes/mainnet/).
2.  **Verifica la integridad de los datos descargados** para evitar errores en el nodo. Debes asegurarte que el checksum coincida. En el directorio de la descarga, ejecuta:

    ```
    sha256sum mainnet-bedrock.tar.zst
    ```

    Deberías obtener uno de los siguientes resultados dependiendo de la versión descargada:

    * `ec4baf47e309a14ffbd586dc85376833de640c0f2a8d7355cb8a9e64c38bfcd1 mainnet-bedrock.tar.zst`
    * `c17067b7bc39a6daa14f71d448c6fa0477834c3e68a25e96f26fe849c12a09bffe510e96f7eacdef19e93e3167d15250f807d252dd6f6f9053d0e4457c73d5fb mainnet-bedrock.tar.zst`

    Siguiendo estas instrucciones técnicas, podrás configurar y ejecutar un nodo de Optimism.

<figure><img src="https://images.mirror-media.xyz/publication-images/HkNMzQrXA-RxNLXCzA3xP.png" alt="Ejemplo 1"><figcaption><p>Ejemplo 1</p></figcaption></figure>

En caso de que la operación de verificación esté tomando más tiempo del esperado y desees verificar el estado del proceso, puedes abrir otra terminal y ejecutar el siguiente comando:

```powershell
ps -e  
```

Busca la entrada `sha256sum` en la columna `CMD` para encontrar el proceso correspondiente. Durante nuestras pruebas, el tiempo promedio fue de aproximadamente 30 minutos, aunque esto puede variar en función de la configuración de hardware utilizada

<figure><img src="https://images.mirror-media.xyz/publication-images/ya-2EfVh15xPUTvtmSPVj.png" alt=""><figcaption></figcaption></figure>

Durante nuestras pruebas, el tiempo promedio fue de aproximadamente 30 minutos, aunque esto puede variar en función de la configuración de hardware utilizada.

3\. **Preparación del directorio de datos**

Ahora, es necesario crear un nuevo directorio dentro de `op-geth` y llenarlo con los datos que hemos descargado. Este proceso también será intensivo en tiempo. Ejecuta los siguientes comandos dentro del directorio `op-geth`:

```powershell
mkdir datadir 
cd datadir 
tar -xvf /path/to/downloaded/data
```

Ejemplo:

<figure><img src="https://images.mirror-media.xyz/publication-images/1pnN7m-9nhjyOmM2mBOqL.png" alt="Ejemplo 2"><figcaption><p>Ejemplo 2</p></figcaption></figure>

Si la ruta completa al archivo descargado es `/downloads/mainnet-bedrock.tar.zst`, reemplazarías `/path/to/downloaded/data` con esta ruta.

**Entendiendo el proceso:**

El comando `tar -xvf /path/to/downloaded/data` se utiliza para descomprimir y extraer archivos de un archivo tar. Aquí está la explicación de cada bandera utilizada:

* `tar`: Es un programa utilizado en sistemas Unix y Unix-like para archivar y comprimir archivos y directorios.
* `x`: Extraer archivos del archivo tar.
* `v`: Modo verbose; muestra en detalle los archivos que se están extrayendo.
* `f`: Especifica que se debe proporcionar el nombre del archivo tar después de esta bandera.
*   `f`: En este caso, "<\<RUTA\_DE\_LOS\_DATOS>>" debería ser reemplazado por la ruta y el nombre del archivo tar que deseas extraer.

    Es importante que esta Ruta sea completa, no la ruta relativa. (ver el ejemplo 2)

    ### **Crea un secreto compartido entre** `op-geth` **y** `op-node` <a href="#heading-crea-un-secreto-compartido-entre-op-geth-y-op-node" id="heading-crea-un-secreto-compartido-entre-op-geth-y-op-node"></a>

    Dentro de `op-geth`, ejecuta los siguientes comandos para crear un secreto compartido:

    ```
    openssl rand -hex 32 > jwt.txt
    cp jwt.txt ../optimism/op-node
    ```

    ### **Scripts para iniciar los componentes** <a href="#heading-scripts-para-iniciar-los-componentes" id="heading-scripts-para-iniciar-los-componentes"></a>

    En la raíz de tu directorio de trabajo, crea una nueva carpeta llamada `scripts`.

    #### **Op-geth:** <a href="#heading-op-geth" id="heading-op-geth"></a>

    1. Navega a la carpeta `scripts`.
    2.  Crea un nuevo archivo con el comando:

        ```
        touch run-op-geth.sh
        ```
    3.  Convierte el archivo en ejecutable:

        ```
        chmod +x run-op-geth.sh
        ```
    4.  Edita `run-op-geth.sh` con tu editor de texto preferido y agrega el siguiente script, asegurándote de modificar la ruta al directorio `op-geth`donde corresponda:

        ```
        #! /usr/bin/bash

        SEQUENCER_URL=https://mainnet-sequencer.optimism.io/

        cd <<Path al directorio op-geth>>

        ./build/bin/geth \
          --datadir=./datadir \
          --http \
          --http.port=8545 \
          --http.addr=0.0.0.0 \
          --authrpc.addr=localhost \
          --authrpc.jwtsecret=./jwt.txt \
          --verbosity=3 \
          --rollup.sequencerhttp=$SEQUENCER_URL \
          --nodiscover \
          --syncmode=full \
          --maxpeers=0
        ```
    5.  Para iniciar `op-geth`, ejecuta:

        ```
        ./run-op-geth.sh
        ```

        Luego, deberías comenzar a ver la indexación de transacciones en tu terminal:

<figure><img src="https://images.mirror-media.xyz/publication-images/oDjNuJw_bb_opg_6Fsrnq.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://images.mirror-media.xyz/publication-images/3XJBLjODMoMQnuAVFIZYC.png" alt=""><figcaption></figcaption></figure>

Esto puede tardar un rato o solamente unos segundos, lo que recomendamos es que una vez que dejen de aparecer las líneas de información, sigas con el paso siguiente de `op-node` utilizando otra terminal. Podrías tener las 2 terminales a la vista para ver como van avanzando a la par.

#### **op-node:** <a href="#heading-op-node" id="heading-op-node"></a>

1. Dentro de la carpeta `scripts`, crea un nuevo archivo llamado `touch run-op-node.sh`
2.  Repite el proceso para hacerlo ejecutable con el siguiente comando:

    ```
    chmod +x run-op-node.sh
    ```
3.  Inserta el siguiente código en `run-op-node.sh` :

    ```
    #!/usr/bin/bash
    ```

    ```
    L1URL=<< L1 RPC URL >> L1KIND=basic NET=mainnet
    ```

    ```
    cd <<Path to op-node directory>>
    ```

    ```
    ./bin/op-node 
    ```

    ```
    --l1=$L1URL  
    ```

    ```
    --l1.rpckind=$L1KIND 
    ```

    ```
    --l2=ws://localhost:8551 
    ```

    ```
    --l2.jwt-secret=./jwt.txt 
    ```

    ```
    --network=$NET 
    ```

    ```
    --rpc.addr=0.0.0.0 
    ```

    ```
    --rpc.port=8547
    ```

    Cambia el `<< L1 RPC URL >>` a tu nodo local L1 o a un proveedor de URL de nodos de L1 (L1 Ethereum). Por ej: [Infura](https://mainnet.infura.io/v3/API\_KEY.)
4. Establece `L1KIND` de acuerdo al proveedor de red que estés utilizando (opciones: alchemy, quicknode, infura, parity, nethermind, debug\_geth, erigon, basic).
5.  Para arrancar `op-node`, ejecuta:

    ```
    ./run-op-node.sh
    ```

Después de iniciar ambos scripts, simplemente queda monitorear su progreso.

### Iniciación de la Sincronización <a href="#heading-iniciacion-de-la-sincronizacion" id="heading-iniciacion-de-la-sincronizacion"></a>

El datadir suministrado por Optimism no se actualiza de forma constante (refiriéndose al archivo de gran tamaño que hemos descargado), por lo que antes de empezar a utilizar el nodo es necesario sincronizarlo. Durante este proceso, observaremos mensajes en la consola de op-node, y podría parecer que no ocurre nada más.

```
INFO [06-26|13:31:20.389] Advancing bq origin         origin=17171d..1bc69b:8300332 originBehind=false
```

Esto es un comportamiento esperado - indica que op-node está buscando una posición en la cola de lotes. Tras unos minutos, la encuentra y entonces comienza el proceso de sincronización. Durante la sincronización, es normal ver mensajes como estos en la consola de op-node:

```
INFO [06-26|14:00:59.460] Sync progress                            reason="processed safe block derived from L1" l2_finalized=ef93e6..e0f367:4067805 l2_safe=7fe3f6..900127:4068014 l2_unsafe=7fe3f6..900127:4068014 l2_time=1,673,564,096 l1_derived=6079cd..be4231:8301091
INFO [06-26|14:00:59.460] Found next batch                         epoch=8e8a03..11a6de:8301087 batch_epoch=8301087 batch_timestamp=1,673,564,098
INFO [06-26|14:00:59.461] generated attributes in payload queue    txs=1  timestamp=1,673,564,098
INFO [06-26|14:00:59.463] inserted block                           hash=e80dc4..72a759 number=4,068,015 state_root=660ced..043025 timestamp=1,673,564,098 parent=7fe3f6..900127 prev_randao=78e43d..36f07a fee_recipient=0x4200000000000000000000000000000000000011 txs=1  update_safe=true
```

Y mensajes similares en la consola de `op-geth`:

```
INFO [06-26|14:02:12.974] Imported new potential chain segment     number=4,068,194 hash=a334a0..609a83 blocks=1         txs=1         mgas=0.000  elapsed=1.482ms     mgasps=0.000   age=5mo2w20h dirty=2.31MiB
INFO [06-26|14:02:12.976] Chain head was updated                   number=4,068,194 hash=a334a0..609a83 root=e80f5e..dd06f9 elapsed="188.373µs" age=5mo2w20h
INFO [06-26|14:02:12.982] Starting work on payload                 id=0x5542117d680dbd4e
```

#### **¿Cuánto tiempo tardará la sincronización?** <a href="#heading-cuanto-tiempo-tardara-la-sincronizacion" id="heading-cuanto-tiempo-tardara-la-sincronizacion"></a>

Para estimar la duración de la sincronización, primero debes calcular cuántos bloques sincroniza en un minuto. Puedes utilizar este script de \*\*[Foundry](https://book.getfoundry.sh/) \*\*para obtener un tiempo estimado de sincronización.

Luego, debes navegar a tu directorio de scripts y crear un archivo nuevo:

```
touch run-estimate.sh
```

Ejecutá el siguiente comando para hacerlo ejecutable:

```
chmod +x run-estimate.sh
```

Inserta el siguiente código dentro de `run-estimate.sh`:

```
#!/usr/bin/bash

export ETH_RPC_URL=http://localhost:8545
CHAIN_ID=`cast chain-id`
echo Chain ID: $CHAIN_ID
echo Please wait

if [ $CHAIN_ID -eq 10 ]; then
  L2_URL=https://mainnet.optimism.io
fi


if [ $CHAIN_ID -eq 420 ]; then
  L2_URL=https://goerli.optimism.io
fi


if [ $CHAIN_ID -eq 11155420 ]; then
  L2_URL=https://sepolia.optimism.io
fi

T0=`cast block-number --rpc-url $ETH_RPC_URL` ; sleep 60 ; T1=`cast block-number --rpc-url $ETH_RPC_URL`
PER_MIN=`expr $T1 - $T0`
echo Blocks per minute: $PER_MIN


if [ $PER_MIN -eq 0 ]; then
    echo Not synching
    exit;
fi

# During that minute the head of the chain progressed by thirty blocks
PROGRESS_PER_MIN=`expr $PER_MIN - 30`
echo Progress per minute: $PROGRESS_PER_MIN


# How many more blocks do we need?
HEAD=`cast block-number --rpc-url $L2_URL`
BEHIND=`expr $HEAD - $T1`
MINUTES=`expr $BEHIND / $PROGRESS_PER_MIN`
HOURS=`expr $MINUTES / 60`
echo Hours until sync completed: $HOURS

if [ $HOURS -gt 24 ] ; then
   DAYS=`expr $HOURS / 24`
   echo Days until sync complete: $DAYS
fi
```

Ejecuta el siguiente comando para obtener un tiempo estimado:

```
./run-estimate.sh
```

Una vez todo en marcha, la configuración quedaría de la siguiente manera:

<figure><img src="https://images.mirror-media.xyz/publication-images/CISqBzHBg0JkMlEH4X6tf.png" alt=""><figcaption></figcaption></figure>

### Operaciones <a href="#heading-operaciones" id="heading-operaciones"></a>

Se recomienda iniciar con `op-geth` primero y cerrarlo como último paso.

### **Comunicación con el nodo** <a href="#heading-comunicacion-con-el-nodo" id="heading-comunicacion-con-el-nodo"></a>

Para verificar que nuestro nodo está operativo, podemos ejecutar el siguiente comando en la terminal:

```
curl --request POST \
     --url http://localhost:8545/ \
     --header 'accept: application/json' \
     --header 'content-type: application/json' \
     --data '
{
  "id": 1,
  "jsonrpc": "2.0",
  "method": "eth_blockNumber"
}
'
```

La respuesta esperada debería ser algo similar a lo siguiente:

<figure><img src="https://images.mirror-media.xyz/publication-images/MIwfLiihqZwEat-a74TXb.png" alt=""><figcaption></figcaption></figure>

**Nota**: es posible que al ejecutar este comando directamente desde un copiado y pegado se encuentren errores de sintaxis. Si te funcionó directamente, ¡perfecto! De lo contrario, si te encuentras con errores como falta de URL, es necesario ajustar el comando para que tenga la misma estructura.

Aquí algunos ejemplos de errores comunes que podrías encontrar:

<figure><img src="https://images.mirror-media.xyz/publication-images/WTGQLbXY-A1HO3VfiDJeh.png" alt="La consola CMD no reconocía la estructura al copiar y pegar directamente"><figcaption><p>La consola CMD no reconocía la estructura al copiar y pegar directamente</p></figcaption></figure>

### Conclusión: <a href="#heading-conclusion" id="heading-conclusion"></a>

En conclusión, el despliegue y la sincronización efectiva de un nodo rollup de Optimism requiere un entendimiento técnico y una implementación detallada de varios pasos críticos. Primero, es fundamental reconocer que el datadir proporcionado no se mantiene actualizado de forma constante, lo que obliga al usuario a llevar a cabo una sincronización inicial antes de poder utilizar plenamente el nodo. Este proceso se caracteriza por una serie de mensajes en la consola de `op-node`, los cuales son indicativos del progreso normal y no deben ser motivo de alarma. El hecho de que `op-node` esté buscando una posición en la cola de lotes y eventualmente comience la sincronización es una señal de que todo está procediendo como se espera.

Durante la sincronización, se destacan mensajes que informan el progreso en tiempo real y la incorporación de nuevos bloques. Estos son signos que reflejan la actividad normal del nodo y su interacción con la cadena. Además, es imprescindible el monitoreo continuo mediante herramientas de scripting, como el proporcionado en el ejemplo `run-estimate.sh`, que permite a los operadores estimar el tiempo restante de sincronización, evaluando el número de bloques procesados por minuto.

La operatividad de un nodo rollup es crítica, ya que desempeña un papel central en la escalabilidad y eficiencia de la red. Facilita transacciones más rápidas y económicas al realizar la mayor parte del procesamiento fuera de la cadena principal (layer 1) antes de enviar las transacciones agrupadas para su finalización.

Finalmente, para la interacción y comprobación del estado del nodo, se recomienda el uso de comandos estructurados correctamente, prestando atención a posibles errores de sintaxis que pueden surgir de copiar y pegar directamente desde diferentes fuentes. Todo esto garantiza que el nodo rollup funcione de manera eficiente y segura, lo cual es esencial para el rendimiento óptimo y la integridad de la red Optimism.

¡Gracias por leer hasta aca! Sigamos aportando a la descentralización y desarrollo del ecosistema juntos.

## Fuentes: <a href="#heading-fuentes" id="heading-fuentes"></a>

[optimism/specs/rollup-node.md at 65ec61dde94ffa93342728d324fecf474d228e1f · ethereum-optimism/optimismOptimism is Ethereum, scaled. Contribute to ethereum-optimism/optimism development by creating an account on GitHub.github.com](https://github.com/ethereum-optimism/optimism/blob/65ec61dde94ffa93342728d324fecf474d228e1f/specs/rollup-node.md)

[optimism/specs/README.md at 65ec61dde94ffa93342728d324fecf474d228e1f · ethereum-optimism/optimismOptimism is Ethereum, scaled. Contribute to ethereum-optimism/optimism development by creating an account on GitHub.github.com](https://github.com/ethereum-optimism/optimism/blob/65ec61dde94ffa93342728d324fecf474d228e1f/specs/README.md)

[community.optimism.iocommunity.optimism.io](https://community.optimism.io/docs/developers/build/json-rpc/#rollup-node-op-node)

[community.optimism.iocommunity.optimism.io](https://community.optimism.io/docs/developers/bedrock/how-is-bedrock-different/)

[community.optimism.iocommunity.optimism.io](https://community.optimism.io/docs/developers/bedrock/)

[community.optimism.iocommunity.optimism.io](https://community.optimism.io/docs/developers/bedrock/node-operator-guide/)
