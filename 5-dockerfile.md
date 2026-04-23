# Dockerfile
Un Dockerfile es un archivo de texto plano que contiene una serie de instrucciones que Docker utiliza para construir una imagen de contenedor Docker. Este conjunto de instrucciones define cómo se debe configurar y construir una imagen de contenedor, incluyendo qué sistema operativo base usar, qué software instalar, qué archivos copiar en el contenedor y cómo configurar el entorno de ejecución.
 ![Dockerfile](img/relacion.PNG)


Tradicionalmente, el archivo docker no tiene extensión. Simplemente se denomina Dockerfile sin ningún tipo de extensión. Adicionalmente, los Dockerfiles pueden ser creados usando la extensión .dockerfile. Esto se utiliza cuando hay una necesidad de almacenar múltiples archivos docker en un solo directorio.
Las instrucciones en un Dockerfile son simples y están diseñadas para ser leídas y comprendidas fácilmente. 

### FROM 
Especifica la imagen base que se utilizará como punto de partida para construir la nueva imagen de Docker. Por ejemplo, FROM nginx:alpine significa que la nueva imagen se basará en la imagen oficial de Nginx que está etiquetada como "alpine". Al utilizar una imagen base existente, se heredan todas las configuraciones y software incluidos en esa imagen, lo que facilita la creación de nuevas imágenes sin tener que empezar desde cero.

### RUN
Ejecuta comandos en el contenedor durante el proceso de construcción de la imagen. Puedes usar esta instrucción para instalar software, configurar el entorno, o realizar cualquier otra tarea necesaria para preparar la imagen. Por ejemplo, RUN apt-get update && apt-get install -y <paquete> instalará un paquete utilizando el administrador de paquetes apt en una imagen basada en Ubuntu.

### COPY
Copia archivos o directorios desde la máquina host al sistema de archivos del contenedor. Se utiliza para agregar archivos, scripts u otros recursos necesarios para la aplicación en la imagen de contenedor. Por ejemplo, COPY ./mi-app /app copiará el directorio mi-app desde la máquina host al directorio /app en el contenedor.

### ADD
Copia archivos o directorios desde el sistema de archivos de la máquina host al sistema de archivos del contenedor. Aunque es similar a la instrucción COPY, ADD tiene algunas características adicionales, como la capacidad de descomprimir automáticamente archivos y de copiar archivos desde una URL remota.

### CMD 
Define el comando predeterminado que se ejecutará cuando se inicie el contenedor. Puede haber solo una instrucción CMD en un Dockerfile. Si se proporciona más de una, solo la última tendrá efecto. Por ejemplo, CMD ["nginx", "-g", "daemon off;"] ejecutará el servidor web Nginx en modo daemon cuando se inicie el contenedor.

### ENTRYPOINT
Configura el comando o el script que se ejecutará cuando se inicie un contenedor basado en la imagen. A diferencia de la instrucción CMD, que especifica el comando predeterminado que se ejecutará y que puede ser sobrescrito desde la línea de comandos al iniciar el contenedor, ENTRYPOINT establece un comando que no se puede sobrescribir fácilmente al iniciar el contenedor.
Es importante destacar que, si se proporciona un comando en la línea de comandos al iniciar el contenedor (por ejemplo, docker run <imagen> <comando>), este comando se agregará como argumentos al comando especificado en ENTRYPOINT, en lugar de sobrescribirlo. Esto permite que el contenedor sea más versátil y se adapte a diferentes necesidades de uso.

### EXPOSE
Informa a Docker que el contenedor escuchará en un puerto específico en tiempo de ejecución. No abre realmente el puerto, solo documenta que la aplicación dentro del contenedor puede usar dicho puerto. Por ejemplo, EXPOSE 80 expone el puerto 80 en el contenedor, lo que permite que se acceda a la aplicación que se esté ejecutando en ese puerto desde fuera del contenedor.

### ENV
Define variables de entorno dentro del contenedor. Las variables de entorno definidas con ENV estarán disponibles para cualquier proceso en el contenedor durante su ejecución. Por ejemplo, ENV MYSQL_ROOT_PASSWORD=password define una variable de entorno llamada MYSQL_ROOT_PASSWORD con el valor password.

### VOLUME 
Esta instrucción se se utiliza para definir un punto de montaje para volúmenes dentro del contenedor.

##  Para construir una imagen de Docker a partir de un Dockerfile
```
docker build -t <nombre imagen>:<tag> .
```
- **-t** esta opción se utiliza para etiquetar la imagen que se está construyendo con un nombre y una versión.
- <nombre_imagen>:<tag> por ejemplo: myapp:1.0
- **.** este punto indica al comando docker build que busque el Dockerfile en el directorio actual, es decir especifica la ubicación del contexto de la construcción que incluye el Dockerfile y cualquier otro archivo necesario para la construcción de la imagen.

## Ejemplo
### Colocar las siguientes instrucciones en un Dockerfile, 
![Dockerfile](img/Dockerfile.PNG)
- apachectl: Es el script de control para el servidor web Apache. Se utiliza para iniciar, detener y controlar el servidor web.
- -D FOREGROUND: Esta opción le dice a Apache que se ejecute en primer plano. Por defecto, Apache se ejecuta como un servicio en segundo plano. Sin embargo, en un contenedor Docker, es preferible que el proceso principal (en este caso, Apache) se ejecute en primer plano para que Docker pueda monitorear el estado del proceso. Si Apache se ejecutara en segundo plano, Docker no podría saber si el servidor web está funcionando correctamente o no.

 ## IMPORRANTE 
