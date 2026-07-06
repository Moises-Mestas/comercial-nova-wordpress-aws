# Pruebas de funcionamiento - Comercial Nova WordPress AWS

## 1. Objetivo

Documentar las pruebas funcionales realizadas sobre la arquitectura WordPress implementada en AWS para Comercial Nova.

Estas pruebas permiten validar que los servicios principales operan correctamente de forma integrada: EC2, RDS, S3, ALB, Auto Scaling, CloudWatch y WordPress.

---

## 2. Prueba de acceso al sitio WordPress

| Elemento | Resultado |
|---|---|
| URL del sitio | http://nova-wp-alb-1031980542.us-east-1.elb.amazonaws.com/ |
| Servicio de entrada | Application Load Balancer |
| Estado | Correcto |

Resultado:

El portal WordPress fue accesible desde el navegador mediante el DNS público del Application Load Balancer.

---

## 3. Prueba de publicación de contenido

| Elemento | Resultado |
|---|---|
| Panel de administración WordPress | Accesible |
| Entrada publicada | Correcto |
| Imagen insertada | Correcto |
| Visualización desde navegador | Correcto |

Entrada publicada:


http://nova-wp-alb-1031980542.us-east-1.elb.amazonaws.com/index.php/2026/07/03/comercial-nova-implementa-su-portal-en-aws/

Resultado:

Se publicó una entrada en WordPress con contenido relacionado a Comercial Nova y la implementación del portal en AWS.

4. Prueba de almacenamiento estático en S3
Elemento	Resultado
Bucket S3	nova-wp-storage-moises-20260702
Plugin	WP Offload Media Lite
Archivos enviados a S3	Correcto
URL S3 funcionando	Correcto

URL S3 validada:

https://nova-wp-storage-moises-20260702.s3.us-east-1.amazonaws.com/wp-content/uploads/2026/07/06014746/imagen-nova-150x150.gif

Resultado:

Se validó que los archivos multimedia cargados desde WordPress fueron almacenados en Amazon S3 y que al menos una imagen fue servida correctamente desde una URL S3.

5. Prueba de base de datos RDS
Elemento	Resultado
Servicio	Amazon RDS for MySQL
Instancia	nova-wp-db
Base de datos	wordpress
Endpoint	nova-wp-db.cgpwpzsek2f0.us-east-1.rds.amazonaws.com
Acceso público	No
Conexión desde WordPress	Correcta

Resultado:

WordPress se conectó correctamente a la base de datos RDS MySQL. La publicación de contenido confirmó la persistencia de datos en la base de datos administrada.

6. Prueba de alta disponibilidad
Elemento	Resultado
Application Load Balancer	Configurado
Target Group	nova-wp-tg
Health check	/health.html
Auto Scaling Group	nova-wp-asg
Instancias mínimas	2
Estado de targets	Healthy

Resultado:

El Target Group mostró instancias saludables y el Auto Scaling Group mantuvo dos instancias EC2 disponibles para la capa WordPress.

7. Prueba de monitoreo
Elemento	Resultado
Dashboard	nova-wp-dashboard
Métricas EC2	Configuradas
Métricas RDS	Configuradas
Métricas ALB	Configuradas
Alarma	nova-alarm-ec2-cpu-high
Umbral	CPU mayor a 70%

Resultado:

Se configuró un dashboard en CloudWatch con métricas relevantes y una alarma operacional para CPU alta en EC2.

8. Prueba de costos
Elemento	Resultado
Herramienta	AWS Pricing Calculator
Costo mensual estimado	49.69 USD
Costo anual estimado	596.28 USD
Optimizaciones propuestas	5 acciones

Resultado:

Se estimó el costo mensual de la arquitectura y se propusieron acciones de optimización como rightsizing, lifecycle en S3, control de Auto Scaling, uso de RDS Single-AZ en laboratorio y eliminación de recursos no usados.

9. Problemas encontrados y soluciones
Problema	Solución aplicada
Target Group unhealthy	Se corrigió la tabla de rutas de las subredes públicas hacia el Internet Gateway
WordPress no respondía correctamente	Se verificó health check y conectividad HTTP
RDS no aceptaba conexión	Se permitió MySQL 3306 desde el Security Group de EC2
Restricción IAM en AWS Academy	Se utilizó LabRole del laboratorio
Necesidad de servir archivos desde S3	Se instaló y configuró WP Offload Media Lite
10. Evidencias visuales

Las capturas de las pruebas se encuentran en:

evidencias/capturas_servicios/

y también en el informe final:

Informe_Final_Comercial_Nova_WordPress_AWS_S3.pdf

Capturas principales recomendadas:

VPC, subredes y rutas.
Security Groups.
EC2 en estado running.
RDS disponible.
Bucket S3 configurado.
Plugin WP Offload Media Lite.
Objetos cargados en S3.
URL S3 funcionando.
Entrada WordPress publicada.
ALB y Target Group healthy.
Auto Scaling Group.
Dashboard CloudWatch.
Alarma CloudWatch.
Estimación de costos.
11. Conclusión de pruebas

Las pruebas realizadas confirman que la solución WordPress en AWS opera correctamente. El sitio se encuentra disponible mediante Application Load Balancer, utiliza Amazon RDS como base de datos, almacena archivos estáticos en Amazon S3, cuenta con monitoreo en CloudWatch y tiene una estimación de costos documentada.
