# Evidencias AWS: EC2, S3, RDS y CloudWatch

## 1. Objetivo

Documentar las evidencias técnicas principales de los servicios AWS implementados para el portal WordPress de Comercial Nova.

Este documento resume las pruebas de funcionamiento de EC2, S3, RDS, Application Load Balancer, Auto Scaling, IAM y CloudWatch.

---

## 2. Evidencia de Amazon EC2

Se implementaron instancias Amazon EC2 para ejecutar la capa de aplicación WordPress.

| Elemento | Evidencia |
|---|---|
| Servicio | Amazon EC2 |
| Sistema operativo | Amazon Linux 2023 |
| Tipo de instancia | t2.micro |
| Rol | Servidor web WordPress |
| Estado esperado | Running |
| Administración | SSH restringido / Systems Manager según disponibilidad |

Validaciones realizadas:

- Instancias EC2 en estado `running`.
- Servicio web Apache activo.
- Archivo `/health.html` respondiendo correctamente.
- WordPress accesible mediante el Application Load Balancer.
- Instancias asociadas al Auto Scaling Group.

URL de prueba de salud:


http://nova-wp-alb-1031980542.us-east-1.elb.amazonaws.com/health.html
3. Evidencia de Application Load Balancer y Auto Scaling

Se implementó un Application Load Balancer para distribuir el tráfico HTTP hacia las instancias EC2 de WordPress.

Elemento	Valor
Load Balancer	nova-wp-alb
Target Group	nova-wp-tg
Health check	/health.html
Auto Scaling Group	nova-wp-asg
Mínimo de instancias	2
Máximo de instancias	4

Validaciones realizadas:

Target Group con instancias saludables.
Balanceador accesible desde navegador.
Auto Scaling Group manteniendo dos instancias activas.
Health check funcionando correctamente.

DNS del ALB:

nova-wp-alb-1031980542.us-east-1.elb.amazonaws.com
4. Evidencia de Amazon RDS

Se implementó Amazon RDS for MySQL como base de datos administrada para WordPress.

Elemento	Valor
Servicio	Amazon RDS
Motor	MySQL
Instancia	nova-wp-db
Base de datos	wordpress
Usuario	adminwp
Endpoint	nova-wp-db.cgpwpzsek2f0.us-east-1.rds.amazonaws.com
Acceso público	No
Backup automático	Habilitado

Validaciones realizadas:

Instancia RDS en estado disponible.
WordPress conectado correctamente a la base de datos.
Publicación de contenido almacenado en la base de datos.
Acceso MySQL permitido solo desde el Security Group de EC2.
5. Evidencia de Amazon S3

Se implementó Amazon S3 como almacenamiento de archivos estáticos de WordPress.

Elemento	Valor
Servicio	Amazon S3
Bucket	nova-wp-storage-moises-20260702
Región	us-east-1
Uso	Imágenes, documentos PDF y assets
Integración WordPress	WP Offload Media Lite
Versionado / lifecycle	Configurado según evidencia

Validaciones realizadas:

Bucket S3 creado para archivos estáticos.
Integración WordPress hacia S3 configurada.
Plugin WP Offload Media Lite activado.
Archivos cargados desde WordPress visibles en S3.
URL S3 funcionando desde navegador.
Entrada WordPress con imagen servida desde S3.

URL S3 validada:

https://nova-wp-storage-moises-20260702.s3.us-east-1.amazonaws.com/wp-content/uploads/2026/07/06014746/imagen-nova-150x150.gif
6. Evidencia de IAM

Debido a restricciones de AWS Academy, no fue posible crear roles IAM personalizados.

Se utilizó el rol del laboratorio:

LabRole

Validaciones y consideraciones:

El rol permitió a EC2 interactuar con servicios necesarios del entorno.
Se documentó la restricción del laboratorio.
En un entorno productivo se recomienda crear un rol específico con permisos mínimos para S3, CloudWatch y Systems Manager.
7. Evidencia de CloudWatch

Se configuró Amazon CloudWatch para monitoreo y alertamiento.

Elemento	Valor
Dashboard	nova-wp-dashboard
Alarma	nova-alarm-ec2-cpu-high
Métrica principal	CPUUtilization
Umbral	Mayor a 70%
Servicios monitoreados	EC2, RDS, ALB

Validaciones realizadas:

Dashboard con métricas de EC2.
Dashboard con métricas de RDS.
Métricas del Application Load Balancer.
Alarma de CPU alta configurada.
Umbral visible en CloudWatch.
8. Evidencias visuales asociadas

Las capturas correspondientes se encuentran en:

evidencias/capturas_servicios/

y también dentro del informe final:

Informe_Final_Comercial_Nova_WordPress_AWS_S3.pdf

Capturas recomendadas:

EC2 running.
Target Group healthy.
Auto Scaling Group con 2 instancias.
RDS disponible.
Bucket S3 con objetos.
Plugin WP Offload Media Lite.
URL S3 funcionando.
Dashboard CloudWatch.
Alarma CloudWatch.
Sitio WordPress funcionando desde ALB.
9. Resultado general

Las evidencias demuestran que la arquitectura AWS implementada para Comercial Nova opera correctamente.

Se validó el funcionamiento de:

Capa de aplicación en EC2.
Alta disponibilidad mediante ALB y Auto Scaling.
Base de datos administrada con RDS MySQL.
Almacenamiento estático en S3.
Monitoreo y alertamiento con CloudWatch.
Seguridad básica mediante Security Groups e IAM del laboratorio.
