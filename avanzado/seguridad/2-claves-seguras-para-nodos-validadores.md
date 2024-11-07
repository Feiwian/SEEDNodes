# #2 Claves seguras para nodos validadores

{% embed url="https://mirror.xyz/seedlatam.eth/NE-iXSkA4_PCZpBWW_r4EhmTxXjilCytX3IxQEKi4KQ" %}

## **Segunda parte:** <a href="#heading-segunda-parte" id="heading-segunda-parte"></a>

En esta segunda parte, exploraremos el procedimiento práctico para crear claves seguras para un validador de Ethereum. Es recomendable leer la [primera parte](https://mirror.xyz/seedlatam.eth/alffxS1TAN1un8Nt0n7zgarc1nvi01KDvz\_80pEXTFQ) antes de continuar, dado que brinda información esencial sobre las distintas clases de claves y su relevancia.

La guía se divide en las siguientes partes:

1. Descarga del cliente [staking-deposit-cli](https://github.com/ethereum/staking-deposit-cli)
2. Creación de una Unidad USB con Ubuntu Live
3. Bootear desde el USB-Live
4. Consideraciones acerca de la dirección de retiro
5. Consideraciones acerca de la frase mnemónica del validator key
6. Generación de la frase mnemónica y de los archivos del keystore.
7. Verificación de la frase mnemónica
8. Resultados

El objetivo principal de esta guía es generar claves seguras para un validador de Ethereum sin necesidad de conexión a internet en el dispositivo que se utiliza para manipular la frase mnemónica.

Utilizaremos el sistema operativo Ubuntu Live en un pendrive para ejecutar el cliente staking-deposit-cli y crear el keystore BLS12–381 y su archivo deposit\_data\*.json necesario para el Ethereum Staking Launchpad.

***

#### **1. Descarga del cliente** [**staking-deposit-cli**](https://github.com/ethereum/staking-deposit-cli) <a href="#heading-1-descarga-del-clientestaking-deposit-cli" id="heading-1-descarga-del-clientestaking-deposit-cli"></a>

Ingresamos a: [https://github.com/ethereum/staking-deposit-cli/releases](https://github.com/ethereum/staking-deposit-cli/releases)

Seleccionamos la versión que finaliza con \***linux-amd64.tar.gz** ya que es la necesaria para el SO que vamos a utilizar.

<figure><img src="https://images.mirror-media.xyz/publication-images/HVeKoFl-2XHs9OVHEKoei.png" alt=""><figcaption></figcaption></figure>

Copiamos el archivo staking\_deposit-cli-fdab65d-linux-amd64.tar en un pendrive limpio.

#### **2. Creación de una Unidad USB con Ubuntu Live: Guía paso a paso** <a href="#heading-2-creacion-de-una-unidad-usb-con-ubuntu-live-guia-paso-a-paso" id="heading-2-creacion-de-una-unidad-usb-con-ubuntu-live-guia-paso-a-paso"></a>

1\. Descarga de la imagen ISO de Ubuntu:

Ingresamos a: [https://www.releases.ubuntu.com/](https://www.releases.ubuntu.com/) Descargamos la ultima versión LTS

<figure><img src="https://images.mirror-media.xyz/publication-images/pGPGAbHKw1At4Jq0Nue9B.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://images.mirror-media.xyz/publication-images/5kkilBTYdK-2065Zy0lqt.png" alt=""><figcaption></figcaption></figure>

2\. Creamos el USB Live

Este paso depende del SO operativo base de cada usuario. En este caso utilizaremos Rufus desde Windows. Para descargar Rufus ingresamos en: [https://rufus.ie/es/](https://rufus.ie/es/)

<figure><img src="https://images.mirror-media.xyz/publication-images/cJikQD7qoQR3oHUXZm15L.png" alt=""><figcaption></figcaption></figure>

Insertamos un pendrive limpio (uno diferente al que le descargamos el cliente de staking).

Ejecutamos Rufus y cargamos la imagen ISO que descargamos de Ubuntu y sin cambiar nada de la configuración le damos a Empezar.

<figure><img src="https://images.mirror-media.xyz/publication-images/Cv0PsNXVWMbwNyoNp-Cu8.png" alt=""><figcaption></figcaption></figure>

De esta forma ya tenemos Ubuntu listo para usar desde el pendrive.

#### **3. Booteamos desde el USB-Live** <a href="#heading-3-booteamos-desde-el-usb-live" id="heading-3-booteamos-desde-el-usb-live"></a>

Este procedimiento dependerá del modelo de la placa madre y/o BIOS de cada usuario. Una guía general es la siguiente:

* Antes que nada, asegúrate de que tu computadora esté apagada y desconectada de internet (apaga el WIFI o desconecta el cable de red).
* Conecta el USB-Live.
* Enciende la computadora y comienza a presionar repetidamente la tecla F12 (o F10). Esto te permitirá acceder al menú de inicio.
* Desde el menú de inicio único, elige la opción que corresponde al USB-Live.
* Si lo anterior no funciona, es probable que necesites acceder al BIOS utilizando la tecla F2 o DEL. Desde allí, podrás configurar el inicio para que arranque desde el USB.
* Si nada de esto funciona, deberás buscar información sobre cómo acceder al menú de inicio único específico para el modelo de tu computadora.
* Una vez que hayas seleccionado el USB de Ubuntu Live, si no hay ningún error, verás aparecer el menú GRUB. En algunos casos, el sistema continuará automáticamente. Sin embargo, si no lo hace, simplemente selecciona la primera opción del menú y presiona ENTER.

<figure><img src="https://images.mirror-media.xyz/publication-images/VolhbBprFgky5dOXoWaOM.png" alt=""><figcaption></figcaption></figure>

* En la pantalla de instalación presionar Try Ubuntu:

<figure><img src="https://images.mirror-media.xyz/publication-images/_xA9mTFPTNt8ZQHFer_is.png" alt=""><figcaption></figcaption></figure>

* Una vez iniciado Ubuntu, conectamos el pendrive con el cliente. Descomprimimos el archivo (click derecho extraer)

<figure><img src="https://images.mirror-media.xyz/publication-images/Kz41wl9VJlSlXjschtiVK.png" alt=""><figcaption></figcaption></figure>

movemos la carpeta al escritorio y por comodidad le cambiamos el nombre a la carpeta a uno mas corto, en la guía usamos “deposit-cli”.

#### **4. Consideraciones acerca de la dirección de retiro** <a href="#heading-4-consideraciones-acerca-de-la-direccion-de-retiro" id="heading-4-consideraciones-acerca-de-la-direccion-de-retiro"></a>

Vamos a necesitar en los siguientes pasos una cuenta de Ethereum segura. En esta guía no se realizará un procedimiento sobre cómo hacerlo.

Simplemente mencionaremos la importancia de que esta cuenta tenga altísimos estándares de seguridad, como los proporcionados por una billetera de hardware (HW) o una multisig. Para más detalles sobre la importancia de esta dirección, recomiendo leer la parte 1.

Tomamos como dirección de retiro para los fines de esta guía:0xf2Bb4caBAB44d50Ed9f53bF09a2504A1BDda60f2

#### **5. Consideraciones acerca de la frase mnemónica para las claves del validador** <a href="#heading-5-consideraciones-acerca-de-la-frase-mnemonica-para-las-claves-del-validador" id="heading-5-consideraciones-acerca-de-la-frase-mnemonica-para-las-claves-del-validador"></a>

Para obtener las 24 palabras para las claves del validador, podemos usar la herramienta **offline** generadora de frases BIP39 que deseemos. Algunas alternativas son:

* El mismo cliente que vamos a usar para crear los archivos del keystore.
* Una billetera de hardware (HW).
* Generar nuestra propia entropía.

Para el desarrollo de esta guía, usaremos el cliente, pero queremos dejar claro que podemos utilizar cualquier frase BIP39.

Las normas de seguridad generales para guardar la frase mnemónica son las mismas que para cualquier otra billetera web3. Básicamente, se pueden resumir en dos consejos:

* No almacenar la frase mnemónica en un dispositivo con conexión a internet.
* Idealmente, resguardarla escrita en un medio físico, preferentemente metal. Hay muchos tutoriales sobre cómo hacer esto, incluso herramientas que nos permiten hacerlo de forma casi profesional. Por ejemplo, blockmit:[https://blockmit.com/english/guides/diy/make-cold-wallet-washers/](https://blockmit.com/english/guides/diy/make-cold-wallet-washers/)

#### **6. Generación de la frase mnemónica y de los archivos del keystore.** <a href="#heading-6-generacion-de-la-frase-mnemonica-y-de-los-archivos-del-keystore" id="heading-6-generacion-de-la-frase-mnemonica-y-de-los-archivos-del-keystore"></a>

* Verificamos que no haya conexión a internet.
*   Abrimos una terminal y navegamos a la carpeta del cliente (esto dependerá de la ubicación donde guardamos la carpeta; aquí se presenta un ejemplo).

    ```
    cd deposit-cli
    ```
*   Creamos una nueva frase mnemónica utilizando el comando “new-mnemonic” del cliente. Mantendremos los argumentos al mínimo y permitiremos que la aplicación nos guíe. El único argumento necesario es “-execution\_address”, ya que si no lo agregamos, el cliente no nos lo solicitará y dejará la credencial “withdrawal address” en “0x00” en el archivo “deposit-data”. Como se mencionó en el artículo anterior, esto no es una práctica recomendada. La dirección que sigue al argumento “-execution\_address” es la dirección de retiro.

    ```
    ./deposit new-mnemonic -execution_address 0xf2Bb4caBAB44d50Ed9f53bF09a2504A1BDda60f2
    ```

<figure><img src="https://images.mirror-media.xyz/publication-images/Lhu09CE95GCuSlejWDBEY.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://images.mirror-media.xyz/publication-images/JbPkPQOUWIyn99kapWaeM.png" alt=""><figcaption></figcaption></figure>

* Elegimos el idioma del menú. Para ingles ingresamos 3.
* Nos pide re-confirmar la dirección de retiro. Ingresamos nuevamente la misma dirección de retiro.

<figure><img src="https://images.mirror-media.xyz/publication-images/jizyhWDc_UD-GD3K9DnwJ.png" alt=""><figcaption></figcaption></figure>

* Luego nos pide el idioma que deseamos para la frase mnemónica. Para ingles ingresamos 4.

<figure><img src="https://images.mirror-media.xyz/publication-images/lP2bnD3TUfldupnqTySt2.png" alt=""><figcaption></figcaption></figure>

* Como se explico en la parte 1 con una sola frase mnemónica podemos manejar múltiples validadores. Para un único validador presionamos 1.

<figure><img src="https://images.mirror-media.xyz/publication-images/IwN210hTmz9YgA1fiVvRX.png" alt=""><figcaption></figcaption></figure>

* Elegimos que red vamos a usar, si mainnet o una de las testnet. Escribimos “mainnet”.
* Luego nos pide la contraseña con la que vamos a cifrar el archivo del signing-key. Se recomienda una contraseña fuerte. En esta pagina hay ejemplos que pueden servir de inspiración para armar una contraseña segura.[https://xkcd.com/936/](https://xkcd.com/936/)

<figure><img src="https://images.mirror-media.xyz/publication-images/Yy6NhZGMgwH1Kk2KanZiX.png" alt=""><figcaption></figcaption></figure>

* Finalmente nos entrega una frase mnemónica. Copiar la frase en una hoja.

<figure><img src="https://images.mirror-media.xyz/publication-images/SSRI5-aDu3VcqYqlDtd7v.png" alt=""><figcaption></figcaption></figure>

* Nos va a pedir verificar ingresando todas las palabras nuevamente. En caso de que todo sea correcto ha terminado el procedimiento.

<figure><img src="https://images.mirror-media.xyz/publication-images/pU3wf3dUKLKHMB0iM3rUe.png" alt=""><figcaption></figcaption></figure>

Una vez finalizado el proceso se debieron crear los archivo JSON en la misma carpeta donde esta el cliente. Como se menciono en la parte 1 el archivo deposit\_data-\*.JSON tiene información publica y el keystore-\*.JSON tiene el signing key encriptado con la contraseña que elegimos.

#### **7. Verificación de la frase mnemónica** <a href="#heading-7-verificacion-de-la-frase-mnemonica" id="heading-7-verificacion-de-la-frase-mnemonica"></a>

Nunca está de más realizar una nueva verificación. Además, este procedimiento también sirve si necesitan recuperar los archivos a partir de una frase mnemónica guardada.

1. Verificamos que no haya conexión a internet.
2.  Abrimos una terminal y nos dirigimos a la carpeta del cliente (la ubicación puede variar dependiendo de dónde guardamos la carpeta; este es solo un ejemplo).

    ```
    cd deposit-cli
    ```
3. Creamos nuestro nuevo keystore utilizando la frase mnemónica obtenida en el paso anterior. Para ello, utilizaremos el comando “existing-mnemonic” del cliente:

<figure><img src="https://images.mirror-media.xyz/publication-images/KB0t44PhFfJ2r8kdxjCl1.png" alt=""><figcaption></figcaption></figure>

```
./deposit existing-mnemonic  -execution_address 0xf2Bb4caBAB44d50Ed9f53bF09a2504A1BDda60f2
```

* Elegimos el idioma del menú. Para inglés, ingresamos 3.
* Nos pide confirmar nuevamente la dirección de retiro. Ingresamos la misma dirección de retiro.
* A continuación, nos solicita seleccionar el idioma para la frase mnemónica. Para inglés, ingresamos 4.
* Ingresamos la frase mnemónica.
* Luego, nos solicita ingresar el index (key number). En nuestro caso, es cero.

<figure><img src="https://images.mirror-media.xyz/publication-images/ZjAwoaLzyH7RYyChumMdn.png" alt=""><figcaption></figcaption></figure>

> _En el punto anterior, al utilizar el cliente para crear un mnemónico nuevo y clave para un único validador, el índice que se utilizó fue 0. Si hubiéramos generado 2 claves, se habrían utilizado los índices 0 y 1. Si hubiéramos generado 3 claves, se habrían generado los índices 0, 1 y 2. Sabiendo esto, si quisiéramos generar una nueva clave utilizando la misma frase mnemónica, debemos comenzar en el índice 3. Si se comienza en cualquier índice más alto, seguirá funcionando, pero podría resultar confuso si no se recuerda qué índice se utilizó en el pasado._

* Nos pide cuántos validadores van a ser. Para un único validador, ingresamos 1.

<figure><img src="https://images.mirror-media.xyz/publication-images/p_dQh14ESWDvH_Qzj6Gzb.png" alt=""><figcaption></figcaption></figure>

* Elegimos que red vamos a usar, si mainnet o una de las testnet. Escribimos “mainnet”.
*   Luego nos pide la contraseña con la que vamos a cifrar el archivo del signing-key.

    En caso de que todo sea correcto ya hemos generado el segundo par de archivos.

<figure><img src="https://images.mirror-media.xyz/publication-images/tP1mgom0HICE9nh71ZRkV.png" alt=""><figcaption></figcaption></figure>

* El último y más importante paso es verificar que los archivos .JSON creados en los pasos **V** y **VI** correspondan a las mismas claves. Ingresamos a la carpeta “validator\_keys” que se creó dentro de la carpeta del cliente. Abrimos ambos archivos deposit-data y verificamos el “pubkey”.

<figure><img src="https://images.mirror-media.xyz/publication-images/IEN8h004YMUvSRVMBeWVE.png" alt=""><figcaption></figcaption></figure>

Vamos a verificar que son idénticos. También podemos observar que “withdrawal\_credentials” comienza con “01” y que nuestra dirección de retiro está insertada en los 32 bytes de este campo.

Por último, comparamos los archivos “keystore-m\*.json” y nuevamente comparamos los “pubkey”.

<figure><img src="https://images.mirror-media.xyz/publication-images/lq6gwkf8eeC0EKFsSdUg-.png" alt=""><figcaption></figcaption></figure>

> _Es normal que los archivos keystore-m\*.json sean diferentes. Lo importante es verificar el pubkey._

Una vez comprobados que la frase mnemónica anotada es la correcta, ya podemos guardar la carpeta contenedora de los archivos “deposito-data” y “keystore-m” en nuestro pendrive para luego utilizarla en el validador.

El USB live no realiza ninguna modificación en sus archivos. Pueden verificarlo explorando el pendrive una vez reiniciada la computadora.

#### **8. Resultados** <a href="#heading-8-resultados" id="heading-8-resultados"></a>

Como dijimos, el objetivo de la guía fue crear el keystore de forma segura y offline. Por lo tanto, al finalizarla, lo que debemos tener en nuestro poder es:

* Una frase mnemónica del validador escrita en un medio físico.
* El control absoluto de una dirección de retiro.
* Un pendrive con el validator key.

Tener un backup del validator key en un pendrive, u otro medio físico sin conexión a internet, es una decisión personal. No es esencial, ya que como realizamos en la guía, este se puede recuperar desde la frase mnemónica.

Como comentario final, el archivo keystore-m\*.json podrá ser usado directamente en el validador (como hot wallet) o mediante servicios como Web3Signer para disminuir el riesgo de slashing (aunque aumentando la dificultad técnica).

***

Disclaimer: _El staking conlleva sus propios riesgos, además de los inherentes a la generación y gestión de claves. El objetivo de esta guía es únicamente educativo!_

Abordar la seguridad en profundidad requiere un análisis extenso debido a sus diversas facetas. Estos artículos, tanto la [parte 1](https://mirror.xyz/seedlatam.eth/alffxS1TAN1un8Nt0n7zgarc1nvi01KDvz\_80pEXTFQ) como la parte 2 no tienen la intención de ser una guía definitiva ni establecer un único enfoque para abordar la seguridad. Su objetivo principal es exponer el problema de la gestión de claves para validadores de Ethereum y ofrecer una alternativa segura para afrontarlo.

Es importante recordar que la seguridad es un proceso continuo que requiere atención constante.
