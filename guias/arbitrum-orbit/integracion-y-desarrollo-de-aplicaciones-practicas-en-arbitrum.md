# Integración y desarrollo de aplicaciones prácticas en Arbitrum

**Objetivo:** Proporcionar conocimientos integrales sobre aplicaciones prácticas e integraciones dentro del ecosistema Arbitrum, preparando a los participantes para escenarios de desarrollo en el mundo real.

### **Integración con Arbitrum Bridge** <a href="#block-ead2961b7ffa43fda345025781f08cba" id="block-ead2961b7ffa43fda345025781f08cba"></a>

* Conexión de una cadena local Orbit al puente Arbitrum.

<details>

<summary>Integración con Arbitrum Bridge</summary>

#### Prerrequisitos <a href="#block-94d9e20297eb462ab6e6f9fd96458d62" id="block-94d9e20297eb462ab6e6f9fd96458d62"></a>

**Configuración de una Cadena Local Orbit:**

* Si aún no has configurado una cadena local Orbit, sigue la [guía rápida de Orbit](https://docs.arbitrum.io/launch-orbit-chain/orbit-quickstart).

**Instalar una Billetera Ethereum basada en navegador (como MetaMask):**

* Si no tienes MetaMask, puedes descargarla e instalarla desde [metamask.io](https://metamask.io/).

#### Procedimiento <a href="#block-1f8d9ba2a7fe4fee8bd55d53496b9361" id="block-1f8d9ba2a7fe4fee8bd55d53496b9361"></a>

#### 1. Preparación Inicial <a href="#block-c8e73a1e70e84f51b0bece4b403c79be" id="block-c8e73a1e70e84f51b0bece4b403c79be"></a>

**Abrir Terminal:** Inicia la terminal en tu sistema operativo.

**Configurar el Nodo Orbit:**

* Asegúrate de que tu nodo Orbit esté funcionando correctamente siguiendo los pasos previos de configuración y despliegue.

#### 2. Agregar la Cadena Local Orbit al Puente Arbitrum <a href="#block-be7c464ce719476f8d6ca377ae253e7b" id="block-be7c464ce719476f8d6ca377ae253e7b"></a>

**Navegar a la Interfaz del Puente Arbitrum:**

* Abre tu navegador web y navega a [https://bridge.arbitrum.io/](https://bridge.arbitrum.io/).

**Conectar tu Billetera:**

* Conéctate a la interfaz del puente usando tu billetera Ethereum (como MetaMask). La interfaz del puente cambiará automáticamente a la vista del testnet correcto.

**Activar el Modo Testnet:**

* Haz clic en tu dirección en la esquina superior derecha.
* Ve a "Configuración" y activa el modo testnet haciendo clic en "Turn on testnet mode".

**Agregar la Cadena Testnet Orbit:**

* En la misma pantalla de configuración, desplázate hacia abajo hasta "Add Testnet Orbit Chain".

**Copiar y Pegar la Configuración JSON:**

* Utiliza la configuración en tu archivo `outputInfo.json` generado durante el despliegue de tu cadena local Orbit.
* El contenido del archivo `outputInfo.json` se verá algo así:

<!---->

* ```json
  jsonCopiar código
  {
    "chainID": "421611",
    "chainName": "Local Orbit Testnet",
    "rpc": "http://localhost:8545",
    "explorer": "http://localhost:8545/explorer",
    "nativeCurrency": {
      "name": "Ether",
      "symbol": "ETH",
      "decimals": 18
    }
  }

  ```

**Añadir la Cadena:**

* Copia y pega esta configuración en el campo "Add Testnet Orbit Chain".
* Haz clic en "Add Chain".

#### Conclusión <a href="#block-1599dd00fdf34949948484cddc61a253" id="block-1599dd00fdf34949948484cddc61a253"></a>

Estos pasos detallados guiarán a los participantes a través del proceso de configuración y ejecución de un nodo Orbit conectado a un puente Arbitrum, permitiendo la transferencia de activos entre redes de manera eficiente.

</details>

### **Implementación de Tokens de Gas Personalizados** <a href="#block-2e8d0e0cb4c24bd2b0746bfeb998e05e" id="block-2e8d0e0cb4c24bd2b0746bfeb998e05e"></a>

* Creación y despliegue de tokens de gas personalizados.

<details>

<summary>Implementación de Tokens de Gas Personalizados</summary>

Para utilizar un token de gas personalizado en una cadena Orbit, es importante tener una configuración adecuada de la cadena, especialmente si se está utilizando el modelo AnyTrust. A continuación las instrucciones detalladas para configurar una blockchain Orbit con AnyTrust, seguido de la creación y despliegue de un token de gas personalizado.

#### Configuración de una Blockchain Orbit con AnyTrust <a href="#block-0053c0f899e14009a630efc9ce75b9e4" id="block-0053c0f899e14009a630efc9ce75b9e4"></a>

**Objetivo:** Aprender a configurar una blockchain Orbit utilizando el modelo AnyTrust y luego desplegar un token de gas personalizado.

#### Prerrequisitos <a href="#block-b24a256fab9347b98d7b833d9d0264e6" id="block-b24a256fab9347b98d7b833d9d0264e6"></a>

**Instalar Docker y Docker Compose:**

* Asegúrate de tener `Docker` y `Docker Compose` instalados. Si no los tienes, instálalos siguiendo las instrucciones en [docker.com](https://www.docker.com/get-started).

**Configuración de Node.js y npm:**

* Asegúrate de tener `Node.js` y `npm` instalados. Si no los tienes, instálalos con:

<!---->

* ```powershell
  sudo apt install nodejs npm   # Linux
  brew install node             # macOS
  choco install nodejs          # Windows
  ```

#### Procedimiento <a href="#block-e957fde625a4443190152c9de9bf32e9" id="block-e957fde625a4443190152c9de9bf32e9"></a>

#### 1. Configuración de la Blockchain Orbit con AnyTrust <a href="#block-bc256f4ff78b4304b492e73c9e384e92" id="block-bc256f4ff78b4304b492e73c9e384e92"></a>

**Clonar el Repositorio de Orbit:**

* En la terminal, clona el repositorio oficial de Orbit:

<!---->

* ```powershell
  git clone <https://github.com/OffchainLabs/orbit-chain.git>
  cd orbit-chain
  ```

**Crear la Configuración del AnyTrust:**

* Crea un archivo de configuración para el despliegue de AnyTrust:

<!---->

* ```powershell
  touch config.json
  nano config.json
  ```
* Copia y pega el siguiente contenido en `config.json`:

<!---->

* ```json
  {
    "rollup": {
      "chain_id": 421611,
      "base_chain_rpc": "<https://mainnet.infura.io/v3/YOUR_INFURA_PROJECT_ID>",
      "confirmations": 12,
      "anytrust_config": {
        "validators": [
          "0xVALIDATOR_1_ADDRESS",
          "0xVALIDATOR_2_ADDRESS"
        ],
        "threshold": 2
      }
    },
    "sequencer": {
      "enable": true,
      "priv_key": "YOUR_PRIVATE_KEY"
    },
    "staker": {
      "enable": true,
      "priv_key": "YOUR_PRIVATE_KEY"
    },
    "http_port": 8545,
    "ws_port": 8546,
    "db_path": "/var/lib/orbit/db",
    "log_level": "info"
  }
  ```
* Reemplaza `"YOUR_INFURA_PROJECT_ID"`, `"YOUR_PRIVATE_KEY"`, `"0xVALIDATOR_1_ADDRESS"`, y `"0xVALIDATOR_2_ADDRESS"` con los valores apropiados.

**Configurar Docker Compose:**

* Crea un archivo `docker-compose.yml`:

<!---->

* ```powershell
  touch docker-compose.yml
  nano docker-compose.yml
  ```
* Copia y pega el siguiente contenido en `docker-compose.yml`:

<!---->

* ```yaml
  version: '3'
  services:
    orbit:
      image: offchainlabs/orbit-chain
      ports:
        - "8545:8545"
        - "8546:8546"
      volumes:
        - ./data:/var/lib/orbit/db
      command: ["--config", "/config/config.json"]
      environment:
        - CONFIG=/config/config.json
      volumes:
        - ./config.json:/config/config.json
  ```

**Iniciar la Cadena Orbit:**

* En la terminal, ejecuta:

<!---->

* ```powershell
  docker-compose up -d
  ```
* Esto iniciará el contenedor de Docker con la configuración especificada para AnyTrust.

#### 2. Creación y Despliegue de Tokens de Gas Personalizados <a href="#block-75f9bbb1141d43b89b21817d2cde20cb" id="block-75f9bbb1141d43b89b21817d2cde20cb"></a>

**Crear un Proyecto con Hardhat:**

* En la terminal, escribe:

<!---->

* ```powershell
  mkdir my-gas-token
  cd my-gas-token
  npx hardhat
  ```
* Sigue las instrucciones para crear un proyecto básico de Hardhat.

**Instalar Dependencias:**

* Instala las dependencias necesarias para desarrollar y desplegar el contrato ERC-20:

<!---->

* ```powershell
  npm install @openzeppelin/contracts
  ```

**Crear el Contrato ERC-20:**

* En el directorio del proyecto, navega a `contracts` y crea un nuevo archivo llamado `GasToken.sol`:

<!---->

* ```powershell
  touch contracts/GasToken.sol
  nano contracts/GasToken.sol
  ```
* Copia y pega el siguiente código en `GasToken.sol`:

<!---->

* ```solidity
  // SPDX-License-Identifier: MIT
  pragma solidity ^0.8.0;

  import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
  import "@openzeppelin/contracts/access/Ownable.sol";

  contract GasToken is ERC20, Ownable {
      constructor() ERC20("GasToken", "GAS") {
          _mint(msg.sender, 1000000 * 10 ** decimals());
      }

      function mint(address to, uint256 amount) public onlyOwner {
          _mint(to, amount);
      }
  }
  ```

**Compilar el Contrato:**

* En la terminal, ejecuta el siguiente comando para compilar el contrato:

<!---->

* ```powershell
  npx hardhat compile
  ```

**Crear un Script de Despliegue:**

* Navega a la carpeta `scripts` y crea un nuevo archivo llamado `deploy.js`:

<!---->

* ```powershell
  touch scripts/deploy.js
  nano scripts/deploy.js
  ```
* Copia y pega el siguiente código en `deploy.js`:

<!---->

* ```javascript
  async function main() {
      const [deployer] = await ethers.getSigners();

      console.log("Deploying contracts with the account:", deployer.address);

      const GasToken = await ethers.getContractFactory("GasToken");
      const gasToken = await GasToken.deploy();

      console.log("GasToken deployed to:", gasToken.address);
  }

  main()
      .then(() => process.exit(0))
      .catch((error) => {
          console.error(error);
          process.exit(1);
      });
  ```

**Configurar la Red Orbit en Hardhat:**

* Abre el archivo `hardhat.config.js` y configura la red Orbit:

<!---->

* ```javascript
  require("@nomiclabs/hardhat-waffle");

  module.exports = {
    solidity: "0.8.4",
    networks: {
      orbit: {
        url: "<http://localhost:8545>",
        accounts: [`0x${YOUR_PRIVATE_KEY}`]
      }
    }
  };
  ```

**Desplegar el Contrato en la Red Orbit:**

* En la terminal, ejecuta el siguiente comando:

<!---->

* ```powershell
  npx hardhat run scripts/deploy.js --network orbit
  ```
* Toma nota de la dirección del contrato desplegado (`GasToken deployed to: 0x...`).

#### 3. Configurar el Token de Gas Personalizado en Orbit <a href="#block-7608cdd0ca2c4436a8cad79a24ca0347" id="block-7608cdd0ca2c4436a8cad79a24ca0347"></a>

**Editar el Archivo de Configuración del Nodo:**

* Abre el archivo de configuración del nodo (`config.json`) y añade la dirección del token de gas:

<!---->

* ```json
  {
    "network": "orbit",
    "db_path": "/var/lib/orbit/db",
    "http_port": 8545,
    "ws_port": 8546,
    "log_level": "info",
    "rollup": {
      "chain_id": 421611,
      "base_chain_rpc": "<https://mainnet.infura.io/v3/YOUR_INFURA_PROJECT_ID>",
      "confirmations": 12,
      "anytrust_config": {
        "validators": [
          "0xVALIDATOR_1_ADDRESS",
          "0xVALIDATOR_2_ADDRESS"
        ],
        "threshold": 2
      }
    },
    "sequencer": {
      "enable": true,
      "priv_key": "YOUR_PRIVATE_KEY"
    },
    "staker": {
      "enable": true,
      "priv_key": "YOUR_PRIVATE_KEY"
    },
    "gas_token": "0xGAS_TOKEN_ADDRESS"  // Añade la dirección de tu token de gas aquí
  }
  ```

**Reiniciar el Nodo:**

* En la terminal, detén el nodo y reinícialo para aplicar la nueva configuración:

<!---->

* ```powershell
  docker-compose down
  docker-compose up -d
  ```

#### Conclusión <a href="#block-f47f9c09f6834bc9a3189cb97a798233" id="block-f47f9c09f6834bc9a3189cb97a798233"></a>

Estos pasos detallados guiarán a los participantes a través del proceso de configuración de una blockchain Orbit utilizando el modelo AnyTrust, así como la creación y despliegue de un token de gas personalizado. Una vez completado, el nuevo token de gas se puede utilizar para pagar las tarifas de transacción en lugar del token nativo (Ether). Para más detalles y configuraciones avanzadas, consulta la [documentación oficial de Arbitrum](https://docs.arbitrum.io/).

</details>

### **Garantizar la Disponibilidad de Datos** <a href="#block-1fbd0aa1a7c34fea8f6af3f069980aeb" id="block-1fbd0aa1a7c34fea8f6af3f069980aeb"></a>

* Estrategias para mantener la disponibilidad de datos.

<details>

<summary>Garantizar la Disponibilidad de Datos</summary>

### Estrategias para Mantener la Disponibilidad de Datos <a href="#block-c236bc08cd53492496c85e5b602756a7" id="block-c236bc08cd53492496c85e5b602756a7"></a>

**Objetivo:** Aprender diferentes estrategias para asegurar que los datos en una cadena Orbit estén siempre disponibles y accesibles, utilizando soluciones descentralizadas de disponibilidad de datos (Data Availability - DA).

### Importancia de la Disponibilidad de Datos <a href="#block-ac2de5d8c3524190887450f5ba7b22fe" id="block-ac2de5d8c3524190887450f5ba7b22fe"></a>

La disponibilidad de datos es crucial en cualquier red blockchain, ya que garantiza que todos los nodos puedan acceder a la misma información de manera confiable. La falta de disponibilidad de datos puede afectar la validez de las transacciones y la seguridad de la red.

### Estrategias para Mantener la Disponibilidad de Datos <a href="#block-1082a17207e149d6928780d39445311f" id="block-1082a17207e149d6928780d39445311f"></a>

#### 1. Uso de Redes de Disponibilidad de Datos (DANs) <a href="#block-4c912a9178b248d9b3ff3db9718ad992" id="block-4c912a9178b248d9b3ff3db9718ad992"></a>

**Data Availability Committee (DAC):**

1. **Qué es un Data Availability Committee (DAC):**
   1. Un DAC es un grupo de validadores que se encargan de asegurar la disponibilidad de datos de las transacciones en una cadena de bloques.
      * Los DACs actúan como custodios de los datos y garantizan que estos estén accesibles para todos los nodos de la red.
2.  **Implementación de un DAC en Arbitrum Orbit:**



    **Paso 1: Selección de Miembros del DAC:**

    * Identifica y selecciona un conjunto de validadores confiables que actuarán como miembros del DAC.
      * Estos validadores deben ser entidades independientes para garantizar la descentralización y seguridad.

    **Paso 2: Configuración del DAC:**

    * Configura el archivo de configuración de tu cadena Orbit para incluir los detalles del DAC.
    * Aquí tienes un ejemplo de cómo se puede configurar:



    * ```json
      {
        "rollup": {
          "chain_id": 421611,
          "base_chain_rpc": "<https://mainnet.infura.io/v3/YOUR_INFURA_PROJECT_ID>",
          "confirmations": 12,
          "dac_config": {
            "members": [
              "0xVALIDATOR_1_ADDRESS",
              "0xVALIDATOR_2_ADDRESS",
              "0xVALIDATOR_3_ADDRESS"
            ],
            "threshold": 2
          }
        },
        "sequencer": {
          "enable": true,
          "priv_key": "YOUR_PRIVATE_KEY"
        },
        "staker": {
          "enable": true,
          "priv_key": "YOUR_PRIVATE_KEY"
        },
        "http_port": 8545,
        "ws_port": 8546,
        "db_path": "/var/lib/orbit/db",
        "log_level": "info"
      }
      ```

    **Paso 3: Implementar Mecanismos de Verificación:**

    * Asegúrate de que los miembros del DAC implementen mecanismos de verificación para comprobar la disponibilidad de datos y reportar cualquier anomalía.
    * Estos mecanismos pueden incluir auditorías periódicas y herramientas de monitoreo para asegurar la integridad de los datos.

#### 2. Uso de IPFS (InterPlanetary File System) <a href="#block-c67b3f7047ac4ae8acd43838b5526b0a" id="block-c67b3f7047ac4ae8acd43838b5526b0a"></a>

**IPFS:**

1. **Qué es IPFS:**
   1. IPFS es un sistema de archivos distribuido que permite almacenar y acceder a archivos y datos de manera descentralizada.
      * Utiliza un modelo de red peer-to-peer para asegurar que los datos estén siempre disponibles, incluso si algunos nodos fallan.
2. **Integración con IPFS:**
   1.  **Paso 1: Instalar y Configurar IPFS:**

       * Instala IPFS en tu servidor siguiendo las instrucciones en [ipfs.io](https://ipfs.io/).
       * ```powershell
         sudo apt install ipfs
         ipfs init
         ipfs daemon
         ```

       **Paso 2: Almacenar Datos en IPFS:**

       * Añade los datos de la blockchain a IPFS y obtiene el hash CID correspondiente.
         * ```powershell
           ipfs add -r /path/to/blockchain/data
           ```

       **Paso 3: Publicar el CID en la Configuración de Orbit:**

       * Configura tu nodo Orbit para referenciar los datos almacenados en IPFS utilizando el CID obtenido.
         * ```json
           {
             "rollup": {
               "chain_id": 421611,
               "base_chain_rpc": "<https://mainnet.infura.io/v3/YOUR_INFURA_PROJECT_ID>",
               "confirmations": 12,
               "data_availability": {
                 "ipfs": {
                   "cid": "Qm...YOUR_CID"
                 }
               }
             },
             "sequencer": {
               "enable": true,
               "priv_key": "YOUR_PRIVATE_KEY"
             },
             "staker": {
               "enable": true,
               "priv_key": "YOUR_PRIVATE_KEY"
             },
             "http_port": 8545,
             "ws_port": 8546,
             "db_path": "/var/lib/orbit/db",
             "log_level": "info"
           }
           ```

#### 3. Monitoreo y Alertas <a href="#block-525f48e5232e41568336bedb7262665c" id="block-525f48e5232e41568336bedb7262665c"></a>

1. **Configurar Herramientas de Monitoreo:**
   1. Utiliza herramientas como Prometheus y Grafana para monitorear el estado de los nodos y la disponibilidad de datos.
2. **Configurar Alertas:**
   1. Configura alertas para recibir notificaciones en caso de problemas de disponibilidad de datos.
   2. Ejemplo de configuración de alerta en Grafana:
      * Navega a "Alerting" > "Notification channels".
      * Añade un nuevo canal de notificaciones (e.g., correo electrónico, Slack).
      * Configura reglas de alerta basadas en métricas de disponibilidad.
      * ```yaml
        alerting:
          alertmanagers:
            - static_configs:
                - targets:
                  - 'localhost:9093'
        rule_files:
          - "alert.rules"

        groups:
          - name: availability_alerts
            rules:
              - alert: NodeDown
                expr: up == 0
                for: 5m
                labels:
                  severity: page
                annotations:
                  summary: "Node is down"
                  description: "The node {{ $labels.instance }} is down for more than 5 minutes."
        ```

#### Conclusión <a href="#block-8fb77f3a94634c08876633f642b87ac2" id="block-8fb77f3a94634c08876633f642b87ac2"></a>

Estas estrategias asegurarán que los datos en tu cadena Orbit estén siempre disponibles y accesibles, manteniendo la integridad y el funcionamiento de la red.

</details>

### **Aplicaciones del Mundo Real y Estudios de Caso** <a href="#block-3cd32b7b56854d4683cded7b28c76de5" id="block-3cd32b7b56854d4683cded7b28c76de5"></a>

* Discusión de aplicaciones construidas en cadenas Orbit.

<details>

<summary>Aplicaciones del Mundo Real y Estudios de Caso</summary>

### Discusión de Aplicaciones Construidas en Cadenas Orbit <a href="#block-c2997bc83b164ae686c57b9fa30639f0" id="block-c2997bc83b164ae686c57b9fa30639f0"></a>

**Objetivo:** Analizar y discutir aplicaciones reales construidas en cadenas Orbit, explorando cómo estas soluciones están siendo implementadas y qué beneficios aportan al ecosistema blockchain.

### Introducción <a href="#block-2e3a59bb4636440089bc0e4425ecc27b" id="block-2e3a59bb4636440089bc0e4425ecc27b"></a>

Las cadenas Orbit permiten una mayor escalabilidad y flexibilidad en el desarrollo de aplicaciones descentralizadas (dApps). A continuación, se presentan algunos ejemplos de aplicaciones del mundo real y estudios de caso que demuestran el potencial de las cadenas Orbit.

### Aplicaciones del Mundo Real <a href="#block-c7982a21ec844d40afd59d0f8de6ee9d" id="block-c7982a21ec844d40afd59d0f8de6ee9d"></a>

#### 1. XAI Games en Orbit <a href="#block-3d2f0c5176994b83bde3135f1737acbe" id="block-3d2f0c5176994b83bde3135f1737acbe"></a>

**Descripción:**

* XAI Games es una plataforma de juegos basada en blockchain que ofrece una variedad de juegos que utilizan tecnologías descentralizadas para mejorar la transparencia y la equidad en los juegos.

**Implementación en Orbit:**

* La implementación de XAI Games en una cadena Orbit permite transacciones rápidas y tarifas bajas, mejorando la experiencia del jugador.

**Beneficios:**

* Aumento en la velocidad de las transacciones y reducción de costos operativos debido a la mayor eficiencia de las cadenas Orbit.
* Mejora en la transparencia y equidad de los juegos debido a la naturaleza inmutable de la blockchain.

**Enlace oficial:** [XAI Games Documentation](https://xai-foundation.gitbook.io/xai-network)

#### 2. RARI Chain en Orbit <a href="#block-095a03f3b2694deeba86a77348562d43" id="block-095a03f3b2694deeba86a77348562d43"></a>

**Descripción:**

* RARI Chain es una plataforma de finanzas NFT (NFT-Fi) que permite a los usuarios intercambiar, prestar y pedir prestado activos digitales y NFTs de manera descentralizada.

**Implementación en Orbit:**

* Desplegar RARI Chain en una cadena Orbit proporciona una infraestructura más eficiente y escalable para las transacciones financieras y de NFTs.

**Beneficios:**

* Reducción de tarifas de transacción y tiempos de confirmación más rápidos.
* Mayor seguridad y accesibilidad para los usuarios de NFT-Fi.

**Enlace oficial:** [RARI Chain Documentation](https://rari.docs.caldera.dev/)

#### 3. Deri Protocol en Orbit <a href="#block-f2fdc4e5e1114aa8badb4b17bbbbe73f" id="block-f2fdc4e5e1114aa8badb4b17bbbbe73f"></a>

**Descripción:**

* Deri Protocol es una plataforma de derivados descentralizados que permite a los usuarios negociar contratos perpetuos, opciones y otros productos derivados directamente desde sus billeteras.

**Implementación en Orbit:**

* La implementación en Orbit permite manejar un alto volumen de transacciones con tarifas bajas, lo cual es crucial para la negociación de derivados.

**Beneficios:**

* Mejor experiencia del usuario gracias a la reducción de costos y tiempos de transacción.
* Mayor liquidez y eficiencia en el mercado de derivados.

**Enlace oficial:** [Deri Protocol Documentation](https://docs.deri.io/)

#### 4. Polychain Monsters en Orbit <a href="#block-dd04032ab3864eefb15490d971abd6a5" id="block-dd04032ab3864eefb15490d971abd6a5"></a>

**Descripción:**

* Polychain Monsters es un juego de colección de monstruos digitales basado en NFTs, donde los jugadores pueden coleccionar, intercambiar y mejorar sus criaturas.

**Implementación en Orbit:**

* Migrar Polychain Monsters a una cadena Orbit mejora la escalabilidad del juego, permitiendo más transacciones por segundo y reduciendo costos.

**Beneficios:**

* Mejor rendimiento del juego y experiencia del usuario debido a la reducción de latencia y tarifas de gas.
* Mayor adopción debido a la accesibilidad y asequibilidad de las transacciones en la plataforma.

**Enlace oficial:** [Polychain Monsters Documentation](https://community-docs.polychainmonsters.com/doc)

### Casos de Estudio <a href="#block-48b16ecc6bef42a3a2976bf917ef4aae" id="block-48b16ecc6bef42a3a2976bf917ef4aae"></a>

#### Caso 1: RARI Chain en Orbit <a href="#block-fc2f3a0dd17446a1be277893e48aa6cb" id="block-fc2f3a0dd17446a1be277893e48aa6cb"></a>

**Descripción del Proyecto:**

* RARI Chain es una plataforma de finanzas NFT (NFT-Fi) que permite a los usuarios intercambiar, prestar y pedir prestado activos digitales y NFTs de manera descentralizada.
* **Enlace oficial:** [RARI Chain Documentation](https://rari.docs.caldera.dev/)

**Desafíos Abordados:**

* Escalabilidad: La cadena principal de Ethereum enfrenta problemas de congestión y altas tarifas de gas.
* Velocidad de Transacción: Las transacciones en Ethereum pueden ser lentas durante los picos de uso.

**Solución con Orbit:**

* Desplegar RARI Chain en una cadena Orbit permite manejar un mayor volumen de transacciones con tarifas significativamente más bajas.
* La cadena Orbit proporciona una experiencia de usuario más fluida debido a tiempos de confirmación más rápidos.

**Resultados:**

* Aumento en el número de usuarios activos debido a la reducción de costos y mejora en la velocidad de las transacciones.
* Mayor liquidez en los pools de RARI Chain, fomentando una economía NFT-Fi más robusta.

#### Caso 2: Deri Protocol en Orbit <a href="#block-359850cc82ed4c2f86bd13defa0df180" id="block-359850cc82ed4c2f86bd13defa0df180"></a>

**Descripción del Proyecto:**

* Deri Protocol es una plataforma de derivados descentralizados que permite a los usuarios negociar contratos perpetuos, opciones y otros productos derivados directamente desde sus billeteras.
* **Enlace oficial:** [Deri Protocol Documentation](https://docs.deri.io/)

**Desafíos Abordados:**

* Escalabilidad: Manejar el alto volumen de transacciones necesarias para los productos derivados.
* Costos: Reducción de las tarifas de transacción para hacer la negociación más accesible.

**Solución con Orbit:**

* Implementar Deri Protocol en una cadena Orbit permite manejar más transacciones por segundo y reducir las tarifas.
* Ofrece una infraestructura más eficiente y escalable para las transacciones de derivados.

**Resultados:**

* Mejor experiencia del usuario gracias a la reducción de costos y tiempos de transacción.
* Mayor liquidez y eficiencia en el mercado de derivados.

#### Conclusión <a href="#block-7784fd0a729f494db92100e63cabaf61" id="block-7784fd0a729f494db92100e63cabaf61"></a>

Las cadenas Orbit ofrecen soluciones escalables y eficientes para una variedad de aplicaciones en el mundo real, desde DeFi y NFT-Fi hasta juegos y gestión de derivados. Estos estudios de caso demuestran cómo las cadenas Orbit pueden mejorar significativamente la experiencia del usuario, reducir costos y aumentar la adopción de tecnologías blockchain.

</details>

### **Integración del Toolkit de Ethereum** <a href="#block-7fe1f23919c14cf2af49df7c019c1649" id="block-7fe1f23919c14cf2af49df7c019c1649"></a>

* Uso de toolkits de Ethereum para el desarrollo en la cadena Orbit.

<details>

<summary>Integración del Toolkit de Ethereum</summary>

#### Uso de Toolkits de Ethereum para el Desarrollo en la Cadena Orbit <a href="#block-fc95afe732bd4fbe8213a450f189c285" id="block-fc95afe732bd4fbe8213a450f189c285"></a>

**Objetivo:** Aprender a utilizar toolkits de Ethereum para desarrollar aplicaciones en la cadena Orbit, aprovechando las herramientas y bibliotecas populares de Ethereum.

#### Introducción <a href="#block-f19e8e833d9f495dbcbccdd85a65a344" id="block-f19e8e833d9f495dbcbccdd85a65a344"></a>

Las herramientas y bibliotecas desarrolladas para Ethereum pueden ser utilizadas en cadenas Orbit debido a su compatibilidad. Esto permite a los desarrolladores aprovechar un amplio ecosistema de recursos y herramientas probadas para construir aplicaciones descentralizadas (dApps) en Orbit.

#### Toolkits de Ethereum Populares <a href="#block-3f176165ed92448eb411e775e7981fbb" id="block-3f176165ed92448eb411e775e7981fbb"></a>

#### 1. Hardhat <a href="#block-fed874db035c4aa6b2019fdca64547b9" id="block-fed874db035c4aa6b2019fdca64547b9"></a>

**Descripción:**

* Hardhat es un entorno de desarrollo para Ethereum que permite compilar, desplegar, probar y depurar contratos inteligentes.

**Uso en Orbit:**

* Hardhat puede ser configurado para trabajar con cadenas Orbit, facilitando el desarrollo y despliegue de contratos inteligentes.

**Pasos para Configurar Hardhat:**

1. **Instalar Hardhat:**
   1. En la terminal, dentro de tu proyecto, ejecuta:
      * ```powershell
        npm install --save-dev hardhat
        ```
2. **Crear un Proyecto Hardhat:**
   1. Inicia un nuevo proyecto de Hardhat:
      * ```powershell
        npx hardhat
        ```
      * Sigue las instrucciones para crear un proyecto básico.
3. **Configurar la Red Orbit:**
   1. Abre el archivo `hardhat.config.js` y añade la configuración de la red Orbit:
      * ```javascript
        require("@nomiclabs/hardhat-waffle");

        module.exports = {
          solidity: "0.8.4",
          networks: {
            orbit: {
              url: "<http://localhost:8545>",
              accounts: [`0x${YOUR_PRIVATE_KEY}`]
            }
          }
        };
        ```
4. **Ejemplo de Despliegue de Contrato:**
   1.  Crea un archivo `deploy.js` en la carpeta `scripts`:

       * ```javascript
         async function main() {
           const [deployer] = await ethers.getSigners();

           console.log("Deploying contracts with the account:", deployer.address);

           const Contract = await ethers.getContractFactory("YourContract");
           const contract = await Contract.deploy();

           console.log("Contract deployed to:", contract.address);
         }

         main()
           .then(() => process.exit(0))
           .catch((error) => {
             console.error(error);
             process.exit(1);
           });
         ```

       Despliega el contrato en la red Orbit:

       * ```powershell
         npx hardhat run scripts/deploy.js --network orbit
         ```

#### 2. Foundry <a href="#block-252a9ed9fb3b4497b5738651213a3ecc" id="block-252a9ed9fb3b4497b5738651213a3ecc"></a>

**Descripción:**

* Foundry es un toolkit rápido y portátil para desarrollar contratos inteligentes, que incluye herramientas para compilar, probar, desplegar y depurar contratos.

**Uso en Orbit:**

* Foundry puede ser configurado para trabajar con cadenas Orbit, proporcionando un entorno eficiente y moderno para el desarrollo de dApps.

**Pasos para Configurar Foundry:**

1. **Instalar Foundry:**
   1. En la terminal, instala Foundry ejecutando:
      * ```powershell
        curl -L <https://foundry.paradigm.xyz> | bash
        foundryup
        ```
2. **Inicializar un Proyecto Foundry:**
   1. Inicia un nuevo proyecto de Foundry:
      * ```powershell
        forge init my-foundry-project
        cd my-foundry-project
        ```
3. **Configurar la Red Orbit:**
   1. Abre el archivo `foundry.toml` y añade la configuración de la red Orbit:
      * ```toml
        [default]
        rpc_url = "<http://localhost:8545>"
        ```
4. **Ejemplo de Despliegue de Contrato:**
   1.  Crea un archivo de script de despliegue en `script/Deploy.s.sol`:

       * ```solidity
         // SPDX-License-Identifier: MIT
         pragma solidity ^0.8.4;

         import "forge-std/Script.sol";
         import "../src/YourContract.sol";

         contract Deploy is Script {
             function run() external {
                 vm.startBroadcast();
                 new YourContract();
                 vm.stopBroadcast();
             }
         }
         ```

       Despliega el contrato en la red Orbit:

       * ```powershell
         forge script script/Deploy.s.sol:Deploy --rpc-url <http://localhost:8545> --broadcast
         ```

#### 3. Ethers.js <a href="#block-d7c426f6df3f40a08736a83c78058e7a" id="block-d7c426f6df3f40a08736a83c78058e7a"></a>

**Descripción:**

* Ethers.js es una biblioteca de JavaScript para interactuar con la blockchain de Ethereum, proporcionando una API limpia y completa para desarrolladores.

**Uso en Orbit:**

* Ethers.js puede ser usado en aplicaciones front-end para interactuar con contratos desplegados en la red Orbit.

**Pasos para Configurar Ethers.js:**

1. **Instalar Ethers.js:**
2.
   * En el directorio de tu proyecto, instala Ethers.js:
   * Copy
   * ```powershell
     npm install ethers
     ```
3. **Conectar a la Red Orbit:**
4.
   * Usa Ethers.js para conectarte a la red Orbit y realizar operaciones con contratos inteligentes:
   * Copy
   * ```javascript
     const { ethers } = require("ethers");
     const provider = new ethers.providers.JsonRpcProvider("<http://localhost:8545>");

     const contractABI = [ /* ABI del contrato */ ];
     const contractAddress = "0xCONTRACT_ADDRESS";
     const contract = new ethers.Contract(contractAddress, contractABI, provider);

     async function getContractData() {
       const data = await contract.someMethod();
       console.log(data);
     }

     getContractData();
     ```

#### 4. Viem <a href="#block-41ad8fd8407445b1823febfa13ba65ec" id="block-41ad8fd8407445b1823febfa13ba65ec"></a>

**Descripción:**

* Viem es una biblioteca de TypeScript para interactuar con la blockchain de Ethereum, proporcionando una API moderna y completa, adecuada para desarrolladores que prefieren TypeScript.

**Uso en Orbit:**

* Viem puede ser usado en aplicaciones front-end para interactuar con contratos desplegados en la red Orbit, ofreciendo ventajas adicionales al estar escrito en TypeScript.

**Pasos para Configurar Viem:**

1. **Instalar Viem:**
2.
   * En el directorio de tu proyecto, instala Viem:
   * Copy
   * ```powershell
     npm install viem
     ```
3. **Conectar a la Red Orbit:**
4.
   * Usa Viem para conectarte a la red Orbit y realizar operaciones con contratos inteligentes:
   * Copy
   * ```typescript
     import { createPublicClient, http } from 'viem';
     import { mainnet } from 'viem/chains';

     const client = createPublicClient({
       chain: mainnet,
       transport: http('<http://localhost:8545>'),
     });

     const contractABI = [ /* ABI del contrato */ ];
     const contractAddress = '0xCONTRACT_ADDRESS';

     async function getContractData() {
       const data = await client.readContract({
         address: contractAddress,
         abi: contractABI,
         functionName: 'someMethod',
       });
       console.log(data);
     }

     getContractData();
     ```

</details>

### **Ejercicio Práctico: Integración de dApp** <a href="#block-dc8bc84bfa7d490081349e2ebfdd47cb" id="block-dc8bc84bfa7d490081349e2ebfdd47cb"></a>

* Integración de una aplicación descentralizada simple con una cadena local Orbit.

<details>

<summary>Ejercicio Práctico: Integración de dApp</summary>

### Integración de una Aplicación Descentralizada Simple con una Cadena Local Orbit <a href="#block-ec9e34cc51fa496091cbdbb2711afb41" id="block-ec9e34cc51fa496091cbdbb2711afb41"></a>

**Objetivo:** Guiar a los participantes a través del proceso de integración de una aplicación descentralizada (dApp) simple con una cadena local Orbit, utilizando herramientas populares de Ethereum como Hardhat y Ethers.js.

### Preparación <a href="#block-3f5d2792821440f98abfcccfd59ff2e6" id="block-3f5d2792821440f98abfcccfd59ff2e6"></a>

**Prerrequisitos:**

1. Tener Node.js y npm instalados.
2. Tener Hardhat y Ethers.js configurados.
3. Tener una cadena local Orbit corriendo.

### Pasos del Ejercicio <a href="#block-94280055d8b44fa28e8478b9b10b28ad" id="block-94280055d8b44fa28e8478b9b10b28ad"></a>

#### 1. Configuración del Proyecto <a href="#block-44c7c89bb35049f39b1565c297a67bb8" id="block-44c7c89bb35049f39b1565c297a67bb8"></a>

**Crear un Proyecto con Hardhat:**

En la terminal, crea y navega a un nuevo directorio para tu proyecto:

* ```shell
  mkdir my-dapp
  cd my-dapp
  ```

Inicializa un nuevo proyecto de Hardhat:

* ```shell
  npm init -y
  npm install --save-dev hardhat
  npx hardhat
  ```
* Sigue las instrucciones para crear un proyecto básico.

**Instalar Dependencias Necesarias:**

Instala Ethers.js y otras dependencias necesarias:

* ```shell
  npm install --save ethers
  npm install --save-dev @nomiclabs/hardhat-waffle @nomiclabs/hardhat-ethers
  ```

**Configurar Hardhat:**

* Abre el archivo `hardhat.config.js` y configura la red Orbit:

<!---->

* ```javascript
  require("@nomiclabs/hardhat-waffle");
  require("@nomiclabs/hardhat-ethers");

  module.exports = {
    solidity: "0.8.4",
    networks: {
      orbit: {
        url: "<http://localhost:8545>",
        accounts: [`0x${YOUR_PRIVATE_KEY}`]
      }
    }
  };
  ```

#### 2. Crear y Desplegar el Contrato Inteligente <a href="#block-491db2bda175482a8e57332af38dc0a7" id="block-491db2bda175482a8e57332af38dc0a7"></a>

**Crear el Contrato Inteligente:**

* En el directorio `contracts`, crea un nuevo archivo llamado `SimpleStorage.sol`:

<!---->

* ```solidity
  // SPDX-License-Identifier: MIT
  pragma solidity ^0.8.4;

  contract SimpleStorage {
      uint256 public data;

      function set(uint256 _data) public {
          data = _data;
      }

      function get() public view returns (uint256) {
          return data;
      }
  }
  ```

**Compilar el Contrato:**

* En la terminal, compila el contrato inteligente:

<!---->

* ```shell
  npx hardhat compile
  ```

**Crear un Script de Despliegue:**

* En el directorio `scripts`, crea un nuevo archivo llamado `deploy.js`:

<!---->

* ```javascript
  async function main() {
    const [deployer] = await ethers.getSigners();

    console.log("Deploying contracts with the account:", deployer.address);

    const SimpleStorage = await ethers.getContractFactory("SimpleStorage");
    const simpleStorage = await SimpleStorage.deploy();

    console.log("SimpleStorage deployed to:", simpleStorage.address);
  }

  main()
    .then(() => process.exit(0))
    .catch((error) => {
      console.error(error);
      process.exit(1);
    });
  ```

**Desplegar el Contrato en la Red Orbit:**

* En la terminal, ejecuta el script de despliegue:

<!---->

* ```shell
  npx hardhat run scripts/deploy.js --network orbit
  ```

#### 3. Integrar la dApp con el Frontend <a href="#block-8477b1cd6a324d71b913ceefab79f8d4" id="block-8477b1cd6a324d71b913ceefab79f8d4"></a>

**Crear un Proyecto React:**

* En el directorio principal, crea un nuevo proyecto React:

<!---->

* ```shell
  npx create-react-app my-dapp-frontend
  cd my-dapp-frontend
  ```

**Instalar Ethers.js en el Proyecto React:**

* En la terminal, instala Ethers.js:

<!---->

* ```shell
  npm install ethers
  ```

**Crear la Interfaz de Usuario:**

* Abre el archivo `src/App.js` y modifícalo para incluir la lógica de interacción con el contrato inteligente:

<!---->

* ```javascript
  import React, { useState } from 'react';
  import { ethers } from 'ethers';
  import './App.css';

  const CONTRACT_ADDRESS = '0xYOUR_CONTRACT_ADDRESS';
  const CONTRACT_ABI = [
    // ABI del contrato
    "function set(uint256 _data) public",
    "function get() public view returns (uint256)"
  ];

  function App() {
    const [data, setData] = useState('');
    const [provider, setProvider] = useState(null);
    const [signer, setSigner] = useState(null);
    const [contract, setContract] = useState(null);

    const connectWallet = async () => {
      if (window.ethereum) {
        const prov = new ethers.providers.Web3Provider(window.ethereum);
        await prov.send("eth_requestAccounts", []);
        const sign = prov.getSigner();
        const cont = new ethers.Contract(CONTRACT_ADDRESS, CONTRACT_ABI, sign);

        setProvider(prov);
        setSigner(sign);
        setContract(cont);
      } else {
        console.error('MetaMask is not installed!');
      }
    };

    const setDataOnChain = async () => {
      if (contract) {
        const tx = await contract.set(data);
        await tx.wait();
      }
    };

    const getDataFromChain = async () => {
      if (contract) {
        const result = await contract.get();
        alert(result.toString());
      }
    };

    return (
      <div className="App">
        <header className="App-header">
          <button onClick={connectWallet}>Connect Wallet</button>
          <div>
            <input
              type="text"
              value={data}
              onChange={(e) => setData(e.target.value)}
              placeholder="Set Data"
            />
            <button onClick={setDataOnChain}>Set Data</button>
            <button onClick={getDataFromChain}>Get Data</button>
          </div>
        </header>
      </div>
    );
  }

  export default App;
  ```

**Ejecutar la dApp:**

* En la terminal, inicia el proyecto React:

<!---->

* ```shell
  npm start
  ```
* Abre tu navegador y navega a `http://localhost:3000`. Conecta tu billetera MetaMask y prueba la funcionalidad de la dApp para interactuar con el contrato desplegado en la cadena Orbit.

### Conclusión <a href="#block-d9005b0d17d14c31907a621e10be8f2c" id="block-d9005b0d17d14c31907a621e10be8f2c"></a>

Este ejercicio práctico guía a los participantes a través del proceso de configuración de un entorno de desarrollo, creación y despliegue de un contrato inteligente simple, y la integración de una dApp con una cadena local Orbit. Utilizando herramientas populares como Hardhat y Ethers.js, los desarrolladores pueden aprovechar un ecosistema robusto para construir y desplegar aplicaciones descentralizadas de manera eficiente.

</details>
