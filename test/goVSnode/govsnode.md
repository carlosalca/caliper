# Eficiencia de Go vs Node.js en Hyperledger Fabric con Hyperledger Caliper

En este conjunto de pruebas se comparan los chaincode escritos en Go y Node.js, con el fin de averiguar cuál es más eficiente y cuál sería el más idóneos a la hora de implementar una solución.

## Primera prueba

En la primera prueba se ha realizado el test descrito en este [benchmark](/test/goVSnode/1/benchmark/configVS.yaml). Principalmente lo que hace es realizar dos rondas para cada lenguaje con el mismo 
chaincode escrito en Go y en Node.js. Las transacciones inyectadas son las mismas para los dos y son las que se reflejan en la siguiente tabla:

|       	| Go                         	| Node.js                    	|
|-------	|----------------------------	|----------------------------	|
| Init  	| 500 transacciones a 25 tps 	| 500 transacciones a 25 tps 	|
| Query 	| 15 transacciones a 5 tps   	| 15 transacciones a 5 tps   	|

El informe completo generado por Hyperledger Caliper se puede descargar de [aquí](/test/goVSnode/1/report-20190712T113010.html).

Los resultados más distintos se pueden observar en la latencia, siendo más eficiente en este aspecto el chaincode escrito en Node.js que el escrito en Go. Los resultados de la latencia se pueden
observar en el siguiente gráfico:

![Latency comparison chart](/test/goVSnode/1/images/1_latency.png)

Sin embargo, a pesar de que haya diferencias en los resultados de la latencia, los resultados en las transacciones por segundo no son muy disntintos:

![Throughput comparison chart](/test/goVSnode/1/images/1_tps.png)
