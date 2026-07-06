# Inventario de recursos AWS - Comercial Nova WordPress AWS

## 1. Objetivo

Documentar los recursos principales implementados en AWS para el despliegue del portal WordPress de Comercial Nova.

Este inventario permite identificar los servicios utilizados, su función dentro de la arquitectura y su relación con los requisitos del caso final.

---

## 2. Información general del proyecto

| Campo | Valor |
|---|---|
| Proyecto | Comercial Nova WordPress AWS |
| Plataforma | Amazon Web Services |
| Entorno | AWS Academy |
| Región | US East (Norte de Virginia) / us-east-1 |
| Aplicación | WordPress |
| Base de datos | Amazon RDS MySQL |
| Almacenamiento estático | Amazon S3 |

---

## 3. Inventario de red

| Recurso | Nombre | Función |
|---|---|---|
| VPC | nova-wp-vpc | Red principal del proyecto |
| Subred pública A | nova-public-a | Subred pública en zona de disponibilidad A |
| Subred pública B | nova-public-b | Subred pública en zona de disponibilidad B |
| Subred privada A | nova-private-a | Subred privada para base de datos |
| Subred privada B | nova-private-b | Subred privada para base de datos |
| Internet Gateway | nova-wp-igw | Salida y entrada a Internet para subredes públicas |
| Tabla de rutas pública | nova-public-rt | Ruta hacia Internet Gateway |
| Tabla de rutas privada | nova-private-rt | Rutas internas para recursos privados |

---

## 4. Inventario de seguridad

| Recurso | Nombre | Función |
|---|---|---|
| Security Group ALB | nova-sg-alb | Permite tráfico HTTP hacia el balanceador |
| Security Group EC2 | nova-sg-ec2 | Permite tráfico desde ALB hacia WordPress |
| Security Group RDS | nova-sg-rds / default ajustado | Permite MySQL 3306 solo desde EC2 |
| IAM Role | LabRole | Rol disponible por AWS Academy |
| Instance Profile | LabInstanceProfile | Perfil usado por EC2 para acceso a servicios AWS |

---

## 5. Inventario de cómputo

| Recurso | Nombre | Función |
|---|---|---|
| Amazon EC2 | Instancias WordPress | Ejecutan Apache, PHP y WordPress |
| Launch Template | nova-wp-lt | Plantilla para crear instancias WordPress |
| Auto Scaling Group | nova-wp-asg | Mantiene mínimo 2 instancias activas |
| AMI | Amazon Linux 2023 | Sistema operativo base |
| Tipo de instancia | t2.micro | Instancia usada en laboratorio |

---

## 6. Inventario de balanceo y alta disponibilidad

| Recurso | Nombre | Función |
|---|---|---|
| Application Load Balancer | nova-wp-alb | Distribuye tráfico HTTP hacia EC2 |
| Target Group | nova-wp-tg | Agrupa instancias WordPress saludables |
| Health Check | /health.html | Verifica disponibilidad de las instancias |

DNS del balanceador:


nova-wp-alb-1031980542.us-east-1.elb.amazonaws.com

URL del sitio WordPress:

http://nova-wp-alb-1031980542.us-east-1.elb.amazonaws.com/
7. Inventario de base de datos
Recurso	Valor
Servicio	Amazon RDS
Motor	MySQL
Instancia	nova-wp-db
Base de datos	wordpress
Usuario	adminwp
Endpoint	nova-wp-db.cgpwpzsek2f0.us-east-1.rds.amazonaws.com
Acceso público	No
Backup automático	Habilitado
8. Inventario de almacenamiento estático
Recurso	Valor
Servicio	Amazon S3
Bucket	nova-wp-storage-moises-20260702
Región	us-east-1
Uso	Archivos estáticos de WordPress
Integración	WP Offload Media Lite
Versionado / ciclo de vida	Configurado según evidencia
Archivos validados	Imagen, PDF y asset adicional

URL S3 validada:

https://nova-wp-storage-moises-20260702.s3.us-east-1.amazonaws.com/wp-content/uploads/2026/07/06014746/imagen-nova-150x150.gif
9. Inventario de monitoreo
Recurso	Nombre	Función
CloudWatch Dashboard	nova-wp-dashboard	Visualización de métricas EC2, RDS y ALB
CloudWatch Alarm	nova-alarm-ec2-cpu-high	Alarma por CPU alta en EC2
Métrica principal	CPUUtilization	Control de uso de CPU
Umbral	Mayor a 70%	Activación de alarma operacional
10. Inventario de costos
Servicio	Costo mensual estimado
Amazon EC2	18.22 USD
Amazon RDS for MySQL	14.71 USD
Elastic Load Balancing	16.45 USD
Amazon S3	0.21 USD
Amazon CloudWatch	0.10 USD

Total mensual estimado: 49.69 USD
Total anual estimado: 596.28 USD

11. Tags utilizados

Los recursos fueron identificados mediante tags del proyecto:

Key	Value
Project	ComercialNovaWordPress
Student	Moises
Course	VirtualizacionServicios
12. Estado general de implementación
Componente	Estado
VPC y subredes	Implementado
Security Groups	Implementado
EC2 WordPress	Implementado
RDS MySQL	Implementado
S3 estático	Implementado
ALB	Implementado
Auto Scaling Group	Implementado
CloudWatch Dashboard	Implementado
CloudWatch Alarm	Implementado
Estimación de costos	Implementado
Informe final PDF	Implementado
13. Conclusión

El inventario evidencia que la solución WordPress para Comercial Nova fue implementada con los servicios principales solicitados: red, cómputo, base de datos, almacenamiento estático, seguridad, monitoreo, alta disponibilidad y costos.

Los recursos se encuentran documentados para facilitar la revisión técnica y trazabilidad del proyecto.
