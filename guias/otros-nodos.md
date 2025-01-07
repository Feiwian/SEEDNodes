---
icon: share-nodes
---

# Otros Nodos



{% tabs %}
{% tab title="Farcaster" %}
### Introduccion: <a href="#block-dab9686f21c147978ecf397e4ee55361" id="block-dab9686f21c147978ecf397e4ee55361"></a>

[Farcaster](https://www.farcaster.xyz/) es un protocolo de redes sociales descentralizado construido sobre [Optimism](https://decrypt.co/es/96255/que-es-optimism-usando-rollups-para-ayudar-a-escalar-ethereum/) donde su aplicación más popular es Warpcast, una plataforma similar a Twitter.

Desde SEEDNode creamos una guía paso a paso para ejecutar tu propio nodo, empecemos.

## Requerimientos: <a href="#block-d003c6531164497f99d8dbdcfbce839c" id="block-d003c6531164497f99d8dbdcfbce839c"></a>

* 16 GB de RAM
* 4 núcleos de CPU o vCPU
* 40 GB de almacenamiento gratuito
* Una dirección IP pública con los puertos 2281 - 2283 expuestos
* Puntos finales RPC para Ethereum y Optimism Mainnet. (use [**Alchemy** ](https://www.alchemy.com/)[**Infura** ](https://www.infura.io/)[**QuickNode**](https://www.quicknode.com/))

### **Buscar tu FID de Warpcast** <a href="#block-483de4afabc84e529b74f145ac38166d" id="block-483de4afabc84e529b74f145ac38166d"></a>

Primero nos creamos una cuenta en [warpcast.com](https://warpcast.com/) , donde aproximadamente te va a salir unos 5 USD. Luego vas hasta tu perfil, selecciona “About” y anota tu FID.

![image](https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/7a36b673-7ef7-4a3d-8e51-90679e12dbe6/Untitled/w=384,quality=90,fit=scale-down)![image](https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/da66072a-6484-4a87-991b-f9ab5de2610c/Untitled/w=384,quality=90,fit=scale-down)![image](https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/62a81c78-6fd5-4bb5-a3d8-737a434fb690/Untitled/w=384,quality=90,fit=scale-down)

### **Configurar una API RPC con Infura** <a href="#block-261af60c77644da0a7db77d2c2412d42" id="block-261af60c77644da0a7db77d2c2412d42"></a>

Registrate para obtener una cuenta en [https://app.infura.io](https://app.infura.io/) . Selecciona 'My First Key', luego eleji “Endpoints for Ethereum Mainnet and Optimism Mainnet” y guardá los cambios.

## **Iniciar el nodo** <a href="#block-2ed0909a37204b8ca6e98d5d757e20ad" id="block-2ed0909a37204b8ca6e98d5d757e20ad"></a>

**Opciones para configurarlo:**

Podemos configurarlo de dos maneras: teniendo un propio Hardware o en su defecto, con un _servidor privado virtual (VPS)._

* **Hardware propio:** ideal para usuarios con experiencia técnica y control total.
* **Servidor privado virtual (VPS):** una opción más accesible y fácil de usar.

En esta guía utilizaremos [Contabo](https://contabo.com/en/), un proveedor de servidores privados virtuales (VPS), y su plan de 100 GB para garantizar un funcionamiento estable del nodo a largo plazo.

https://contabo.com/en/

![image](https://mirror.xyz/_next/image?url=https%3A%2F%2Fimages.mirror-media.xyz%2Fpublication-images%2FxuEK8O4tKzCxUTKod76O4.png\&w=3840\&q=75)

Una vez seleccionado el tipo de plan (Cloud VPS 1), región (Alemania) y almacenamiento (400 GB); vamos a ‘_Aplicaciones y paneles_’ > _Docker_

![image](https://mirror.xyz/_next/image?url=https%3A%2F%2Fimages.mirror-media.xyz%2Fpublication-images%2Ff7gsM7B4C1SZ_xeQoc_yA.png\&w=3840\&q=75)

Iniciamos sesión con usuario y contraseña:

![image](https://mirror.xyz/_next/image?url=https%3A%2F%2Fimages.mirror-media.xyz%2Fpublication-images%2Fb_84JVBEUhFRtSa0xI8qG.png\&w=3840\&q=75)

Y por último, antes de pagar, configuramos el almacenamiento (predeterminado), redes (predeterminada) y complementos (predeterminado).

Posterior a todo esto, vamos a instalar [puTTy](https://www.putty.org/) , pueden hacerlo desde Windows descargando desde la web, o en terminal Ubuntu con los siguientes pasos:

```
sudo add-apt-repository universe
```

Agregas los repositorios con el siguiente comando.

![image](https://mirror.xyz/_next/image?url=https%3A%2F%2Fimages.mirror-media.xyz%2Fpublication-images%2FsfhxskY4_sU-RcFmFzxdp.png\&w=3840\&q=75)Copy

```
sudo apt update
```

Actualizas con el siguiente comando.

![image](https://mirror.xyz/_next/image?url=https%3A%2F%2Fimages.mirror-media.xyz%2Fpublication-images%2FS7jJ17U_VgMFNnHuvQWOi.png\&w=3840\&q=75)Copy

```
sudo apt install putty
```

Instalas putty con el siguiente comando.

![image](https://mirror.xyz/_next/image?url=https%3A%2F%2Fimages.mirror-media.xyz%2Fpublication-images%2F9xEzAJMmZWy6Vepq-Jf7L.png\&w=3840\&q=75)

Vamos a actividad y buscamos ‘SSH’, abrimos:

![image](https://mirror.xyz/_next/image?url=https%3A%2F%2Fimages.mirror-media.xyz%2Fpublication-images%2FZQVRZzi8bXEFlkDi01vF1.png\&w=3840\&q=75)

Ingresas la dirección IP del VPS y luego haga clic en _OPEN_

* Login : root
* Password: (es la que configuraste anteriormente en Contabo VPS)

![image](https://mirror.xyz/_next/image?url=https%3A%2F%2Fimages.mirror-media.xyz%2Fpublication-images%2FmljRazfdEN-T9_dEqOZ8f.png\&w=3840\&q=75)

### **Iniciar el nodo Farcaster en VPS** <a href="#block-916e745662314a9eb7ff54efb30e06c9" id="block-916e745662314a9eb7ff54efb30e06c9"></a>

Iniciamos sesión en nuestro VPS usando SSH y creamos una sesión de pantalla para garantizar que el nodo quede funcionando las 24 /7 y:

**Instalamos los componentes esenciales:**

```
sudo apt update && sudo apt -y upgrade
```

Este comando se encarga de actualizar la lista de paquetes disponibles en los repositorios, y si eso se realiza correctamente, procede a actualizar todos los paquetes instalados en el sistema.

Alternativas según el sistema:

* En sistemas basados en Red Hat (como CentOS o Fedora): `sudo dnf update -y`
* En sistemas basados en Arch Linux: `sudo pacman -Syu --noconfirm`
* En sistemas basados en SUSE (como openSUSE): `sudo zypper refresh && sudo zypper update -y`

**Instalamos el SCREEN**

```
sudo apt install screen -y
screen -S Hubble
```

Ejecutamos el script:

```
curl -sSL https://download.thehubble.xyz/bootstrap.sh | bash
```

Dato: Acá puede ser que “curl” no este instalado, de ser así, ejecutamos:

```
sudo apt-get install curl
```

Esperamos a que se complete el script y luego completamos:

1\. Red principal RPC Ethereum

2\. Red principal RPC OP

3\. FID

Donde el “RPC Ethereum Mainnet” y el “RPC OP Mainnet” son los endpoints que registramos en el punto 2. El “FID” es tu identificador de tu cuenta de Warpcast.

Después de ejecutar el script, si tu VPS muestra la misma imagen, podemos decir que tu nodo está operativo.

![image](https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/e24ba60c-7e70-45f6-b8ec-fcebcfa26ae4/Untitled/w=3840,quality=90,fit=scale-down)

Si cerrás la terminal y querés volver a ingresar a la pantalla del nodo de Farcaster que se esta ejecutando, volvé a iniciar sesión en tu VPS y ejecuta el comando:

```

screen -r Hubble
```

Si el nodo deja de funcionar o hay una nueva versión disponible, se puede actualizar con el siguiente comando:

```
cd ~/hubble && ./hubble.sh upgrade
```

Todo el material que se presenta aquí es de acceso público. Para obtener información mas detallada los invitamos a ver los docs en: [https://docs.farcaster.xyz/hubble/install](https://docs.farcaster.xyz/hubble/install)
{% endtab %}

{% tab title="Aleo" %}
{% embed url="https://x.com/SeedsPuntoEth/status/1809648719186510037" %}
{% endtab %}

{% tab title="Taiko" %}
{% embed url="https://mirror.xyz/seedlatam.eth/UV9r_j2u6GIbAF8gpMBxgizsJkYilsa-erN1YYXCQ_s" %}
[Consejos extra aquí](https://x.com/SeedsPuntoEth/status/1758204851761062086)!
{% endembed %}
{% endtab %}

{% tab title="Initia" %}
## Prerequisitos <a href="#block-7262183dc8e34f2fba2b6ce9a605e05b" id="block-7262183dc8e34f2fba2b6ce9a605e05b"></a>

### Hardware <a href="#block-40e58409a29e4cd2b2ff2f8df169fbde" id="block-40e58409a29e4cd2b2ff2f8df169fbde"></a>

Los requisitos minimos para el hardware en initia mainnet son:

* CPU: 16 nucleos
* Memory: 32GB RAM
* Disk: 2 TB NVMe/SSD Almacenamiento con Write Throughput > 1000 MiBps
* Bandwidth: 100 Mbps

Es importante resaltar si estar haciendo pruebas para testnet, los requisitos son mucho mas bajos.

### Software <a href="#block-e202dd6a85264d418a0cb6ebabb567bd" id="block-e202dd6a85264d418a0cb6ebabb567bd"></a>

* Go v1.22+ or higher
* Git
* curl
* jq

#### Instalar Go <a href="#block-895e3ebdf6174128885f707ffff8a730" id="block-895e3ebdf6174128885f707ffff8a730"></a>

Correremos en la terminal:

```json
sudo snap install go --classic
```

Vemos que version tenemos (debe ser +1.22.x)

```json
go version
```

Si no nos detecta el comando, reiniciemos la PC.

Deberiamos obtener:

#### Instalación git <a href="#block-2b978c25e0314c6f94dc4a9fd4f7d8de" id="block-2b978c25e0314c6f94dc4a9fd4f7d8de"></a>

Corremos:

```powershell
sudo apt install git
```

Comprobamos la version con “git version”

#### Instalacion curl y jq <a href="#block-9332391f1d3840edbc50e7f263ba397f" id="block-9332391f1d3840edbc50e7f263ba397f"></a>

Instalacion curl

```powershell
sudo apt install curl
```

Comprobamos la version con “curl —version”

Instalacion jq

```powershell
sudo apt install jq
```

Comprobamos la version con “jq —version”

## **Instalación initia** <a href="#block-dff54bf9e250428d89df8d8d836f0159" id="block-dff54bf9e250428d89df8d8d836f0159"></a>

Creamos una carpeta donde querramos, por ejemplo en el escritorio:

```powershell
mkdir InitiaNode
```

Clonamos el repositorio oficial

```powershell
git clone https://github.com/initia-labs/initia
```

Entramos dentro de la carpeta que descargamos

```powershell
cd initia
```

Nos cambiamos a la version v0.2.15:

```powershell
git checkout v0.2.15
```

Con este comando:

* Git busca la etiqueta `v0.2.15` en el repositorio.
* Cambiará el árbol de trabajo y el índice al estado del commit correspondiente a esa etiqueta.
* Estarás en un estado "detached HEAD", lo que significa que no estarás en ninguna rama específica, sino en el commit exacto asociado con `v0.2.15`.

Ahora buildeamos:

```powershell
make build
```

Movemos initiad al bin:

```powershell
sudo mv build/initiad /usr/local/bin/
```

Comprobamos que se haya instalado correctamente:

```powershell
intiad version
```

Ejecutamos initiad utilizando algun \[moniker], el moniker es un nombre para nuestro nodo. Puede contener solamente caracteres ASCII y no pueden ser mas de 70 caracteres.

```powershell
initiad init [moniker] --chain-id initiation-1
```

Esto va a generar una private key que se va a guardar en `~/.initia/config/priv_validator_key.json`

Para verlo, debemos visualizar los archivos ocultos:

Navegamos dentro de ./initia y vamos a config

el archivo priv\_validator\_key.json estos 3 datos importantes:

* Address
* Public key
* Private key

Descargamos el genesis.json

```powershell
wget https://initia.s3.ap-southeast-1.amazonaws.com/initiation-1/genesis.json
```

Y lo copiamos a `~/.initia/config/genesis.json`

```powershell
cp genesis.json ~/.initia/config/genesis.json
```

Utilizamos este comando para prevenir intentos continuos de reconexión.

```powershell
sed -i -e 's/external_address = \"\"/external_address = \"'$(curl httpbin.org/ip | jq -r .origin)':26656\"/g' ~/.initia/config/config.toml
```

* Evitará intentos continuos de reconexión, el puerto P2P por defecto es 26656.
* El comando `sed` modifica el archivo de configuración `config.toml` para establecer tu dirección IP pública y el puerto 26656 como `external_address`.

**Iniciamos el nodo**

🎬Para mayor comprensión, dejamos a disposición un video ilustrativo con todos los pasos descriptos en la guía:

{% embed url="https://www.youtube.com/watch?v=CCO0MTeq52U" %}
{% endtab %}
{% endtabs %}