### CentOS 7 llegó al final de su vida útil (End of Life - EOL) el 30 de junio de 2024. Como resultado, los servidores de espejos oficiales (mirrorlist.centos.org) fueron desactivados, y el comando yum ya no puede encontrar los repositorios para descargar actualizaciones o software. Por lo tanto ejecutar el Dockerfile asi como se indica el comando resultaría en un error, Para que yum funcione en CentOS 7, debes ejecutar unos comandos sed que cambien las rutas de los repositorios hacia vault.centos.org, que es donde se guarda el software antiguo de forma permanente.
Dockderfile nuevo
```
FROM centos:7

# Estos comandos cambian los repositorios fallidos por el archivo histórico (Vault)
RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-* && \
    sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*

RUN yum -y update
RUN yum -y install httpd

COPY ./web /var/www/html

EXPOSE 80

CMD ["apachectl", "-D", "FOREGROUND"]
```

### Ejecutar el archivo Dockerfile y construir una imagen en la versión 1.0
No olvides verificar en qué directorio se encuentra el archivo Dockerfile
```
docker build -t myapp:1.0 .
```

**¿Cuántos pasos se han ejecutado?**
<img width="1919" height="492" alt="image" src="https://github.com/user-attachments/assets/da58feb3-cf79-44f2-ab58-59a4e91a0698" />
Como se observa en el terminal, se han ejecutado 5 pasos lógicos, que son las instrucciones o capas definidas directamente en el Dockerfile (del [1/5] al [5/5]), y 11 operaciones técnicas totales, las cuales abarcan tareas administrativas internas del motor BuildKit, como la carga de metadatos, la transferencia del contexto de construcción y la exportación final de la imagen.
# RESPONDER 

### Inspeccionar la imagen creada
# COMPLETAR CON UNA CAPTURA
<img width="1913" height="1042" alt="image" src="https://github.com/user-attachments/assets/128afcf8-f22e-467c-bf58-a682b5f1e460" />

**Modificar el archivo index.html para incluir su nombre y luego crear una nueva versión de la imagen anterior**
**¿Cuántos pasos se han ejecutado? ¿Observa algo diferente en la creación de la imagen**
<img width="1919" height="492" alt="image" src="https://github.com/user-attachments/assets/bce2022f-38da-47d1-a1bb-d90c3374549f" />

En este proceso de construcción se observan los mismos pasos que antes, pero ahora antes de los primeros 4 pasos lógicos (instrucciones del Dockerfile) aparece la palabra CACHED, y el tiempo de ejecucion es 0.0 segundos, esto significa que, como no hubo cambios en el Dockerfile ni en la imagen base hasta ese punto, Docker reutilizó las capas que ya habías construido previamente para ahorrar tiempo y recursos, pero como el paso 5 (que es de copiar el directorio modificado) es un paso nuevo, entonces no aparece la palabra CACHED.

## Mecanismo de caché
Docker usa un mecanismo de caché cuando crea imágenes para acelerar el proceso de construcción y evitar la repetición de pasos que no han cambiado. Cada instrucción en un Dockerfile crea una capa en la imagen final. Docker intenta reutilizar las capas de una construcción anterior si no han cambiado, lo que reduce significativamente el tiempo de construcción.

- Instrucción FROM: Si la imagen base ya está en el sistema, Docker la reutiliza.
- Instrucciones de configuración (ENV, RUN, COPY, etc.): Docker verifica si alguna instrucción ha cambiado. Si no, reutiliza la capa correspondiente de la caché.
- Instrucción COPY y ADD: Si los archivos copiados no han cambiado, Docker reutiliza la capa de caché correspondiente.
![mapeo](img/dockerfile-cache.PNG)


### Crear un contenedor a partir de las imagen creada, mapear todos los puertos
```
docker run -d --name mi-app-auto -P myapp:1.1
```
Con la utilización de la bandera -P (mayúscula), Docker mapeará todos los puertos que fueron declarados con la instrucción EXPOSE en el Dockerfile a puertos aleatorios disponibles en mi máquina host.

### ¿Con que puerto host se está realizando el mapeo?
# COMPLETAR CON LA RESPUESTA
<img width="1185" height="126" alt="image" src="https://github.com/user-attachments/assets/6f412d0a-ca19-476e-8efe-b4d118c0cfbb" />
Con el puerto 32768.

**¿Qué es una imagen huérfana?**
# COMPLETAR CON LA RESPUESTA
Una imagen huérfana (o dangling image) es básicamente una imagen de Docker que se ha quedado sin nombre ni etiqueta, apareciendo usualmente como <none>:<none> en el sistema. Esto sucede casi siempre cuando se construye una nueva versión de una imagen usando el mismo nombre que una que ya existía; la versión antigua pierde su identificación para "cederle" el nombre a la nueva y se queda olvidada ocupando espacio en el disco duro sin estar conectada a ningún contenedor activo. En pocas palabras, es basura digital que queda tras actualizar proyectos y que se puede borrar tranquilamente para recuperar almacenamiento en la computadora.

### Identificar imágenes huérfanas
```
docker images -f "dangling=true"
```

### Listar los IDS de las imágenes huérfanas
```
docker images -f "dangling=true" -q
```

### Eliminar imágenes huérfanas
Este comando eliminará todas las imágenes que no estén asociadas a ningún contenedor en ejecución. Antes de ejecutarlo, asegúrate de revisar las imágenes que serán eliminadas para evitar la pérdida de imágenes importantes. 
```
docker image prune
```

### Para Ejecutar un archivo Dockerfile que tiene otro nombre
```
docker build -t <nombre imagen>:<tag> -f <ruta y nombre del Dockerfile> .
```

## Por ejemplo
docker build -t imagen:1.0 -f Dockerfile-custom .



