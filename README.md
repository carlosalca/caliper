# Hyperledger Caliper

Hyperledger Caliper es una herramienta del proyecto Hyperledger dentro de la Linux Fundation. Permite medir el rendimiento de una red blockchain, que en este caso sería una red blockchain de Hyperledger Fabric. Actualmente da soporte a:

* Hyperledger Burrow
* Hyperledger Composer
* Hyperledger Fabric
* Hyperledger Iroha
* Hyperledger Sawtooh

Aunque quizá el más relevante sea dar soporte a Hyperledger Fabric, y también que tenga planeado dar soporte a otras blockchain como Ethereum. Los indicaodres de rendimiento que se pueden obtener se pueden observar en la siguiente tabla:

| Inidicadores de rendimiento |  Descripción                                                         |
|-----------------------------|----------------------------------------------------------------------|
| Ratio de éxito              | Número de transacciones llevadas a cabo con éxito y número de fallos |
| Throughput de transacciones | Número de transacciones por segundo                                  |
| Latencia                    | Latencia mínima, máxima y media                                      |
| Consumo de recursos         | Consumo de memoria, de CPU, I/O de red                               |

La arquitectura de Hyperledger Caliper se puede consultar en [1], pero básicamente se trata de una serie de capas:

* Capa de adaptacación: integra la blockchain existente, como puede ser Hyperledger Fabric, con Caliper. El adaptador para cada red implementa una interfaz que Caliper denomina "Caliper Blockchain NBI (North Bound Interface)" usando el SDK nativo de cada red blockchain. En [2] se puede encontrar documentación sobre cómo crear un adaptador.
* Capa de interfaz y núcleo: en esta capa se encuentran tanto la interfaz NBI como los medidores de los indicadores de recursos. 
* Capa de aplicación: contiene el test a realizar sobre la red. Caliper denomina a este test "benchmark" y usa en "benchmark engine" para llevarlo a cabo. El benchmark engine se puede observar en la siguiente imagen:

![Benchmark engine](/images/test-framework.png)
*Figure 1: ![Benchmark engine](url)*

Como se puede apreciar, al benchmark le entran dos archivos de configuración en los cuales se definen la red blockchain en uno y las pruebas a realizar en el otro.

## Benchamark configuration file

Mediante este archivo de configuración se pueden configurar las pruebas a realizar sobre la red de blockchain, como por ejemplo el número de transacciones o el ritmo al que se inyectan estas en la red. Se trata de un archivo ".yaml" en el que se van a distinguir dos zonas principales, una primera zona "test" en la que se indica la información sobre las pruebas a realizar, y una segunda zona "monitor" en la que se define qué se va a monitorizar durante las pruebas. Un ejemplo de este archivo se puede encontrar [aquí](/benchmark/config.yaml).

Los aspectos más importantes de este archivo son quizá los que definen las "rounds" que se van a hacer. Cada ronda se identifica con un "label", que normalmente suele ponerse el nomnbre de la función del chaincode que se va a evaluar. 

```
rounds:
  - label: open
    txMode:
      type: real-time
    txNumber:
    - 5000
    - 5000
    rateControl:
    - type: fixed-rate
      opts: 
        tps: 100
    - type: fixed-rate
      opts:
        tps: 200
    arguments:
      money: 10000
    callback: benchmark/simple/open.js
```













[1] Hyperledger Caliper. (2019). Architecture -. [online] Available at: https://hyperledger.github.io/caliper/docs/2_Architecture.html [Accessed 12 Jul. 2019].

[2] Hyperledger Caliper. (2019). Writing Adapters. [online] Available at: https://hyperledger.github.io/caliper/docs/Writing_Adapters.html [Accessed 12 Jul. 2019].
