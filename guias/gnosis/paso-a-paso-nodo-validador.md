---
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Paso a Paso - Nodo Validador

{% embed url="https://mirror.xyz/seedlatam.eth/M-s9q4jJbq12n5JPwT2qdlqjbf9BzLOsVtFQdfhd-bM" %}

_NOTA: La siguiente guía es la opción que creo más sencilla para montar un validador de Gnosis. Existen otras alternativas que se pueden implementar y que son igualmente válidas._

#### A. Instalación de Dappnode: <a href="#heading-a-instalacion-de-dappnode" id="heading-a-instalacion-de-dappnode"></a>

Tener en cuenta que estos requerimientos de PC son los mínimos y que debes tener Ubuntu ya instalado o bien, la distribución de Linux de preferencia:

* Procesador Intel Core i3
* 16 GB de RAM
*   Almacenamiento NVMe de 2 TB o un SSD rápido

    1\. Para comenzar, necesitas abrir una terminal y ejecutar el siguiente comando que instalará todos los pre-requisitos:

```bash
sudo wget -O - <https://prerequisites.dappnode.io> | sudo bash
```

<figure><img src="https://images.mirror-media.xyz/publication-images/Xmbh6tbzt_xUmSxa0tMYE.png" alt=""><figcaption></figcaption></figure>

2\. Luego de ello, puedes proceder a instalar Dappnode utilizando el siguiente comando en la misma terminal:

```bash
sudo wget -O - <https://installer.dappnode.io> | sudo bash
```

<figure><img src="https://images.mirror-media.xyz/publication-images/hX90n24VBwKz-lAQYpm85.png" alt=""><figcaption></figcaption></figure>

3\. Para finalizar y aplicar los cambios, debes reiniciar el sistema con el siguiente comando:

```bash
shutdown -r now
```

4\. Una vez reiniciado, vas a abrir nuevamente una terminal y ejecutar el siguiente comando para acceder a la carpeta "DNCORE":

```bash
cd /usr/src/dappnode/DNCORE
```

<figure><img src="https://images.mirror-media.xyz/publication-images/744axYrbjdCjTe2MZBrVR.png" alt=""><figcaption></figcaption></figure>

5\. Dentro de la carpeta **DNCORE**, vas a ejecutar el comando “source” para que configure las variables de entorno:

```bash
source .dappnode_profile
```

6\. Una vez configurado, se debe acceder a Dappnode. Con el comando `dappnode_help`, podes ver todos los comandos disponibles.

<figure><img src="https://images.mirror-media.xyz/publication-images/wRfhj0Nv8VWPG3qOZG7HT.png" alt=""><figcaption></figcaption></figure>

7\. Ahora bien, vas a iniciar Dappnode con el siguiente comando:

```bash
dappnode_start
```

<figure><img src="https://images.mirror-media.xyz/publication-images/SP3oV6rgFn7ZKg531k0O6.png" alt=""><figcaption></figcaption></figure>

#### B. Acceso al Dappnode: <a href="#heading-b-acceso-al-dappnode" id="heading-b-acceso-al-dappnode"></a>

9\. Ver las credenciales de **Wireguard VPN**, que luego te permitirán conectarte a **my.dappnode**, con el siguiente comando:

```bash
dappnode_wireguard
```

<figure><img src="https://images.mirror-media.xyz/publication-images/A8fECwB404PfwO93ObbQR.png" alt=""><figcaption></figcaption></figure>

10\. Descargar e instalar **Wireguard** desde el siguiente enlace y cargar las credenciales correspondientes (también está la opción de utilizar **OpenVPN**):

