# Clientes: Opciones y Funcionalidades

{% embed url="https://mirror.xyz/seedlatam.eth/9KSXm8Ai9f8LUOyMBBeJFiCMFc82mGUWUg_bGaE1bLY" %}

En el universo de Ethereum, los clientes son herramientas esenciales para interactuar con la blockchain. Estos clientes, que son piezas de software diseñadas para conectarse y operar entre sí, ofrecen una variedad de características y funcionalidades que satisfacen las necesidades de diferentes usuarios y aplicaciones.

En este artículo, exploraremos exhaustivamente los diversos clientes de Ethereum disponibles en el mercado actual, con el objetivo de proporcionar una visión detallada de sus características, opciones y usos prácticos en el contexto del desarrollo de aplicaciones descentralizadas, la validación de transacciones y la participación en la red Ethereum.

### **¿Qué es un Cliente?** <a href="#heading-que-es-un-cliente" id="heading-que-es-un-cliente"></a>

Un cliente es una implementación de Ethereum que verifica los datos con las reglas del protocolo y mantiene la red segura. Un nodo tiene que ejecutar dos clientes: un cliente de consenso y un cliente de ejecución.

> _Recordemos qué es un nodo:_ \
> &#xNAN;_&#x55;n "nodo" es cualquier instancia del software cliente de Ethereum que está conectada a otras computadoras que también ejecutan el software Ethereum, formando una red._

Los clientes facilitan la validación de transacciones, la ejecución de contratos inteligentes y la comunicación con otros nodos en la red, lo que es fundamental para mantener la integridad y la seguridad de la red Ethereum en su conjunto.

## **Los diferentes clientes de Ethereum** <a href="#heading-los-diferentes-clientes-de-ethereum" id="heading-los-diferentes-clientes-de-ethereum"></a>

En la actualidad, existen varios clientes de Ethereum desarrollados por diferentes equipos y organizaciones. Algunos de los clientes más populares incluyen:

## Geth <a href="#heading-geth" id="heading-geth"></a>

Geth (go-ethereum) es una implementación de Go en Ethereum. Fue una de las implementaciones originales de Ethereum, lo que lo convierte en el cliente más probado y experimentado.

