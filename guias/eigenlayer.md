---
icon: e
cover: ../.gitbook/assets/Set D - Wordmark@2x.png
coverY: 0
---

# EigenLayer



{% hint style="info" %}
[EigenLayer](https://www.eigenlayer.xyz) es un protocolo en Ethereum que introduce el concepto de “restaking”, permitiendo la reutilización de ETH y Liquid Staking Tokens (LST) en la capa de consenso. Los nodos en EigenLayer pueden operar de dos maneras: staking individual y delegación.
{% endhint %}

{% embed url="https://mirror.xyz/seedlatam.eth/pZ2zMQEL7JtnITfXyQ6yHZrKVKaElF55OZexwJ1IPoA" %}

## Guía técnica paso a paso: <a href="#heading-guia-tecnica-paso-a-paso" id="heading-guia-tecnica-paso-a-paso"></a>

> #### **¿Qué es EigenLayer?** <a href="#heading-que-es-eigenlayer" id="heading-que-es-eigenlayer"></a>
>
> EigenLayer es un protocolo construido sobre Ethereum que presenta una nueva primitiva en seguridad: el re-staking. Esto posibilita la reutilización de ETH en la capa de consenso -es decir, en staking-. Los usuarios que apuestan ETH de manera nativa o mediante un token de staking líquido (LST) tienen la opción de emplear el protocolo de EigenLayer para reutilizar su ETH del validador o depositar un LST, ampliando así la seguridad en aplicaciones integradas a esta nueva primitiva y por lo tanto obtener recompensas adicionales.

**Presentando a los Node Operators de EigenLayer: Catalizadores de Resiliencia de Red y Stake Delegado**

Los Node Operators se rigen como entidades cruciales, capaces de dar forma a los cimientos mismos del protocolo. Estos operadores, ya sean individuos u organizaciones, asumen un papel activo y preponderante en el ecosistema. En el núcleo de su funcionalidad, los node operators, al registrarse, otorgan a los poseedores de ETH la capacidad de delegar sus activos apostados. Esta delegación, que abarca tanto ETH nativo como LSTs, introduce una capa dinámica de flexibilidad para los apostadores que buscan optimizar su participación en la red.

Pero lo que realmente distingue a los operadores es su compromiso voluntario de brindar una variedad de servicios a los Sistemas de Validación Autónomos (AVSs). Al hacerlo, refuerzan y elevan la seguridad y funcionalidad de toda la red. Es esta participación activa la que impulsa a que sea más que una simple plataforma de staking, transformándola en un ecosistema dinámico donde los node operators desempeñan un papel central.

Los criterios de elegibilidad para convertirse en un Node Operator desafían las restricciones convencionales. No se requiere una cantidad específica de tokens re-stakeados; más bien, cualquier dirección de Ethereum tiene el potencial de servir como uno. La versatilidad se extiende aún más, permitiendo que una dirección asuma sin problemas los roles de re-staker y Operator simultáneamente, aunque esta función dual permanece opcional.

Esencialmente, la participación como node operator no exige la posesión de tokens re-stakeados. Esta flexibilidad abre caminos para que los operadores contribuyan al florecimiento de la red sin estar vinculados por compromisos adicionales. Sin embargo, pueden optar por recibir delegaciones de tokens de otros re-stakers o embarcarse en la autodelegación, asignando su propio equilibrio de tokens re-stakeados para incrementar su peso de participación.

***

**Antes de continuar, unas importantes aclaraciones:**

**En este artículo, explicamos de manera detallada el proceso de armado de un nodo EigenLayer, centrándonos en seguir un orden cronológico de manera no repetitiva.**

**Como se mencionó anteriormente, el nodo se corre en Testnet.**

***

### _**Parte 1: Preparación**_ <a href="#heading-parte-1-preparacion" id="heading-parte-1-preparacion"></a>

**Instalamos componentes esenciales:**

```typescript
sudo apt update && sudo apt -y upgrade 
```

Este comando se encarga de actualizar la lista de paquetes disponibles en los repositorios, y si eso se realiza correctamente, procede a actualizar todos los paquetes instalados en el sistema.

Alternativas según el sistema:

* En sistemas basados en _Red Hat_ (como CentOS o Fedora): `sudo dnf update -y`
* En sistemas basados en _Arch Linux_: sudo pacman -Syu --noconfirm
* En sistemas basados en _SUSE_ (como openSUSE): sudo zypper refresh && sudo zypper update -y

Desglose del comando principal:

* **sudo**: Permite a un usuario ejecutar un comando como el superusuario (root). En este caso, se está utilizando para ejecutar los comandos apt con privilegios de superusuario.
* **apt**: Sistema de gestión de paquetes en distribuciones basadas en Debian, como Ubuntu. Este comando se utiliza para instalar, actualizar y administrar paquetes en el sistema.
* **update**: Este subcomando de apt se utiliza para descargar la información más reciente sobre los paquetes disponibles. Es esencial ejecutar esto antes de realizar una actualización para asegurarse de que la información sea la más reciente.
* **&&**: Es un operador lógico que significa "y". En este contexto, se utiliza para ejecutar el siguiente comando solo si el primero se ejecuta correctamente. Si el comando anterior (sudo apt update) tiene éxito, entonces se ejecutará el siguiente comando.
* **sudo apt -y upgrade**: Este es el comando de actualización propiamente dicho. -y se utiliza para confirmar automáticamente cualquier pregunta que el sistema pueda hacer durante el proceso de actualización. La opción upgrade se encarga de instalar las nuevas versiones de los paquetes ya instalados en el sistema.

**Instalamos códigos esenciales:**

```typescript
sudo apt install pkg-config curl git-all build-essential libssl-dev libclang-dev ufw 
```

Instalará todos estos paquetes en el sistema, asegurando que estén disponibles para su uso posterior en la compilación, desarrollo y configuración del sistema.

Alternativas según el sistema:

* En sistemas basados en _Red Hat_ (como CentOS o Fedora): `sudo dnf install pkgconfig curl git-all gcc-c++ openssl-devel clang ufw`
* En sistemas basados en _Arch Linux_: `sudo pacman -S pkg-config curl git base-devel openssl clang ufw`
* En sistemas basados en _SUSE_ (como openSUSE): `sudo zypper install pkg-config curl git gcc-c++ libopenssl-devel libclang-devel ufw`

<figure><img src="https://images.mirror-media.xyz/publication-images/o286S6U1j02aRrv7MAMGI.png" alt=""><figcaption></figcaption></figure>

Desglose del comando:

* **install**: Subcomando de apt que se utiliza para instalar nuevos paquetes en el sistema.
* **pkg-config**: Herramienta que se utiliza para devolver la información de configuración necesaria para compilar y vincular programas que utilizan bibliotecas.
* **curl**: Herramienta de línea de comandos para transferir datos con URL. Se utiliza comúnmente para descargar archivos desde la web.
* **git-all**: Paquete que incluye todas las herramientas de Git, un sistema de control de versiones distribuido.
* **build-essential**: Paquete que instala los elementos esenciales necesarios para compilar programas en Linux, como el compilador GCC y otras herramientas de compilación.
* **libssl-dev**: Biblioteca de desarrollo para OpenSSL, que se utiliza para implementar protocolos criptográficos seguros.
* **libclang-dev**: Biblioteca de desarrollo para Clang, un conjunto de herramientas de compilación basado en LLVM.
* **ufw**: Un front-end para iptables, el sistema de filtrado de paquetes de Linux. UFW facilita la configuración de reglas de firewall.
* Avanzamos colocando ‘Y’ luego del comando.

_Comprobamos si Docker está instalado:_

```typescript
docker version
```

Si no está instalado, ejecutamos:

```typescript
sudo apt-get install ca-certificates curl gnupg lsb-release 
```

Primeramente, asegurar que el sistema tenga instalados los certificados de autoridad necesarios, herramientas para descargar archivos desde la web (curl), herramientas de encriptación y firma digital (gnupg), y la capacidad de obtener información sobre la distribución Linux (lsb-release).

Alternativas según el sistema:

* En sistemas basados en _Red Hat_ (como CentOS o Fedora): `sudo dnf install ca-certificates curl gnupg lsb`
* En sistemas basados en _Arch Linux_: `sudo pacman -S ca-certificates curl gnupg lsb-release`
* En sistemas basados en _SUSE_ (como openSUSE): `sudo zypper install ca-certificates curl gpg2 lsb-release`

Desglose del comando:

* **apt-get**: Similar a apt, es un front-end para el sistema de gestión de paquetes APT (Advanced Package Tool) en sistemas basados en Debian.
* **ca-certificates**: Este paquete incluye certificados de autoridad (CA) que son utilizados por diversas aplicaciones para verificar la autenticidad de las conexiones seguras (HTTPS).
* **curl**: Herramienta de línea de comandos para transferir datos con URL. Se utiliza comúnmente para descargar archivos desde la web.
* **gnupg**: El paquete GnuPG proporciona herramientas para la encriptación y firma digital de datos, y se utiliza comúnmente para verificar la autenticidad de paquetes descargados.
* **lsb-release**: Este paquete proporciona el comando lsb\_release, muestra información sobre la distribución Linux en uso.

_Ahora agregamos la clave GPG oficial de Dockerers:_

```typescript
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

Comandos alternativos:

* _Sistemas basados en Red Hat (como CentOS o Fedora):_

`sudo mkdir -p /etc/pki/rpm-gpg curl -fsSL https://download.docker.com/linux/ubuntu/gpg`

`sudo gpg --dearmor -o /etc/pki/rpm-gpg/docker.gpg`

_Desglose del comando:_

* **sudo mkdir -p /etc/apt/keyrings**: Crea un directorio llamado "keyrings" dentro de "/etc/apt". La opción -p garantiza que se creen directorios padre si no existen.
* **curl -fsSL** [**https://download.docker.com/linux/ubuntu/gpg**](https://download.docker.com/linux/ubuntu/gpg): Descarga la clave GPG de Docker desde la URL proporcionada. La opción -fsSL garantiza una descarga silenciosa y segura.
* **sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg:** Utiliza GPG para desarmar la clave descargada y guarda el resultado en un archivo llamado "docker.gpg" dentro del directorio "/etc/apt/keyrings".

_Luego para configurar Repositorio: sólo debemos copiar y pegar._

```typescript
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Desglose de comandos:

* **"deb \[arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg]** [**https://download.docker.com/linux/ubuntu**](https://download.docker.com/linux/ubuntu)**$(lsb\_release -cs) stable":** Construye la línea de configuración del repositorio de Docker. Aquí hay una desglose de los elementos clave:
  * **\[arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg]**: Especifica la arquitectura del sistema y la ubicación de la clave GPG para verificar la autenticidad de los paquetes.
  * [**https://download.docker.com/linux/ubuntu**](https://download.docker.com/linux/ubuntu): La URL del repositorio de Docker para Ubuntu.
  * **$(lsb\_release -cs)**: Sustituye esto con el nombre del código de versión de Ubuntu en uso (por ejemplo, "focal" para Ubuntu 20.04).
  * **stable**: Indica que se utilizará la rama "stable" del repositorio de Docker
  * **sudo tee /etc/apt/sources.list.d/docker.list:** Utiliza tee con sudo para escribir la línea de configuración del repositorio en el archivo
  * **"/etc/apt/sources.list.d/docker.list"**. Esto añade la configuración de Docker a las fuentes de apt.
  * **> /dev/null**: Redirige la salida estándar (que sería la línea de configuración) a la nada (es decir, descartando la salida), evitando así que se muestre en la terminal.

_Acá es importante otorgar permiso al archivo Docker antes de actualizar el índice del paquete_

```typescript
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

Ajusta los permisos del archivo GPG de Docker para permitir que todos los usuarios del sistema puedan leerlo. Esto es necesario para que el sistema pueda utilizar la clave GPG durante el proceso de actualización de apt.

```typescript
sudo apt-get update 
```

Actualiza la lista de paquetes disponibles en los repositorios configurados en el sistema.

_Desglose de comandos:_

* _**chmod a+r /etc/apt/keyrings/docker.gpg**_: Modifica los permisos del archivo "/etc/apt/keyrings/docker.gpg" para que sea legible (_readable_) por todos los usuarios (a+r significa "dar permisos de lectura a todos").

_después de otorgar, índice actualizado, instalamos la última versión de docker_

```typescript
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

Comandos alternativos:

* En sistemas basados en _Red Hat_ (como CentOS o Fedora: `sudo dnf install docker-ce docker-ce-cli containerd.io docker-compose`
* En sistemas basados en _Arch Linux_: `sudo pacman -S docker docker-compose`
* En sistemas basados en _SUSE_ (openSUSE): `sudo zypper install docker docker-compose`

_Ahora instalamos Docker componer:_

```typescript
sudo apt install docker-compose
```

<figure><img src="https://images.mirror-media.xyz/publication-images/O8xx-QIo2Icd1j22OcwD4.png" alt=""><figcaption></figcaption></figure>

_Verifiquemos si la **INSTALACIÓN DEL MOTOR** tiene éxito ejecutando el hello-world_

```typescript
sudo docker run hello-world
```

_Desglose de comandos:_

* _**docker run:**_ _Inicia la ejecución de un contenedor Docker a partir de una imagen._
* _**hello-world:**_ _Especifica el nombre de la imagen del contenedor que se utilizará. En este caso, "hello-world" es una imagen de Docker simple y ligera que se utiliza comúnmente para verificar si la instalación de Docker funciona correctamente._

_Deberíamos conseguir ver esto:_

<figure><img src="https://images.mirror-media.xyz/publication-images/GzSkco_WrS2EUY1YbfTF6.png" alt=""><figcaption></figcaption></figure>

_Ahora comprobamos Docker componer versión:_

```javascript
docker-compose -v
```

<figure><img src="https://images.mirror-media.xyz/publication-images/Ckfdl0OjuqJAIdFa5fWZ1.png" alt=""><figcaption></figcaption></figure>

***

### _**Parte 2: Instalación del Nodo**_ <a href="#heading-parte-2-instalacion-del-nodo" id="heading-parte-2-instalacion-del-nodo"></a>

**Instalación del SCREEN:**

```typescript
sudo apt install screen
```

Especifica el paquete que se va a instalar

```typescript
sudo screen -S Eigenlayer 
```

Crea una nueva sesión de Screen

_**Ahora vamos con el lenguaje de Instalación:**_

```typescript
wget https://golang.org/dl/go1.21.4.linux-amd64.tar.gz 
```

Mediante wget descarga la distribución de Go versión 1.21.4 para sistemas Linux con arquitectura AMD64 desde la URL

<figure><img src="https://images.mirror-media.xyz/publication-images/3DoN3MivxmzaEbY2YQQ7Q.png" alt=""><figcaption></figcaption></figure>

```typescript
tar -C /usr/local -xzf go1.21.4.linux-amd64.tar.gz
```

Descarga el archivo mediante tar

_Desglose del comando:_

* _**tar**_: Desempaquetar el contenido del archivo.
* _**-C /usr/local**_: Extraerán en el directorio /usr/local.
*   **-xzf**: _Son opciones de tar que indican las acciones a realizar durante la extracción:_

    _-x: Extraer (desempaquetar)._\
    _-z: Utilizar gzip para descomprimir._\
    _-f: Especifica el nombre del archivo a descomprimir._

```typescript
export PATH=$PATH:/usr/local/go/bin 
```

_Desglose del comando:_

* _**export**_: Este comando se utiliza para definir o exportar variables de entorno en el shell.
* _**PATH=$PATH:/usr/local/go/bin**_: Esto modifica la variable PATH. PATH es una variable de entorno que contiene una lista de directorios separados por dos puntos (:). Es utilizada por el sistema para buscar ejecutables de comandos.
* _**$PATH**_\* toma el valor actual de la variable PATH.\*
* _**:/usr/local/go/bin**_\* agrega el directorio /usr/local/go/bin al final de la variable PATH, separado por dos puntos. Esto significa que el sistema buscará ejecutables en este directorio cuando se introduzca un comando en el terminal.

```typescript
go version
```

_**Instalación del CLI:**_

_**Instalación de Github EigenLayer**_

```typescript
git clone https://github.com/Layr-Labs/eigenlayer-cli.git
cd eigenlayer-cli
mkdir -p build
go build -o build/eigenlayer cmd/eigenlayer/main.go
```

Clonan un repositorio Git de EigenLayer, se mueven al directorio recién creado, crean un directorio para los archivos compilados y luego compilan el programa principal de EigenLayer, almacenando el ejecutable resultante en el directorio "build".

_Desglose del comando:_

* _**mkdir -p build:**_ _Crea un directorio llamado "build". La opción -p garantiza que se creen directorios padre si no existen._
* _**go build -o build/eigenlayer cmd/eigenlayer/main.go:**_ _Utiliza el comando go build para compilar el programa principal de EigenLayer (cmd/eigenlayer/main.go) y genera el ejecutable resultante. La opción -o build/eigenlayer especifica el nombre y la ubicación del ejecutable generado, que será almacenado en el directorio "build" con el nombre "eigenlayer"._

<figure><img src="https://images.mirror-media.xyz/publication-images/wm1VLEe60umEYoz86fRn1.png" alt=""><figcaption></figcaption></figure>

```typescript
cp ./build/eigenlayer /usr/local/bin/
eigenlayer
```

<figure><img src="https://images.mirror-media.xyz/publication-images/PBeTYNlySiDSux1vnDDcv.png" alt=""><figcaption></figcaption></figure>

**Creación de claves:**

```typescript
eigenlayer operator keys create --key-type ecdsa [keyname]
eigenlayer operator keys create --key-type bls [keyname]
```

¿Cómo vemos luego las claves?

```typescript
eigenlayer operator keys list
```

_\*NOTA: Resguardar siempre las claves de manera segura\*_

**Registro del Operador:**

Configuramos la metadata, creamos el GitHub, nombramos el archivo metadata.json y luego es simplemente copiar/pegar lo siguiente modificando lo que está entre < >

```typescript
{
 "name": "<OPERATOR_NAME>",
 "website": "<YOUR_WEBSITE>",
 "description": "<DESCRIPTION>",
 "logo": "https://www.example.com/logo.png",
 "twitter": "<YOUR_TWITTER>"
}
```

Después de completar, vamos a ‘`Raw’`, donde te da el enlace y lo pegamos en `‘operator.yaml’`

En la carpeta:

```typescript
cd pkg/operator/config
```

Crear la carpeta ‘_operator.yaml_’

```typescript
touch operator.yaml
```

Editar la configuración del archivo:

```typescript
 nano operator.yaml
```

`nano operator.yaml`

A continuación editamos lo siguiente:

```typescript
address: <Dirección_de_tu_Wallet>earnings_receiver_address: <Dirección_de_tu_Wallet>metadata_url: <METADATA_Archivo_RAW>eth_rpc_url: <API_ALCHEMY> (Buscar un rpc)private_key_store_path: /root/.eigenlayer/operator_keys/<Nombre_KEY>.ecdsa.key.jsonbls_private_key_store_path: /root/.eigenlayer/operator_keys/<Nombre_KEY>.bls.key.json
#All the below fields are required for successful operator registration.
operator:#This is the standard Ethereum address format (ex: 0x6a8c0D554a694899041E52a91B4EC3Ff23d8aBD5) of your operator#which is the ecdsa key you created or imported using EigenLayer CLIaddress: <Dirección_de_la_Wallet>#This is the standard Ethereum address format (ex: 0x6a8c0D554a694899041E52a91B4EC3Ff23d8aBD5)#This is the address where your operator will receive earnings. This could be same as operator address earnings_receiver_address: <Dirección_de_la_Wallet>#This is the standard Ethereum address format (0x...)#This is the address which operator will use to approve delegation requests from stakers.#if set, this address must sign and approve new delegation from Stakers to this Operator#This is optional, so you can leave it  with the default value for un-gated delegation requestsdelegation_approver_address: 0x0000000000000000000000000000000000000000#Please keep this field to 0, and it can be updated later using EigenLayer CLIstaker_opt_out_window_blocks: 0metadata_url: <METADATA_Archivo_RAW>#EigenLayer Slasher contract address#This will be provided by EigenLayer teamel_slasher_address: 0xD11d60b669Ecf7bE10329726043B3ac07B380C22#Address of BLS Public Key Compendium contract#This will be provided by EigenLayer teambls_public_key_compendium_address: 0xc81d3963087Fe09316cd1E032457989C7aC91b19#ETH RPC URL to the ethereum node you are using for on-chain operationseth_rpc_url: <API_ALCHEMY>#Signer Type to use#Supported values: local_keystoresigner_type: local_keystore#Full path to local ecdsa private key store fileprivate_key_store_path: /root/.eigenlayer/operator_keys/<Nombre_KEY>.ecdsa.key.json#Full path to local bls private key store filebls_private_key_store_path: /root/.eigenlayer/operator_keys/<Nombre_KEY>.bls.key.json#Chain ID: 1 for mainnet, 5 for Goerli, 31337 for localchain_id: 5
```

**Registremos el Operador:**

_\*Nota: Tener geth en la wallet antes de ejecutar el siguiente paso_

```typescript
eigenlayer operator register operator.yaml
```

Verificamos el estado del Operador con el siguiente comando:

```
eigenlayer operator status operator.yaml
```

También se puede chequear después de unos minutos en [este link](https://goerli.eigenlayer.xyz/operator).

***

\*_Todo el material que se presenta aquí es de acceso público. Para obtener información detallada y seguir los pasos en inglés, se puede visitar la_ [_web oficial_](https://docs.eigenlayer.xyz/eigenlayer/operator-guides/operator-installation)_._
