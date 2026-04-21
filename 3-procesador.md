# Limitar uso de procesador
Limitar la cantidad de núcleos de CPU:
```
--cpus=<número de núcleos>
```

Asignar núcleos de CPU específicos:
```
--cpuset-cpus=<lista de núcleos>
```

**¿Como saber el numero de procesadores virtuales que tiene una máquina?**
## COMPLETAR

En sistemas basados en Linux se puede usar el comando:
```
nproc
```
Dicho comando da como respuesta el numero total de hilos (procesadores virtuales) disponibles para el sistema.

En win11 se puede utilizar el administrador de tareas para tener está información de manera visual como se presenta a continuacion:
<img width="1414" height="755" alt="image" src="https://github.com/user-attachments/assets/f1a0ab43-ba4f-4bdd-afab-3c4c7749c7eb" />
En donde Núcleos se refiere a los núcleos físicos que tiene el procesador y Procesadores lógicos se refieren a los núcleos virtuales.
## Ejemplos
_Puedes copiar y ejecutar directamente cada uno de los comandos_

Limitar el uso de CPU a 1.5 núcleos
```
docker run -d --name server-nginx --cpus="1.5" nginx:alpine
```

Restringir el contenedor para que use los núcleos de CPU 0 a 2:
```
docker run -d --name server-nginx --cpuset-cpus="0-2" nginx:alpine
```

Restringir el contenedor para que use los núcleos de CPU 1 y 3:
```
docker run -d --name server-nginx --cpuset-cpus="1,3" nginx:alpine
```
<img width="1042" height="361" alt="image" src="https://github.com/user-attachments/assets/18e930ad-3f13-40ee-8e50-19ae54e1fec5" />
