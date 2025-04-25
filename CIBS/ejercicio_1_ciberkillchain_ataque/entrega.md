# Ejercicio CiberKillChain - Ataque
# Alumno
Pablo Horacio Salas
# Sistema victima 
### Sistema de monitoreo de variables ambientales y alerta temprana
El “Sistema de monitoreo de variables ambientales y alerta temprana” tiene como objetivo proteger ecosistemas mediante la medición de factores ambientales
relacionados a la salud y el bienestar de las personas. Este sistema emplea diversos sensores para capturar las variables en el medio ambiente
y desplegar una red de comunicaciones de largo alcance (LoRaWAN). Además, permitirá
visualizar estos valores en una interfaz web y generar una alerta en caso de que se excedan
ciertos umbrales preestablecidos. Los componentes del proyecto incluyen sensores asequibles,
redes IoT eficientes, una plataforma de procesamiento de datos en tiempo real y sistemas de
alerta temprana.

Los datos recolectados son enviados a un sistema de procesamiento basado en la nube, con una base de datos alojada en Google Cloud, lo que permite escalabilidad y alta disponibilidad.

Además, el sistema permite visualizar estos valores a través de una interfaz web, que está alojada en un servidor interno. Los usuarios acceden a la plataforma a través de un navegador, donde pueden ver los datos en tiempo real y recibir alertas si se exceden ciertos umbrales preestablecidos, lo que les permite tomar medidas preventivas para proteger el ecosistema y la salud pública.

