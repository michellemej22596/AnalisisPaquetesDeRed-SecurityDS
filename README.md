# Laboratorio 5 – Análisis de tráfico de red
### Michelle Mejía Villela

Descripción

En este laboratorio se analizó un archivo de captura de red (.pcap) utilizando Python y la librería Scapy para extraer información relevante de los paquetes. Posteriormente, se aplicaron técnicas estadísticas y de machine learning para detectar anomalías en el tráfico, con el objetivo de identificar actividad sospechosa y comprender su comportamiento.

El flujo de trabajo simula el proceso realizado en un Security Operations Center (SOC):

1. Exploración de paquetes


2. Análisis estadístico


3. Detección de anomalías


4. Confirmación manual del contenido del tráfico



Se utilizaron métodos como Z-score y Isolation Forest para identificar paquetes anómalos en función del tamaño del payload.


---

Herramientas utilizadas

Python

Scapy

Pandas

Matplotlib

Scikit-learn



---

Resultados principales

Estadísticas básicas del tráfico

La IP más frecuente fue 10.1.10.53, la cual se comunicó principalmente con 84.54.22.33 utilizando el puerto 53 (DNS).

El puerto 53 corresponde al protocolo DNS, utilizado para resolver nombres de dominio en direcciones IP.

Sin embargo, se observó que el tamaño de varios payloads era cercano a 900–1000 bytes, lo cual no es común en consultas DNS normales.


---

Análisis con Z-score

Se aplicó el método de Z-score para detectar anomalías en el tamaño del payload.

Inicialmente, al calcular la media y desviación estándar a partir del dataset, no se detectaron anomalías. Esto ocurrió porque la distribución de los datos no era normal, sino bimodal, con muchos valores en 0 bytes y muchos valores cercanos a 900 bytes.

Cuando se utilizó conocimiento del dominio del protocolo DNS (media ≈ 50 bytes y desviación estándar ≈ 15 bytes), todos los paquetes fueron detectados como anómalos, ya que su tamaño era significativamente mayor al esperado.

Esto demuestra que el conocimiento del protocolo es fundamental para interpretar correctamente los resultados de los métodos estadísticos.


---

Gráficas de payload

Las gráficas mostraron que la mayor cantidad de datos fue enviada desde la IP 10.1.10.53 hacia 84.54.22.33 a través del puerto 53.

Esto sugiere una transferencia de datos concentrada en una sola comunicación, lo cual puede indicar comportamiento sospechoso.


---

Detección automática con Isolation Forest

El modelo Isolation Forest identificó 29 paquetes como anomalías, principalmente aquellos con payloads grandes (entre 900 y 1000 bytes).

Las anomalías detectadas coincidieron con la comunicación entre 10.1.10.53 y 84.54.22.33, confirmando que esta conversación presenta características inusuales para tráfico DNS.


---

Análisis del payload y tipo de ataque

Al inspeccionar el contenido del payload, se encontraron fragmentos que corresponden a la estructura de un archivo PNG (por ejemplo: encabezados como PNG, IHDR, IDAT).

Este tipo de contenido no es coherente con el comportamiento esperado del protocolo DNS, lo que indica que se están transmitiendo datos no relacionados con la resolución de nombres de dominio.

El comportamiento observado corresponde a un posible ataque de DNS tunneling o exfiltración de datos, en el cual se utiliza el protocolo DNS para transferir información de forma encubierta y evitar mecanismos tradicionales de detección.


---

Importancia de combinar técnicas automáticas y análisis manual

Las técnicas automáticas permiten identificar patrones anómalos en los datos, como tamaños de payload inusuales o comportamientos atípicos.

Sin embargo, el análisis manual del contenido del payload es necesario para confirmar la naturaleza del tráfico sospechoso y comprender qué tipo de información se está transmitiendo.

La combinación de ambos enfoques permite realizar un análisis más completo y confiable del tráfico de red.


---

Conclusión

El laboratorio permitió aplicar técnicas de análisis estadístico y machine learning para detectar anomalías en tráfico de red.

Se identificó tráfico sospechoso que aparentaba ser DNS legítimo, pero que contenía datos no correspondientes al protocolo, lo que sugiere una posible técnica de ocultamiento de información mediante DNS tunneling.

Este ejercicio demuestra la importancia de comprender el comportamiento normal de los protocolos de red para poder identificar amenazas de manera efectiva.