[Installation - WireGuardwww.wireguard.com](https://www.wireguard.com/install/)

11\. Luego, copia el texto de las credenciales:

<figure><img src="https://images.mirror-media.xyz/publication-images/as3xV98OyA66V44g9pIBY.png" alt=""><figcaption></figcaption></figure>

12\. Abre **Wireguard** y selecciona “**Add empty tunnel”** (en la parte inferior izquierda hay un ícono de "+"; al hacer clic, y te aparece esa opción).

13\. Asigna un nombre, por ejemplo: **SEEDLatam**, y en el campo de abajo, vas a pegar el texto que copiaste desde la terminal y haces clic en **Guardar**.

<figure><img src="https://images.mirror-media.xyz/publication-images/GSsPWqMWGnCPCmJ1oQsIm.png" alt=""><figcaption></figcaption></figure>

14\. Haz clic en el botón **Activar**, y tu configuración debería quedar lista de esta manera:

<figure><img src="https://images.mirror-media.xyz/publication-images/s_fFvNfhIRRmsclKWrEmy.png" alt=""><figcaption></figcaption></figure>

15\. Con **Wireguard** activado, podrás acceder desde tu navegador a [http://my.dappnode/dashboard](http://my.dappnode/dashboard), donde primero te registrarás creando un nombre de usuario y una contraseña.

#### C. Instalación de los clientes del validador: <a href="#heading-c-instalacion-de-los-clientes-del-validador" id="heading-c-instalacion-de-los-clientes-del-validador"></a>

16\. Una vez dentro, navega a **Stakers > Gnosis Chain**.

<figure><img src="https://images.mirror-media.xyz/publication-images/spB1jLN5rt-5VcCJhqscm.png" alt=""><figcaption></figcaption></figure>

17\. Selecciona el cliente de ejecución y consenso que prefieras, así como **Web3signer Gnosis**, y para finalizar, haz clic en **"Apply Changes"**.

#### **D. Generación de claves:** <a href="#heading-d-generacion-de-claves" id="heading-d-generacion-de-claves"></a>

18\. Descarga el paquete correspondiente a tu sistema operativo desde el siguiente enlace e instálalo:

[Releases · alexpeterson91/Gnosis-Wagyu-Key-GenGnosis Chain port of StakeHouse's Wagyu Key Gen. Contribute to alexpeterson91/Gnosis-Wagyu-Key-Gen development by creating an ac…github.com](https://github.com/alexpeterson91/Gnosis-Wagyu-Key-Gen/releases)

<figure><img src="https://images.mirror-media.xyz/publication-images/mSkRDUDzPxXJwpCHojdt8.png" alt=""><figcaption></figcaption></figure>

19\. Eliges “Create new mnemonic recovery phrase”

<figure><img src="https://images.mirror-media.xyz/publication-images/6JQFDlKoppqt7YFvDOrME.png" alt=""><figcaption></figcaption></figure>

20\. Create

IMPORTANTE: Tomamos todas las medidas de seguridad y copiamos bien nuestras palabras

**Vas a tener que checkear todas las palabras cargandolas manualmente una por una.**

<figure><img src="https://images.mirror-media.xyz/publication-images/bbNZN7TAxg1TX-FjaPO47.png" alt=""><figcaption></figcaption></figure>

21\. Eliges un nuevo password, agregas una wallet de retiro y le das a Next

<figure><img src="https://images.mirror-media.xyz/publication-images/ge3464h73HrCt0mjZ7apt.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://images.mirror-media.xyz/publication-images/7nlMrAK7O7Z-NeDaD_u6p.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://images.mirror-media.xyz/publication-images/gBLQviom2o-RYCKCifgZY.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://images.mirror-media.xyz/publication-images/DIqekBGhuHT8X9Ox_8LEt.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://images.mirror-media.xyz/publication-images/4aflN8Si3YuFHk9xsIFqm.png" alt=""><figcaption></figcaption></figure>

22\. Esto va a dejarte 2 archivos importantes para continuar con el proceso:

<figure><img src="https://images.mirror-media.xyz/publication-images/BUvmlw32IjgCpyqnk53xK.png" alt=""><figcaption></figcaption></figure>

#### E. Carga de claves en Dappnode: <a href="#heading-e-carga-de-claves-en-dappnode" id="heading-e-carga-de-claves-en-dappnode"></a>

23\. Luego, vas a regresar al navegador: [http://my.dappnode/stakers/ethereum](http://my.dappnode/stakers/ethereum)

Y abrir Remote signer - Web3signer Gnosis - Upload Keystores

<figure><img src="https://images.mirror-media.xyz/publication-images/ugo_P_CYlLICXGp0sVEWk.png" alt=""><figcaption></figcaption></figure>

Se va a abrir la siguiente pestaña donde se va a importar el archivo Keystore.json que se descargó anteriormente

<figure><img src="https://images.mirror-media.xyz/publication-images/JbF56HMgmnRHppOhWeW3v.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://images.mirror-media.xyz/publication-images/cmgv6cBO-u2DkEDQEQ8q-.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://images.mirror-media.xyz/publication-images/eYgwZpoYmOG2OS0MCW7_H.png" alt=""><figcaption></figcaption></figure>

#### F. Depósito de GNO usando el launchpad: <a href="#heading-f-deposito-de-gno-usando-el-launchpad" id="heading-f-deposito-de-gno-usando-el-launchpad"></a>

24\. Luego vamos a: [https://deposit.gnosischain.com/](https://deposit.gnosischain.com/) y conectamos la wallet que proporcionaste en Gnosis Wagyu Key gen como ETH Withdrawall

<figure><img src="https://images.mirror-media.xyz/publication-images/uVYjY2U4T467g4VrNCqzh.png" alt=""><figcaption></figcaption></figure>

25\. Vas a la pestaña "Deposit".

26\. Subes el archivo `deposit_data*.json` que generaste con Gnosis Wagyu Key gen

27\. Haces clic en `deposit` (Vas a necesitar un poco de Xdai para la transaccion)

<figure><img src="https://images.mirror-media.xyz/publication-images/C2lhV98S0ccN2ZK3XV8j9.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://images.mirror-media.xyz/publication-images/_W2lmE_yLR5rajqFe8wP1.png" alt=""><figcaption></figcaption></figure>

28\. Luego de hacer clic en “Deposit” se abre nuestra wallet y realizas la transacción.

<figure><img src="https://images.mirror-media.xyz/publication-images/-GdK1yzA0Rmr6rDMCmvLU.png" alt=""><figcaption></figcaption></figure>

#### G. Cómo ver a nuestro validador en la red: <a href="#heading-g-como-ver-a-nuestro-validador-en-la-red" id="heading-g-como-ver-a-nuestro-validador-en-la-red"></a>

Para ver el proceso se debe copiar la Public Key que vemos en brain.web3signer-gnosis.dappnode/ y la pegas en el buscador de [https://gnosis.beaconcha.in/](https://gnosis.beaconcha.in/)

<figure><img src="https://images.mirror-media.xyz/publication-images/E4WlZpPcNVxYq3_KKf0-d.png" alt=""><figcaption></figcaption></figure>

Podemos visualizar 3 procesos: Deposited - Pending - Active.

<figure><img src="https://images.mirror-media.xyz/publication-images/yiOh7y_lhmSAejsnTfRT9.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://images.mirror-media.xyz/publication-images/q78d3TCvYY82Dx903FJoX.png" alt=""><figcaption></figcaption></figure>

Una vez en estado Active, nuestro validador ya es parte de la red. A partir de este punto somos un validador mas en la red Gnosis.

#### H. **Withdrawal de nuestro validador**: <a href="#heading-h-withdrawal-de-nuestro-validador" id="heading-h-withdrawal-de-nuestro-validador"></a>

Para ejecutar el retiro del depósito (lo que te lleva a dejar de ser un validador activo de la red) el procedimiento es el siguiente.

Dentro de **Packages - Web3signer Gnosis - Ui**

[brain.web3signer-gnosis.dappnodebrain.web3signer-gnosis.dappnode](http://brain.web3signer-gnosis.dappnode/)

Seleccionamos Exit donde indica la flecha

<figure><img src="https://images.mirror-media.xyz/publication-images/-FM-uau1ZomdFshC4iDd_.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://images.mirror-media.xyz/publication-images/6zjE465zWZoOsWFe83Mec.png" alt=""><figcaption></figcaption></figure>

Seguimos las instrucciones y comenzamos el proceso de salida. Al igual que cuando creamos el validador podemos ver el avance de esta salida voluntaria mediante: [https://gnosis.beaconcha.in/](https://gnosis.beaconcha.in/)

## Referencias: <a href="#heading-referencias" id="heading-referencias"></a>

[Script installation | DappnodeYou can install Dappnode using the installation script. In this case, we recommend you use Ubuntu or Debian as your operating sy…docs.dappnode.io](https://docs.dappnode.io/docs/user/install/script/)

[Gnosis Chain | Gnosis ChainGnosis Chain is one of the first Ethereum sidechains and has stayed true to its values.docs.gnosischain.com](https://docs.gnosischain.com/)

\
\
