# StakeWise Vault

{% embed url="https://mirror.xyz/seedlatam.eth/LX-oDWf9xPUTXKXzqz23PJGE2zNUWaIsUQmsuh3GTFw" %}

### Guía paso a paso utilizando DappNode <a href="#heading-guia-paso-a-paso-utilizando-dappnode" id="heading-guia-paso-a-paso-utilizando-dappnode"></a>

Luego de montar un Validador de Gnosis, surge la curiosidad de seguir indagando dentro del ecosistema. Es entonces cuando nos encontramos con StakeWise:

{% hint style="info" %}
`StakeWise es un protocolo de staking líquido que permite realizar staking de GNO de forma sencilla y segura para personas y organizaciones.`&#x20;

`Además proporciona las herramientas y el software necesarios para que cualquier usuario con su propio hardware ( o algún servicio VPS ) pueda crear su propio pool de staking (llamado Vault) y obtener comisiones ejecutando validadores cuyo GNO fue depositado por otros usuarios.`
{% endhint %}

Ahora bien, teniendo en cuenta lo anterior, armemos un _**Vault**_ de Stakewise utilizando _**Dappnode**_.

La guía esta compuesta por pasos simples y sin dejar huecos de implementación entre ellos.

* **Primera parte - StakeWise dApp -**
  1. Comenzar el procedimiento de creación del vault
  2. Configurar y crear el vault
  3. Dashboard del vault
* **Segunda parte - DappNode -**
  1. Nos conectamos al DappNode
  2. Instalamos el cliente de StakeWise
  3. Configuramos el cliente de StakeWise
  4. Descargamos el backup
* **Tercera parte - StakeWise dApp -**
  * Configuramos el Vault
* **Últimos pasos**
* **SeedNode Vault**

### Primera parte - StakeWise dApp - <a href="#heading-primera-parte-stakewise-dapp" id="heading-primera-parte-stakewise-dapp"></a>

#### 1. Comenzar el procedimiento de creación del Vault: <a href="#heading-1-comenzar-el-procedimiento-de-creacion-del-vault" id="heading-1-comenzar-el-procedimiento-de-creacion-del-vault"></a>

