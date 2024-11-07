---
description: >-
  ¿Eres nuevo en el mundo de la blockchain? ¡Estamos aquí para ayudarte! Si
  necesitas comenzar con conceptos básicos y despejar tus dudas más comunes,
  explora nuestros FAQs (Frequently Asked Questions)
icon: message-question
---

# FAQs

## **Introducción a Nodos**

<details>

<summary><strong>Qué es un Nodo? Qué es un validador?</strong></summary>

* En una blockchain, cualquier computadora conectada se llama nodo. Los nodos completos son aquellos que verifican todas las reglas de la red. Estos nodos validan completamente las transacciones y los bloques. Además, ayudan a la red aceptando transacciones y bloques de otros nodos completos, validándolos y retransmitiéndolos. Los nodos completos son esenciales para la integridad y seguridad de la blockchain, asegurando que todas las transacciones sean legítimas y sigan las reglas del protocolo.
* Un validador es una entidad virtual que reside en la Beacon Chain, representada por un balance, una llave pública y otras propiedades, y participa en el consenso de la red de Ethereum.

</details>

<details>

<summary><strong>Cuáles son los beneficios de ejecutar mi propio nodo?</strong></summary>

* Participar directamente en la red Ethereum.
* Aumentar la descentralización y seguridad de la red.
* Obtener recompensas si decides participar en la validación de transacciones

</details>

<details>

<summary>Qué es un cliente validador</summary>

Un cliente de validador es el software que actúa en nombre del validador al mantener y usar su llave privada para realizar atestaciones sobre el estado de la cadena. Un solo cliente validador puede mantener muchos pares de llaves, controlando múltiples validadores.

</details>

<details>

<summary><strong>Minar Ethereum y tener un nodo es lo mismo?</strong></summary>

No, minar Ethereum y tener un nodo no son lo mismo. Minar Ethereum implica resolver problemas matemáticos complejos para validar transacciones y añadirlas a la blockchain, a cambio de lo cual los mineros reciben recompensas en Ether. Tener un nodo, por otro lado, significa ejecutar un software que mantiene una copia de toda la blockchain y ayuda a mantener la red descentralizada.

</details>

<details>

<summary><strong>Para tener mi propio Nodo, necesito sí o sí tener 32 ETH?</strong></summary>

No necesariamente. El requisito de tener 32 ETH se aplica si quieres ser un validador en Ethereum 2.0, que es un sistema de prueba de participación (PoS). Sin embargo, para ejecutar un nodo en la red Ethereum actual, no necesitas tener ningún ETH. En cuanto a las redes de L2 como Optimism, Arbitrum y Lido, cada una puede tener sus propios requisitos para ejecutar un nodo.

</details>

<details>

<summary><strong>Cómo se comunican los nodos en la red Ethereum?</strong></summary>

Los nodos en la red Ethereum se comunican a través de un software especializado conocido como un cliente. Un “nodo” es cualquier instancia del software de un cliente de Ethereum que está conectado con otros ordenadores que también ejecutan el software de Ethereum, formando una red.

Existen dos tipos de clientes que un nodo necesita ejecutar:

1. **Cliente de ejecución**: Este cliente escucha las nuevas transacciones transmitidas en la red, las ejecuta en la Máquina Virtual de Ethereum (EVM), y mantiene el estado más reciente y la base de datos de todos los datos actuales de Ethereum.
2. **Cliente de consenso**: Este cliente implementa el algoritmo de consenso de la prueba de participación, que permite que la red alcance un acuerdo basado en datos validados del cliente de ejecución.

Además, existe una tercera pieza de software, conocida como “validador”, que se puede agregar al cliente de consenso, lo que permite que un nodo participe en la protección de la red.

Estos clientes trabajan juntos para hacer un seguimiento de la cadena de encabezado de Ethereum y permitir que los usuarios interactúen con la red Ethereum. La diversidad de clientes en diferentes lenguajes de programación desarrollados por diferentes equipos puede fortalecer la red reduciendo su dependencia de una sola base de código.

Es importante mencionar que existen diferentes tipos de nodos que cualquier cliente puede ejecutar dependiendo de sus necesidades específicas, ya sea una aplicación descentralizada (dapp) o una billetera. Estos incluyen nodos completos, nodos ligeros y nodos de archivo. Cada tipo de nodo interpretará los datos de manera diferente y ofrecerá diferentes métodos de sincronización.

</details>

<details>

<summary><strong>Cómo se actualizan los nodos en la red Ethereum cuando hay una nueva versión del protocolo?</strong></summary>

Cuando hay una nueva versión del protocolo Ethereum, los nodos en la red deben actualizarse para seguir siendo compatibles con la red:

1. **Anuncio de la actualización**: La Ethereum Foundation anuncia la actualización en su blog oficial. Por ejemplo, la actualización de la red principal llamada “Dencun” se anunció para activarse en la época 269568, el 13 de marzo de 2024 a las 13:55 UTC.
2. **Actualización del software**: Los operadores de nodos y los participantes deben actualizar su software a las versiones enumeradas en el anuncio. Esto implica descargar e instalar la nueva versión del software del cliente Ethereum en su nodo.
3. **Sincronización con la red**: Una vez que se ha instalado la nueva versión del software, el nodo debe sincronizarse con el último estado de la red. Esto implica descargar datos de otros nodos (pares), verificar criptográficamente su integridad y construir una base de datos local de la cadena de bloques.
4. **Conexión a la red**: Cuando un nuevo nodo se une a la red Ethereum o se actualiza, necesita conectarse a nodos que ya están en la red para luego descubrir nuevos pares. Estos puntos de entrada en la red Ethereum se llaman nodos de arranque.

Es importante mencionar que mantener un nodo actualizado puede requerir un cierto nivel de conocimiento técnico y dedicación. Sin embargo, es una contribución valiosa para la red.

</details>