[Getting started with Geth | go-ethereumGuide to getting up and running with Geth using Clef.geth.ethereum.org](https://geth.ethereum.org/docs/getting-started)

Geth se controla principalmente mediante la línea de comandos. Se inicia Geth utilizando el comando `geth`. Se detiene presionando `ctrl+c.`

Pueden configurar Geth utilizando opciones de línea de comandos (también conocidas como flags). Geth también tiene subcomandos, que se pueden utilizar para invocar funcionalidades como la consola o la importación/exportación de la blockchain.

#### Comandos <a href="#heading-comandos" id="heading-comandos"></a>

Para poder ver la lista de comandos en cualquier momento desde tu propia instancia de Geth ejecutamos:

```powershell
geth --help
```

Como la lista de comandos es muy extensa, vamos a describir la estructura de las opciones que nos deja ejecturar geth:

Primero el uso es el siguiente:

```powershell
 geth [global options] command [command options] [arguments...]
```

Tenemos muchísimas opciones, estos son sólo algunos relevantes:

1. `account`: Gestiona cuentas
2. `dumpconfig`: Exporta valores de configuración en formato TOML
3. `export`: Exporta la blockchain a un archivo
4. `import`: Importa un archivo de blockchain
5. `removedb`: Elimina la blockchain y las bases de datos de estado
6. `snapshot`: Conjunto de comandos basados en el snapshot
7. `version`: Muestra los números de versión
8. `wallet`: Gestiona las billeteras de precompra de Ethereum

Ademas de estos, geth nos brinda [**Opciones Globales**](https://geth.ethereum.org/docs/fundamentals/command-line-options) muy utiles que se estructuran de la siguiente forma:

1. **Log Rotate**:
   * `-log.rotate`: Habilita la rotación de archivos de registro.
2. **Account**:
   * Varias opciones relacionadas con la gestión de cuentas, como desbloqueo seguro, directorio de keystore, configuraciones de seguridad, etc.
3. **Aliased (Deprecated)**:
   * Configuraciones obsoletas que aún se pueden usar, pero se recomienda utilizar alternativas más actualizadas.
4. **API and Console**:
   * Configuraciones relacionadas con la activación y personalización de la API HTTP-RPC y la consola interactiva.
5. **Developer Chain**:
   * Opciones para configurar una red de prueba de autoridad de prueba con cuenta de desarrollador prefinanciada y minería habilitada.
6. **Ethereum**:
   * Configuraciones generales de red, como la cadena de bloques a utilizar, directorio de datos, configuraciones de red, etc.
7. **Gas Price Oracle**:
   * Configuraciones relacionadas con el oráculo de precios de gas y las recomendaciones de tarifas de transacción.
8. **Light Client**:
   * Opciones para configurar y gestionar clientes ligeros en la red Ethereum.
9. **Logging and Debugging**:
   * Configuraciones para el registro y depuración del nodo, como nivel de verbosidad, formato de registro, etc.
10. **Metrics and Stats**:
    * Opciones para habilitar la recolección de métricas y estadísticas del nodo.
11. **Miner**:
    * Configuraciones relacionadas con la minería, como dirección de recompensa, límites de gas, etc.
12. **Misc**:
    * Otras configuraciones diversas, como la visualización de ayuda, impresión de versión, etc.
13. **Networking**:
    * Configuraciones de red, como nodos de arranque, descubrimiento de pares, límites de conexión, etc.
14. **Performance Tuning**:
    * Ajustes para optimizar el rendimiento del nodo, como el uso de memoria caché, límites de recursos, etc.
15. **State History Management**:
    * Configuraciones relacionadas con la gestión del historial de estado de la blockchain.
16. **Transaction Pool (Blob)**:
    * Configuraciones específicas para el pool de transacciones blob.
17. **Transaction Pool (EVM)**:
    * Configuraciones específicas para el pool de transacciones EVM.
18. **Virtual Machine**:
    * Opciones de depuración y seguimiento para la máquina virtual y contratos inteligentes.

### Tools <a href="#heading-tools" id="heading-tools"></a>

También dentro del mundo de Geth nos encontramos con varias herramientas que pueden ser de nuestro interés:

#### Clef <a href="#heading-clef" id="heading-clef"></a>

Clef es una herramienta para firmar transacciones y datos de manera segura en un entorno local. Permite gestionar claves de forma independiente a Geth, lo que brinda flexibilidad y seguridad al proceso de firma. Puede utilizarse de manera autónoma o integrarse con Geth, siendo útil para situaciones donde se accede a Ethereum a través de nodos remotos no confiables.

[Introduction to Clef | go-ethereumIntroduction to the external signing tool, Clefgeth.ethereum.org](https://geth.ethereum.org/docs/tools/clef/introduction)

#### Abigen <a href="#heading-abigen" id="heading-abigen"></a>

Abigen es una herramienta que facilita la interacción con Ethereum utilizando Go. Genera paquetes Go type-safe a partir de definiciones de smart contracts conocidas como ABIs.

Un ABI es un archivo JSON que especifica cómo codificar y decodificar datos enviados y recibidos por un smart contract. Abigen simplifica la implementación e interacción con smart contracts en aplicaciones Go al abstraer la complejidad de la codificación y decodificación de datos.

[Abigen | go-ethereumIntroduction to the Go binding generator tool, Abigengeth.ethereum.org](https://geth.ethereum.org/docs/tools/abigen)

#### Nethermind <a href="#heading-nethermind" id="heading-nethermind"></a>

Este es un cliente de Ethereum desarrollado en C#/.NET, que destaca por su eficiencia y su enfoque en el desarrollo empresarial y en la interoperabilidad.

[GitHub - NethermindEth/sedge: A one-click setup tool for PoS network/chain validators and nodes.A one-click setup tool for PoS network/chain validators and nodes. - NethermindEth/sedgegithub.com](https://github.com/NethermindEth/sedge)

#### Sedge <a href="#heading-sedge" id="heading-sedge"></a>

Sedge es una herramienta de configuración de nodos con “un solo clic” para validadores y nodos de redes/cadenas de Proof of Stake, escrita completamente en el lenguaje de programación Go. Sedge se encarga de toda la configuración del nodo completo localmente, basándose en el cliente elegido y utilizando scripts docker-compose generados según la configuración deseada, está actualmente disponible solo para Linux, macOS y Windows, para arquitecturas amd64 y arm64.

Guia de instalación:

[Installation guide | Sedge documentationSedge is currently available only for Linux, macOS, and Windows, for amd64 and arm64 architectures. You can use any of the follo…docs.sedge.nethermind.io](https://docs.sedge.nethermind.io/docs/quickstart/install-guide)

Una vez que usen `sedge cli` o `sedge generate` para generar el archivo `docker-compose.yml`, podrán manejarlo por su cuenta. Esta guía les mostrará cómo hacerlo.

Existen varias razones por las que necesitarías gestionar la configuración después de usar Sedge para generar los archivos de configuración. Puede que deseen modificar el archivo `.env` o `docker-compose.yml` para cambiar la configuración de la instalación, o simplemente copiar estos archivos a otra máquina. Actualmente, Sedge ejecuta el stack de docker compose que fue generado usando `sedge run`. Puedes usar `sedge logs` y `sedge down` en cualquier `docker-compose.yml`.

Pueden señalar hacia una ruta (path) generación diferente utilizando la opción `--path`. Esto es útil si desean generar los archivos en un directorio distinto, o si quieren generar múltiples configuraciones.

* `Sedge logs:`

Ejecutar `sedge logs` obtendrá los registros de contenedores en ejecución utilizando la CLI de docker compose.

```powershell
$ sedge logs -h
Initializing configuration
Get running container logs using docker-compose CLI. If no services are provided, the logs of all running services will be displayed.

    By default will run 'docker compose -f <script> logs --follow <service>'

Usage:
  sedge logs [flags] [services]

Flags:
  -h, --help          help for logs
  -p, --path string   docker-compose script path (default "./docker-compose-scripts")
  -t, --tail int      Tail the number of desired logs. If not set, or set to 0, logs are followed.

Global Flags:
      --logLevel string   Set Log Level (default "info")
```

* `Sedge down:`

Ejecutar `sedge down` detendrá y eliminará los contenedores utilizando la CLI de Docker Compose.

```powershell
$ sedge down -h
Shutdown sedge running containers using docker compose CLI. Shortcut for 'docker compose -f <script> down'

Usage:
  sedge down [flags]

Flags:
  -h, --help          help for down
  -p, --path string   docker-compose script path (default "./docker-compose-scripts")

Global Flags:
      --logLevel string   Set Log Level (default "info")
```

Ejemplo:\
La ejecución de `sedge down` cerrará y eliminará todos los contenedores y redes abiertas utilizadas por Sedge.

```powershell
$ sedge down
2022-00-00 00:00:00 -- [INFO] [Logger Init] Log level: info
2022-00-00 00:00:00 -- [INFO] You are running the latest version of sedge. Version:  v1.3.2
[sudo] password for maceo:
[+] Running 7/7
 ⠿ Container execution-client                            Removed                                                                 93.8s
 ⠿ Container validator-client                            Removed                                                                  0.0s
 ⠿ Container consensus-client                            Removed                                                                  3.8s
 ⠿ Container validator-import-client                     Removed                                                                  0.0s
 ⠿ Network docker-compose-scripts_default                Removed                                                                  0.2s
 ⠿ Network sedge_network   
```

#### Prender el setup <a href="#heading-prender-el-setup" id="heading-prender-el-setup"></a>

Una vez generado el archivo docker-compose, es posible que modifiquen tanto las variables de entorno en el archivo `.env` como el `docker-compose.yml` bajo el directorio `sedge-data`. Realizarán esta tarea.

Después de eso, podrán ejecutar el siguiente comando para iniciar la configuración desde el directorio `sedge-data:`

```tsx
docker compose up -d
```

O bien, ejecutar el siguiente comando desde cualquier directorio, asumiendo que tienes la ruta al directorio `sedge-data` (llamémoslo `<path>`):

```tsx
docker compose -f <path> up -d
```

#### Apagar el setup <a href="#heading-apagar-el-setup" id="heading-apagar-el-setup"></a>

Para detener la configuración, podrán ejecutar el siguiente comando desde el directorio `sedge-data:`

```tsx
docker compose down
```

O bien, ejecutar el siguiente comando desde cualquier directorio, asumiendo que tienes la ruta al directorio `sedge-data` (llamémoslo `<path>`):

```tsx
docker compose -f <path> down
```

#### Verificar los registros del contenedor <a href="#heading-verificar-los-registros-del-contenedor" id="heading-verificar-los-registros-del-contenedor"></a>

El compose stack está compuesto por varios contenedores de Docker en ejecución.

La configuración para un nodo completo de Ethereum que Sedge aplica consiste en un contenedor para cada nodo (nodo de ejecución, nodo de consenso y nodo validador). Pueden ejecutar el siguiente comando para verificar los registros de un contenedor/nodo específico desde el directorio `sedge-data`:

```tsx
docker compose logs <node>
```

Reemplace `<node>`con el tipo de nodo, por ejemplo: ejecución, consenso, validador

O bien, podrán ejecutar el siguiente comando desde cualquier directorio, asumiendo que tienen la ruta al directorio `sedge-data` (llamémoslo `<path>`):

```tsx
docker compose -f <path> logs <node>
```

Para salir del comando `docker compose logs` presiona `ctrl+c` o `control+c`

(tambien podria agregar otros comandos [https://docs.sedge.nethermind.io/docs/commands](https://docs.sedge.nethermind.io/docs/commands))

#### Erigon <a href="#heading-erigon" id="heading-erigon"></a>

Erigon es una implementación eficiente y optimizada de Ethereum, diseñada para ser más rápida y modular.

Utiliza tecnologías avanzadas como la sincronización escalonada y el almacenamiento de estado eficiente. Es un archieve node por defecto y ha sido reescrito completamente para enfocarse en velocidad y ahorro de almacenamiento.

Puede completar la sincronización de un archive node en menos de tres días y con menos de 2 TB de almacenamiento.

#### Opciones <a href="#heading-opciones" id="heading-opciones"></a>

[Options | Erigon DocumentationAll available optionserigon.gitbook.io](https://erigon.gitbook.io/erigon/advanced-usage/options)

En cuanto a lo que podemos hacer con Erigon es similar a los demas clientes, la forma de acceder a los mandos es utilizando la flag `./build/bin/erigon --help` donde el uso de estos comandos es el siguiente:

```powershell
erigon [command] [flags]
```

Con 5 comandos principales:

`init` Inicializa y arranca un nuevo bloque génesis \
`import` Importa un archivo de cadena de bloques \
`snapshots` Gestiona instantáneas (particiones de datos históricos) \
`support` Conecta una instancia de Erigon a un sistema de diagnóstico para soporte `help, h` Muestra una lista de comandos o ayuda para un comando

Como las flags son un monton, aca dejamos algunas reelevantes:

1. `--datadir value`: Especifica el directorio de datos para las bases de datos.
2. `--ethash.dagdir value`: Establece el directorio para almacenar los DAGs de minería ethash.
3. `--internalcl`: Activa el consenso interno.
4. `--txpool.disable`: Deshabilita la funcionalidad interna de txpool y el productor de bloques.
5. `--prune value`: Configura qué datos antiguos eliminar de la base de datos.
6. `--http`: Habilita el servidor JSON-RPC HTTP.
7. `--http.port value`: Especifica el puerto en el que el servidor HTTP-RPC escucha las solicitudes.
8. `--ws`: Habilita el servidor WS-RPC.
9. `--graphql`: Habilita el endpoint de GraphQL.
10. `--db.pagesize value`: Establece el tamaño de página para la base de datos.

#### **Otterscan** <a href="#heading-otterscan" id="heading-otterscan"></a>

Otterscan es un explorador de bloques de Ethereum diseñado para ser ejecutado localmente junto con Erigon.

Basado completamente en código fuente abierto (open source code), es extremadamente rápido y totalmente privado, ya que funciona en tu máquina local. La interfaz de usuario es intencionalmente muy similar, pero con muchas mejoras, al explorador de bloques de Ethereum más popular para facilitar la ubicación de la información.

Para instalar Otterscan, pueden seguir las instrucciones en el repositorio oficial de Github:

[otterscan/docs/install.md at develop · wmitsuda/otterscanA blazingly fast, local, Ethereum block explorer built on top of Erigon - wmitsuda/otterscangithub.com](https://github.com/wmitsuda/otterscan/blob/develop/docs/install.md)

## Conclusión <a href="#heading-conclusion" id="heading-conclusion"></a>

La elección del cliente de Ethereum dependerá en gran medida de las necesidades y objetivos específicos de cada usuario o proyecto. Es recomendable evaluar las características y capacidades de cada cliente para seleccionar la opción más adecuada según los requisitos de rendimiento, seguridad, facilidad de uso y funcionalidades específicas requeridas. Pero lo mas importante de todo es PROBAR estos clientes y ver que es lo ideal para cada uno de nosotros.