1. Ingresamos a  [https://app.stakewise.io/](https://app.stakewise.io/)

<figure><img src="https://images.mirror-media.xyz/publication-images/kbY2CpDDb2cd2S34wCoXr.png" alt=""><figcaption></figcaption></figure>

2\. Pasamos a _**Connect Wallet**_ - elegimos la que tengamos instalada. En la imagen del ejemplo usamos Rabby

<figure><img src="https://images.mirror-media.xyz/publication-images/bWbOF6fLYPTUqfsc2ksR7.png" alt=""><figcaption></figcaption></figure>

3\. Luego a _**Operate**_

<figure><img src="https://images.mirror-media.xyz/publication-images/4dnuF2TnXRb4Qlo3WPePG.png" alt=""><figcaption></figcaption></figure>

4\. Y luego, vamos a _**Create Vault**_ - ¡Ya estamos listos para empezar a configurar nuestro _**Vault**_!

#### 2. Configuración del Vault <a href="#heading-2-configuracion-del-vault" id="heading-2-configuracion-del-vault"></a>

_**Step 1 -**_** Configurar los parámetros del Vault**

<figure><img src="https://images.mirror-media.xyz/publication-images/SX_hrRdm7gBA91M3f4W3j.png" alt=""><figcaption></figcaption></figure>

* **Capacidad de la bóveda (GNO):** Hay que tener en cuenta que el _**Vault**_ dejará de aceptar nuevos depósitos tan pronto como su saldo alcance este valor.

💡 `En general podemos dejar esta configuración en infinito [como viene seteado por default].`

* **Tarifa de bóveda:** Es la comisión sobre los rewards que se les cobrará a los usuarios que hagan staking en el _**Vault**_.

💡 `Para referencia de las tarifas del otros Vaults:`

[SEEDNode | StakeWiseGnosis v3 Vault 🦉 At SEEDNode, we promote and strengthen decentralization globally by providing best-in-class node infrastruct…app.stakewise.io](https://app.stakewise.io/vaults)

* **Habilitar token de bóveda:** Tu _**Vault**_ tendrá un token ERC-20. Los participantes pueden agregar este token a sus billeteras y transferirlo.
* **Hacer que la bóveda sea privada:** Haz que _**Vault**_ sea privado si no quieres depósitos externos, excepto los de las billeteras que apruebes. Solo las billeteras aprobadas pueden depositar GNO en tu Vault.
*   **Agregar lista de bloqueos:** Agregue una lista de bloqueo para evitar que determinadas direcciones realicen depósitos en su bóveda. Solo las direcciones que no estén en la lista podrán realizar depósitos.

    _**2. Step 2:**_ **Decidir que hacer con los rewards.** Las opciones son enviar las recompensas al Smoothing Pool o si mantenerlas en nuestro Vault.

    ⚠️ `"Smoothing Pool" recolecta las recompensas de propuestas de bloques de los validadores participantes (Vaults) y las distribuye periódicamente de forma proporcional. Esto permite que incluso los Vaults más pequeños reciban pagos regulares. A cambio, cuando un Vault obtiene una recompensa de bloque, también se comparte con los demás. El beneficio principal es mantener un nivel constante de recompensas y un APY estable.`

    `"Escrow" permite a un Vault obtener recompensas de bloques de forma independiente, usando cualquier relay de su elección, sin compartir recompensas con otros Vaults. Esto le da libertad para construir sus propios bloques. Sin embargo, los Vaults con pocos validadores pueden experimentar un APY más volátil debido a la aleatoriedad en la asignación de bloques.`

<figure><img src="https://images.mirror-media.xyz/publication-images/GxOuygIIRApv-G56_0LMq.png" alt=""><figcaption></figcaption></figure>

**3. Step 3: **_**Add Branding**_** -** vas a agregar un nombre, descripción y un logo a tu Vault para distinguirlo.

<figure><img src="https://images.mirror-media.xyz/publication-images/56mGlO8mF26as_lOmJZsf.png" alt=""><figcaption></figcaption></figure>

4\.  **Step 4:** **Resumen de los parámetros del Vault** Vas a ver un resumen de todos los parámetros configurados. Si estás de acuerdo con cada parámetro, se continúa con el proceso.

📌 `Hay que tener algo de xDAI y GNO en la wallet con la que estemos creando el Vault`

<figure><img src="https://images.mirror-media.xyz/publication-images/LtscPN7ZuZK_M1ilMo-PG.png" alt=""><figcaption></figcaption></figure>

¡Listo! Todo en condiciones para crear el _**Vault**_!

<figure><img src="https://images.mirror-media.xyz/publication-images/FB3P_PRKZ2eNR7Rl9qw1G.png" alt=""><figcaption></figcaption></figure>

#### 4. Dashboard del Vault creado recientemente <a href="#heading-4-dashboard-del-vault-creado-recientemente" id="heading-4-dashboard-del-vault-creado-recientemente"></a>

Dentro del dashboard del _**Vault**_ copiamos _**Contract address**_, ya que la vamos a necesitar en _**DappNode**_.

<figure><img src="https://images.mirror-media.xyz/publication-images/gB6kV-aUbUH6CLUX9J9FM.png" alt=""><figcaption></figcaption></figure>

### Segunda parte - DappNode - <a href="#heading-segunda-parte-dappnode" id="heading-segunda-parte-dappnode"></a>

📌 `Les recordamos nuestra guía anterior:`

**1. Vas a conectarte a DappNode** \[ [https://welcome.dappnode.io/](https://welcome.dappnode.io/) ]. En el caso de nuestra guía, se va a hacerlo mediante VPN. Una vez asegurada la conexión por VPN, desde el navegador ingresa a: [http://my.dappnode/stakers/gnosis](http://my.dappnode/stakers/gnosis)

La configuración inicial debería verse así:

<figure><img src="https://images.mirror-media.xyz/publication-images/cn2LCzrT86uX4Qb02EP2K.png" alt=""><figcaption></figcaption></figure>

2\. Ahora, vas a dirigirte al DAppStore:

<figure><img src="https://images.mirror-media.xyz/publication-images/V3cwVOpF2W8AXMg6KqDky.png" alt=""><figcaption></figcaption></figure>

3\. Una vez dentro, clickeamos en _**Install**_

<figure><img src="https://images.mirror-media.xyz/publication-images/KddOqXmKbLgrwcqxk2ToP.png" alt=""><figcaption></figcaption></figure>

4\. Se carga la dirección de contrato como muestra la imagen y el numero de validadores a correr:

`📌 La cantidad de validadores es la cantidad de validadores máxima que la app de StakeWise va a poder utilizar. Los depósitos de GNO en nuestro Vault no podrán superar la cantidad de validadores máximo configurados en este punto.`

<figure><img src="https://images.mirror-media.xyz/publication-images/ycJs2Sms426agEihQUaOj.png" alt=""><figcaption></figcaption></figure>

*   `🫂 El número de validadores que se cargue en DappNode es recomendable que sea mayor a 10 o 20! ¿Por qué? A medida que nos stakeen GNO, StakeWise automáticamente va a crear nuevos validadores pero solo hasta el numero que hayamos dejado seteado. Si este numero se supera, deberemos modificarlo. Esto requiere descargar el backup nuevamente y actualizar la configuracion de nuestro Vault de Stakewise con el nuevo deposit_data_new.json.`

    5\. Se espera a que termine de instalar:

<figure><img src="https://images.mirror-media.xyz/publication-images/gWaqOJ3c7QBa-LmIM1XHe.png" alt=""><figcaption></figcaption></figure>

Una vez instalado, desde _Packages_, debes ver lo siguiente:

<figure><img src="https://images.mirror-media.xyz/publication-images/vJeR4OyUVm0oalbBTCKNq.png" alt=""><figcaption></figcaption></figure>

* En lo marcado con el numero 1 \[color verde] podemos ver la wallet que necesitamos fondear con **al menos 0.1 xDAI**.
* En lo marcado con el numero 2 \[color magenta] es donde se encuentra el backup que se debe descargar a continuación.

Descarga de los archivos de backup

Click en **Backup Now**

<figure><img src="https://images.mirror-media.xyz/publication-images/e-lVhb9b9yLLG-6QZN-Df.png" alt=""><figcaption></figcaption></figure>

*   Se va a descargar un archivo .tar.xz. Una vez descomprimido, se busca el archivo deposit\_data.json en la siguiente carpeta /operator-config/stakewise//deposit\_data.json

    ⚠️ `Este archivo hay que resguardarlo bien.`

<figure><img src="https://images.mirror-media.xyz/publication-images/rtG25SYOnQ922JufvOwQ8.png" alt="" height="298" width="596"><figcaption></figcaption></figure>

### Tercera parte - StakeWise dApp - : <a href="#heading-tercera-parte-stakewise-dapp" id="heading-tercera-parte-stakewise-dapp"></a>

1. Ingresas a “_**Configure**_”

<figure><img src="https://images.mirror-media.xyz/publication-images/ElGqfFzUAYwFqkSaFCLWF.png" alt=""><figcaption></figcaption></figure>

Step 1: Se selecciona _**Dappnode**_

<figure><img src="https://images.mirror-media.xyz/publication-images/enXJH-BMJ94dSItQBPvAe.png" alt=""><figcaption></figcaption></figure>

Step 2: A subir el archivo _**deposit\_data.json**_ y _**Guardamos**_

<figure><img src="https://images.mirror-media.xyz/publication-images/62MNeX0mP2Tm_ghPn6wxy.png" alt=""><figcaption></figcaption></figure>

_**Últimos pasos**_

Finalizado todo este proceso, el _**Vault**_ nos quedaría así:

<figure><img src="https://images.mirror-media.xyz/publication-images/7FlLoxhIz3qJcrjkoPLb-.png" alt=""><figcaption></figcaption></figure>

Para finalizar correctamente, ten en cuenta y contáctate mediante mail a [info@stakewise.io](http://mailto:info@stakewise.io/) o en su Discord [https://discord.gg/stakewise](https://discord.gg/stakewise) para verificar el Vault.

<figure><img src="https://images.mirror-media.xyz/publication-images/epPXbQvyTFh_kciCQK99I.png" alt=""><figcaption></figcaption></figure>

Y a medida que nos vayan stakeando, los Actions y Validators se van a empezar a visualizar así:

<figure><img src="https://images.mirror-media.xyz/publication-images/A9o6ALBZxFxni__DEnCST.png" alt=""><figcaption></figcaption></figure>

### SeedNode Vault <a href="#heading-seednode-vault" id="heading-seednode-vault"></a>

Invitamos a todos los miembros de la comunidad que quiera participar en nuestro _**Vault**_a visitar el siguiente Link:

[SEEDNode | StakeWiseGnosis v3 Vault 🦉 At SEEDNode, we promote and strengthen decentralization globally by providing best-in-class node infrastruct…app.stakewise.io](https://app.stakewise.io/vault/gnosis/0x9eeb6be79899cfe45018866a2113c6b77fa96f35)

Los esperamos!
