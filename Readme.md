# Consultando VPC Flow Logs desde Cloudwatch Insights

## Consulta # 1 - Listado de Logs

* **Campos del log:** @timestamp y @message.
* **Resultado:** Ordenado descendentemente por el campo @timestamp.
* **Límite de logs a mostrar:** 20 

```
fields @timestamp, @message
| sort @timestamp desc
| limit 20
```

## Consulta # 2 - Filtrado de Logs

* **Campos del log:** @timestamp y @message.
* **Filtro:** el campo mensaje tenga algo como la dirección IP 10.100.100.36 (no es case sensitive) 
* **Resultado:** Ordenado descendentemente por el campo @timestamp.
* **Límite de logs a mostrar:** 20 

```
fields @timestamp , @message
| filter @message like /(?i)10.100.100.36/ 
| sort @timestamp desc
| limit 20
```

## Consulta # 3 - Filtrado con Estadística

* **Estadística:** Contar la cantidad (numRejections) de rechazos por IP Origen (srcAddr).
* **Filtro:** se mostrarán solo los mensajes rechazados. 
* **Resultado:** Ordenado descendentemente por el campo numRejections (cantidad de rechazos).
* **Límite de logs a mostrar:** 20 

```
filter action="REJECT"
| stats count(*) as numRejections by srcAddr
| sort numRejections desc
| limit 20
```


## Consulta # 4 - Estadística

* **Estadística:** Cantidad (bytesTransferred) de bytes transferidos de por IP Origen (srcAddr) a IP Destino (dstAddr).
* **Resultado:** Ordenado descendentemente cantidad de bytes transferido.
* **Límite de logs a mostrar:** 10 
```
stats sum(bytes) as bytesTransferred by srcAddr, dstAddr
| sort bytesTransferred desc
| limit 10
```


## Consulta # 5 - Tabla para Dashboard # 1

* **Tabla:** Promedio, mínimo y máximo de bytes por dirección origen y destino.
```
stats avg(bytes), min(bytes), max(bytes) by srcAddr, dstAddr
```


## Consulta # 6 - Tabla para Dashboard # 2

* **Tabla:** Promedio, mínimo, máximo, cantidad de paquetes por dirección origen, destino, puerto origen y destino.
```
stats avg(bytes), min(bytes), max(bytes), max(packets) by srcAddr, dstAddr, srcPort, dstPort
```

