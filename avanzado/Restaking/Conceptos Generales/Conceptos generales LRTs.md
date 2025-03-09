# Conceptos generales del restaking y los LRTs.

# 6. De los LSTs a los LRTs

## 6.1. LSTs (Liquid Staking Tokens)

Para repasar esto, retomemos el caso del **staking delegado**.

![Figura 11](Pepe%201.png)

Figura 11

- **Lola** → La *staker*.
- **Pepe** → El Proveedor de Servicios de Staking (SSP).

### **6.1.1.Flujo del staking delegado:**

En la siguiente animación se puede ver un *modelo de ejemplo* de como es el flujo del staking delegado:

![Figura 12](1_(1).gif)

Figura 12

<aside>
⚠️

Es importante destacar que *noETH* y *soETH* **no son tokens reales** que existan en la blockchain. Son simplemente **figuras teóricas** que nos permiten visualizar de manera más clara el flujo de activos dentro del proceso de staking, pero en la práctica, no existen contratos ni balances en la red con esos nombres.

</aside>

1. Lola deposita ETH nativo en el SSP, con el propósito de que sea *stakeado* en Ethereum.
2. A cambio, el SSP le entrega un “*activo*”, que en este ejemplo llamamos noETH.
    - Este *noETH* representa el ETH que Lola ha delegado mas las recompensas que va generando.
    - No es transferible ni fungible, es decir, solo sirve para que Lola pueda redimir su ETH en el futuro.
    - El término "redimir" (*redeemable*) introduce cierta incertidumbre. ¿Es segura una tasa 1:1 de *noETH* por ETH?   En general, no. Si ocurre un *slashing*, el saldo del validador del SSP se reduce, y como *noETH* representa una fracción de ese saldo, Lola podría recibir menos ETH del que delegó.
3. El SSP stakea el ETH el Ethereum.
4. El SSP obtiene control sobre lo que llamamos en este ejemplo *soETH*. 
    - *soETH* y *noETH* no son identicos, ya que el SSP cobra una comisión por sus servicios.

### **6.1.2. Liquidez del staking delegado  “LSTs”:**

Acabamos de hablar de *noETH* como el derecho que tiene Lola sobre su ETH delegado en el SSP, pero en esta explicación, este activo **no es ni transferible ni fungible**. Es simplemente un registro interno que le dice a Lola cuántos ETH tiene en *staking* bajo la gestión del SSP.