[Link del trabajo práctico](https://drive.google.com/file/d/18wzF2KUW19Co_6zkKrUzV4OzF9rQ69Qz/view?usp=sharing)

# Objetivo
Falsificar datos ambientales para evitar sanciones regulatorias.

# Resolución

## 1- Reconnaissance (Reconocimiento)
[Reconnaissance - TA0043](https://attack.mitre.org/tactics/TA0043/)

#### Técnicas utilizadas
[T1598 - Gather Victim Network Information](https://attack.mitre.org/techniques/T1598/):
Identificar la infraestructura del sistema incluyendo servidores, APIs y bases de datos en la nube.

Se identifica que el sistema posee una base de datos en Google Cloud y una API REST para la recolección y visualización de datos ambientales.
Herramienta útil: dirsearch para enumerar rutas del servidor.

[T1595 - Active Scanning](https://attack.mitre.org/techniques/T1595/): Buscar puntos debiles en la interfaz web (falta de cifrado, APIs inseguras).

Se analiza el dominio de la interfaz web probando endpoints conocidos de la API pública. Se descubre un endpoint vulnerable que no requiere autenticación y es susceptible a inyeccion SQL. 
Herramienta útil: sqlmap para automatizar la detección de vulnerabilidades de inyección SQL.

#### Descripción 
La combinación de ambas técnicas permiten detectar una API vulnerable a inyección SQL que permite acceso no autorizado a la base de datos de registros ambientales.  


## 2- Weaponization (Armamento)

[Resource Development - TA0042](https://attack.mitre.org/tactics/TA0042/)

#### Técnicas utilizadas

[T1587.001 - Develop Malware](https://attack.mitre.org/techniques/T1587/001/): crear un malware para modificar para interceptar y modificar los datos que se envian de los sensores a la base de datos.


[T1608.001 - Upload Malware](https://attack.mitre.org/techniques/T1608/001/): subir software malicioso a un lugar controlado por el atacante. 

#### Descripción 
Se desarrolla un script en Python que automatiza la explotación de la API vulnerable, permitiendo modificar los valores reportados por los sensores en la base de datos.
El script incluye un bloque de código para sobreescribir registros de CO2, PH de agua residual y otros valores clave en tiempo real.
Se utiliza una VPS (virtual private server) para tener una posicion de ataque anonima.


## 3 - Delivery (Entrega)
[Initial access - TA0001](https://attack.mitre.org/tactics/TA0001/)

#### Técnica utilizada

[T1190 - Exploit Public-Facing Application](https://attack.mitre.org/techniques/T1190/): acceder a la base de datos (si hay inyección SQL vulnerable o autenticacion debil).

#### Descripción 
Se utiliza un script de Python para **enviar peticiones de HTTP** a la API REST identificada durante la fase de reconocimiento. El endpoint (por ejemplo: /update_sensor/data) permite recibir parámetros como sensor_id, valor, y timestamp sin validación ni autenticación.


## 4 - Exploit (Explotación)
[Execution - TA0002](https://attack.mitre.org/tactics/TA0002/)

#### Técnica utilizada

[CWE-89 – SQL Injection](https://cwe.mitre.org/data/definitions/89.html)

#### Descripción 
Se aprovecha la falta de verificación en los parametros de la API, se logra ejecutar instrucciones SQL directamente contra la base de datos. Esto permite al atacante acceder, modificar o eliminar datos de la base de datos sin autorización.

    
## 5 - Installation (Instalación)
[Persistence - TA0003](https://attack.mitre.org/tactics/TA0003/)
#### Técnicas utilizadas

[T1505.003 - Server Software Component: Web Shell](https://attack.mitre.org/techniques/T1505/003/): Instalar web shells en el servidor para mantener acceso persistente.

[T1546 - Event Triggered Execution](https://attack.mitre.org/techniques/T1546/):Crear procesos automáticos para modificar datos ambientales periódicamente.

#### Descripción 
Se sube una web shell (por ejemplo shell.php) mediante un endpoint debilmente protegido, lo que permite volver a acceder al servidor cuando sea necesario.
Se programa **tareas cron** en el servidor (servicio del sitema operativo que ejecuta comandos en segundo plano a una hora y frecuencia determinada).
El uso de estas 2 técnicas asegura que incluso si se reinicia el sistema, las manipulaciones continúen.
    
## 6 - Command & control (Mando y control)
[Command and control - TA0011](https://attack.mitre.org/tactics/TA0011/)

#### Técnica utilizada
[T1071.001 - Application Layer Protocol: Web Protocols](https://attack.mitre.org/techniques/T1071/):uso de protocolos web (como HTTP/HTTPS) para la comunicación de comando y control (C2)

#### Descripción 
Se configura el script para que se comunique con un servidor HTTP que se controla desde una computadora personal, para que este servidor sea accesible desde internet se podria usar una herramienta como nrgok.
El servidor HTTP aloja un conjunto de respuestas predefinidas con datos ambientales falsificados.
El script malicioso que interactúa con la API vulnerable está programado para obtener periódicamente estos valores falsos desde el servidor HTTP, simulando datos reales.
De esta forma se mantiene control constante sobre lo valores que se reportan al sistema. 

## 7 - Actions on Objectives (Acciones sobre los objetivos)
[Impact - TA0040](https://attack.mitre.org/tactics/TA0040/)

#### Técnicas utilizadas

[T1565.001 - Data Manipulation: Runtime Data Manipulation](https://attack.mitre.org/techniques/T1565/001/): Manipular datos de manera que las autoridades no detecten ninguna anomalia en las mediciones.

[T1485 - Data Destruction](https://attack.mitre.org/techniques/T1485/): Eliminar registros críticos que puedan generar sospechas.

#### Descripción 

A través de la explotación de la API REST vulnerada, se modifica y falsifica los datos ambientales que se registran en el sistema. Estos datos manipulados son enviados a la base de datos y a la interfaz de usuario para que parezcan normales, evitando que se disparen alertas por condiciones peligrosas. Además, elimina registros históricos para que no queden rastros de las manipulaciones pasadas.
Estas acciones permiten engañar al sistema de monitoreo y las autoridades reguladoras, haciendo que el sistema parezca estar dentro de los parámetros legales y no dispare alertas a las entidades que verifican los datos.
    
