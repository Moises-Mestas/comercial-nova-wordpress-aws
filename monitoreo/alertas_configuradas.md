# Alertas y monitoreo configurado - Comercial Nova WordPress AWS

## 1. Objetivo

Documentar la configuración de monitoreo y alertamiento implementada para la arquitectura WordPress en AWS de Comercial Nova.

El monitoreo permite observar el comportamiento de las instancias EC2, la base de datos RDS y el Application Load Balancer, facilitando la detección temprana de problemas operativos.

---

## 2. Servicio utilizado

| Componente | Servicio |
|---|---|
| Monitoreo | Amazon CloudWatch |
| Dashboard | nova-wp-dashboard |
| Alarma principal | nova-alarm-ec2-cpu-high |

---

## 3. Dashboard de CloudWatch

Se creó el dashboard:

```text
nova-wp-dashboard

Este tablero incluye métricas de los principales servicios de la arquitectura:

Servicio	Métricas configuradas
Amazon EC2	CPUUtilization
Amazon RDS	CPUUtilization, DatabaseConnections, FreeStorageSpace
Application Load Balancer	HealthyHostCount, RequestCount, TargetResponseTime

4. Alarma operacional configurada
Se creó la alarma:
nova-alarm-ec2-cpu-high
Configuración principal:

Campo	Valor
Métrica	CPUUtilization
Recurso	Auto Scaling Group / EC2 WordPress
Estadística	Average
Periodo	5 minutos
Condición	Mayor que
Umbral	70%

5. Justificación de la alarma
La alarma de CPU alta permite identificar situaciones donde las instancias EC2 de WordPress presentan carga elevada. Esto es importante porque un consumo sostenido de CPU puede afectar la disponibilidad y tiempo de respuesta del portal web.
Esta alarma también sirve como base para decisiones de escalamiento y optimización, ya que permite validar si el tamaño de instancia utilizado es suficiente para la carga del sitio.

6. Evidencias generadas
Las evidencias de monitoreo se encuentran en el informe final y en la carpeta de evidencias del repositorio.
Evidencias principales:

Dashboard CloudWatch con métricas de EC2, RDS y ALB.
Alarma CloudWatch nova-alarm-ec2-cpu-high.
Umbral visible de CPU mayor a 70%.
Estado de alarmas en CloudWatch.

7. Resultado
La arquitectura quedó monitoreada mediante Amazon CloudWatch. Se logró visualizar métricas operacionales de la capa de aplicación, base de datos y balanceador de carga. Además, se configuró una alarma operacional para detectar uso alto de CPU en EC2.