Un buen ejemplo de esto es lo que ocurría antes de que existiera [cbETH](https://coinmarketcap.com/currencies/coinbase-wrapped-staked-eth/). Cuando alguien hacía *staking* delegado en Coinbase, obtenía una reclamación virtual (noETH) representada por una línea en el libro contable interno de la base de datos de Coinbase, pero esa reclamación no existía en la blockchain.

Ahora bien, si queremos que Lola pueda hacer algo con su *stake* sin esperar a retirarlo, necesitamos hacer *líquida* esta posición. Esto se logra emitiendo en la blockchain un token que represente su derecho sobre *noETH*, un proceso que convierte el *stake* en un activo líquido.

Al convertir *noETH* en un Liquid Staking Token (LST), este activo pasa a ser fungible y transferible, lo que significa que Lola puede venderlo, usarlo como colateral en DeFi o intercambiarlo libremente. Esta es la idea detrás de tokens como stETH, cbETH, o cualquier otro LST que conocemos en el ecosistema. 

El caso estudiado del flujo de staking delegado que brinda un LST puede verse en la siguiente animación:

![Figura 13](Copia_de_1_(1).gif)

Figura 13

Algunos  ejemplos de uso de LST:

- En el dashboard de AAVE podemos ver que se pueden usar de colateral.

![Figura 14](image%202.png)

Figura 14

- En Uniswap podemos ver como convertir LSTs en ETH nativo o cualquier otro token.
    
    <aside>
    📖
    
    stETH es el LST de Lido. Para poder operar en DEFI con estos tokens es usual verlos en su forma wrapped, de ahí la letra “w” de wstETH. 
    
    </aside>
    

![Figura 15](image%203.png)

Figura 15

![Figura 16](image%204.png)

Figura 16

## 6.2. Restaking de un LST:

Desde su creación, **EigenLayer** ha permitido que un titular de LST deposite su LST y lo coloque en *stake* dentro de su protocolo. Este es uno de los casos del restaking delegado no nativo explicado en 3.4. Analizamos este caso particular:

![Figura 17](restakeLST.png)

Figura 17

<aside>
📖

*noLST* y *soLST* **no son tokens reales** que existan en la blockchain. Son simplemente **figuras teóricas** que nos permiten visualizar de manera más clara el flujo de activos dentro del proceso de restaking, pero en la práctica, no existen contratos ni balances en la red con esos nombres.

</aside>

En este caso, Lola quiere utilizar para el restaking los LSTs que están en su poder para participar del restaking. 

El hecho de que Lola coloque el LST en *staking* dentro del protocolo de restaking no obliga al SSP que emitió el LST a participar en la provisión de servicios para los **AVS**. De hecho el **SSP**  ni siquiera es consciente de que sus tokens han sido *stakeados* en un protocolo de restaking.

Como dijimos ya anteriormente en la seccion 3.4.:

<aside>
📖

La única razón por la que a esto se le llama *restaking* es porque el AVS también permite las otras las formas de restaking, pero técnicamente, lo que Lola está haciendo aquí se parece más a un *staking* normal de sus LST que a un *restaking*.

</aside>

Lola esta actuando como proveedor de capital para el AVS (ver 5.1.2). Analicemos el paso a paso:

1. Lola deposita LST en el protocolo de restaking, con el propósito de que sea re*stakeado*.
2. A cambio, el protocolo le entrega un “*activo*”, que en este ejemplo llamamos noLST.
    - Este *noLST* representa el LST que Lola ha delegado mas las recompensas que va generando.
    - No es transferible ni fungible, es decir, solo sirve para que Lola pueda redimir su LST en el futuro.
    - La tasa noLST y LST no es 1:1. Si ocurre una penalización en el protocolo del AVS el saldo  del protocolo de restaking se reduce, y como *noLST* representa una fracción de ese saldo, Lola podría recibir menos LST del que delegó.
3. El protocolo de restaking delega el LST al AVS quien lo utilizara en nombre de Lola en la red X (usamos el nombre X para dejar en claro que no es Ethereum, es la red que segura el AVS)
4. El protocolo de restaking obtiene control sobre lo que llamamos en este ejemplo *soLST*. El valor de soLST  puede aumentar a medida que la red **X** remunera los servicios de validación del operador del AVS. 
    - *soLST* y *noLST* no son identicos, ya que el protocolo de restaking cobra una comisión por sus servicios.

Concluimos destacando que, en el caso de un **LST restakeado**, existen dos entidades lógicamente distintas:

1. **El SSP**, que emite el LST.
2. **El AVS**, que realiza la validación.

Esta separación de roles podría, en el futuro, impulsar una mayor descentralización. Sin embargo, en la actualidad, esto se logra a costa de introducir múltiples delegaciones de confianza.

## 6.3. LRTs

Ahora podemos adentrarnos en *algunas* de las distintas formas en las que pueden existir activos líquidos que representen ETH en *restaking*: los **Liquid Restaking Tokens (LRTs)**. En esencia, el objetivo de los *LRTs* es el mismo que el de los *LSTs*: contar con activos fungibles y transferibles que generen recompensas, pero esta vez provenientes del *restaking*.

### 6.3.1. LRTs a partir de delegated restaking:

En esta primera construcción consideramos un SSP haciendo *re-staking* con el *stake* que tiene actualmente bajo su control. Veamos el siguiente flujo:

<aside>
⚠️

*soETH* y r*soETH* **no son tokens reales** que existan en la blockchain. Son simplemente **figuras teóricas** que nos permiten visualizar de manera más clara el flujo de activos dentro del proceso de restaking, pero en la práctica, no existen contratos ni balances en la red con esos nombres.

</aside>

![Figura 18](Copia_de_1_(2).gif)

Figura 18

1. Lola deposita ETH nativo en el SSP, con el propósito de que sea re/*stakeado* en Ethereum.
2. A cambio, el SSP le entrega el LRT 
3. Dado que todo queda registrado *on-chain*, sería muy fácil detectar si un SSP hizo *restaking o no* con el ETH. ****El LRT entonces representa el derecho a reclamar el ETH inicial mas la doble recompensa siendo además transferible y fungible.
4. El SSP stakea el ETH el Ethereum (IDEM 6.1.1.)
5. El SSP obtiene control sobre *soETH (*IDEM 6.1.1.)
6. El SSP restakea el soETH al AVS (protocolo de restaking).
7. El SSP recibe lo que llamamos rsoETH que es el “activo” que se va a utilizar en caso de querer reclamar sobre el ETH *restakeado*.

Vemos que la utilidad de los *LRTs* es similar a la de los *LSTs*, pero, como mencionamos antes, más recompensa implica más riesgo. Ahora, el *LRT* no solo está expuesto a los riesgos del *staking*, sino también a los del *restaking*.

Además, al ser un sistema más complejo, podrían darse situaciones atípicas de mal comportamiento. Por ejemplo, el SSP podría tener acuerdos *off-chain* con algún AVS, generando recompensas que no llegan al poseedor del LRT, pero sin embargo si lo exponen a los riesgos asociados al *restaking*.

### 6.3.2. LRT a partir del restaking de LSTs:

Es una extensión de lo que analizamos en el restaking de los LSTs en 7.2.

Podemos sumar un participante nuevo a la escena, el agregador. 

<aside>
⚠️

*aLSTs, noaLSTs* y *soaLSTs* **no son tokens reales** que existan en la blockchain. Son simplemente **figuras teóricas** que nos permiten visualizar de manera más clara el flujo de activos dentro del proceso de restaking, pero en la práctica, no existen contratos ni balances en la red con esos nombres.

</aside>

![Figura 19](Copia_de_Copia_de_1.gif)

Figura 19

1. Lola y Alice que son LSTs holders, depositan en el agregador sus LSTs.
2. Lola y Alice reciben los LRTs emitidos por el agregador. 
3. El agregador deposita en el Protocolo de Restaking los tokens que recibió de Alice y Lola, lo llamamos aLSTs.
4. A cambio, el protocolo le entrega un “*activo*”, que en este ejemplo llamamos noaLSTs.
    - Este *noaLST* representa el aLST que el agregador ha delegado mas las recompensas que va generando.
    - No es transferible ni fungible, es decir, solo sirve para que el agregador pueda redimir su aLSTs en el futuro.
    - La tasa noaLSTs y aLSTs no es 1:1. Si ocurre una penalización en el protocolo del AVS el saldo  del agregador se reduce, y como *noaLSTs* representa una fracción de ese saldo, el agregador podría recibir menos aLSTs del que delegó.
5. El protocolo de restaking delega el aLSTs al AVS quien lo utilizara en nombre del agregador en la red X (usamos el nombre X para dejar en claro que no es Ethereum, es la red que segura el AVS)
6. El protocolo de restaking obtiene control sobre lo que llamamos en este ejemplo *soaLSTs*. El valor de soaLSTs  puede aumentar a medida que la red **X** remunera los servicios de validación del operador del AVS. 
    - *soaLSTs* y *noaLSTs* no son idénticos, ya que el protocolo de restaking cobra una comisión por sus servicios.

# 7. Conclusiones

En este artículo, exploramos de manera general los métodos más utilizados de *restaking*, centrándonos en qué son y cómo se relacionan entre sí, sin entrar en los detalles técnicos sobre cómo se implementan. Analizamos los flujos de *staking* y *restaking*, distinguiendo las distintas formas en que los activos pueden ser utilizados para proporcionar seguridad a múltiples protocolos. También examinamos las diferencias fundamentales entre los LSTs y los LRTs, resaltando sus similitudes en términos de utilidad, pero también sus diferencias en cuanto a riesgos y exposición a múltiples niveles de validación.

Al revisar estos esquemas, vimos que muchos de estos mecanismos funcionan de manera similar entre sí. Es decir, aunque los nombres y estructuras puedan variar, en esencia, cumplen funciones equivalentes dentro del ecosistema. Por lo tanto, lo más importante no es conocer todas las variantes posibles, sino comprender quién tiene el control final de los activos utilizados en *restaking* y cómo esto afecta la seguridad y descentralización de los sistemas que dependen de ellos.

También discutimos los riesgos asociados, como la delegación de confianza en actores específicos, la posibilidad de acuerdos *off-chain* que afectan a los poseedores de LRTs y la creciente complejidad de los sistemas basados en *restaking*.

En el próximo artículo, daremos un paso más allá y nos enfocaremos en las implementaciones reales de *restaking*. Explicaremos cómo funcionan en la práctica y analizaremos su estructura técnica, apoyándonos en los conceptos que desarrollamos aquí.

# 8. Referencias

[Semantics of Staking 1: Liquefaction](https://mirror.xyz/barnabe.eth/v7W2CsSVYW6I_9bbHFDqvqShQ6gTX3weAtwkaVAzAL4)

[Semantics of Staking 2: Re-staking](https://mirror.xyz/barnabe.eth/96MD_A194uXLLjcOWePW3O2N3P-JG-SHtNxU0b40o50)

[Semantics of Staking 3: Advanced Constructions](https://mirror.xyz/barnabe.eth/62E79gUSqiwS9NEbbfdwTdy7G9Hh098fcV38vWv8VQo)

[EigenLayer Will Change Ethereum Forever](https://www.youtube.com/watch?v=HcEGXoC57Rw&t=2s)

[The risks of LRTs](https://ethresear.ch/t/the-risks-of-lrts/18799)

[Explanation of Restaking](https://www.youtube.com/watch?v=rOJo7VwPh7I&t=1s)
