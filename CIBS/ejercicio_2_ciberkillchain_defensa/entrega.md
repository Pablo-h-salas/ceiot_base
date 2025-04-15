# Ejercicio CiberKillChain - Defensa


## Alumno

Pablo Horacio Salas

## Sistema

Sistema de monitoreo de variables ambientales y alerta temprana

## Resolución

## Descripción 
A continuación se presenta una defensa que permita detectar y mitigar el ataque mencionado en el archivo [ciberKillChain-ataque](https://github.com/Pablo-h-salas/ceiot_base/blob/master/CIBS/ejercicio_1_ciberkillchain_ataque/entrega.md).
Se procede a abordar la defensa en distintas etapas del ataque, comenzando desde la fase final y trabajando hacia atrás.

## 7 - Actions on Objectives (Acciones sobre los objetivos)
### Detección:
Monitoreo de cambios en la base de datos. Herramientas como Prometheus o ELK stack supervisan continuamente los registros en la base de datos, permitiendo detectar cambios inusuales o modificaciones en los datos.
Por otro lado el sistema puede configurarse para verificar si los datos que están siendo almacenados en la base de datos coinciden con lo que se espera.
### Mitigación:
Validación de datos. Implementar validación de datos en la API, asegurando que los valores recibidos sean consistentes con las mediciones físicas reales. Si los valores no coinciden, se puede bloquear el acceso a la base de datos o generar una alerta para la intervención de administradores.

## 6 - Command & control (Mando y control)
### Detección:

### Mitigación:


## 5 - Installation (Instalación)
### Detección:

### Mitigación:


## 4 - Exploit (Explotación)
### Detección:

### Mitigación:


## 3 - Delivery (Entrega)
### Detección:

### Mitigación:


## 2- Weaponization (Armamento)
### Detección:

### Mitigación:


## 1- Reconnaissance (Reconocimiento)
### Detección:

### Mitigación:

