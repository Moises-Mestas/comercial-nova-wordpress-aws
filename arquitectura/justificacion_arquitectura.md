# Justificación de arquitectura - Comercial Nova WordPress AWS

## 1. Objetivo de la arquitectura

La arquitectura implementada tiene como objetivo desplegar un portal WordPress funcional para la empresa Comercial Nova en AWS, considerando disponibilidad, seguridad, escalabilidad, monitoreo, almacenamiento estático en S3 y control de costos.

---

## 2. Descripción general

La solución se implementó en AWS Academy utilizando una arquitectura web en capas:

- Capa de red mediante Amazon VPC.
- Capa de aplicación mediante Amazon EC2.
- Distribución de tráfico mediante Application Load Balancer.
- Alta disponibilidad mediante Auto Scaling Group.
- Base de datos administrada mediante Amazon RDS for MySQL.
- Almacenamiento estático mediante Amazon S3.
- Monitoreo mediante Amazon CloudWatch.

---

## 3. Diagrama de arquitectura

El diagrama de arquitectura se encuentra en esta carpeta:


arquitectura/diagrama.png

La arquitectura representa el flujo principal:

Usuario → Internet → Application Load Balancer → EC2 WordPress → RDS MySQL
                                               ↘ Amazon S3
4. Componentes principales
Componente	Recurso implementado	Función
VPC	nova-wp-vpc	Red principal del proyecto
Subred pública A	nova-public-a	Alojamiento de recursos web en AZ A
Subred pública B	nova-public-b	Alojamiento de recursos web en AZ B
Subred privada A	nova-private-a	Red privada para base de datos
Subred privada B	nova-private-b	Red privada para alta disponibilidad de RDS
EC2	Instancias WordPress	Capa de aplicación
ALB	nova-wp-alb	Balanceo de tráfico HTTP
Target Group	nova-wp-tg	Verificación de salud de instancias
Auto Scaling Group	nova-wp-asg	Alta disponibilidad y autohealing
RDS	nova-wp-db	Base de datos MySQL administrada
S3	nova-wp-storage-moises-20260702	Archivos estáticos de WordPress
CloudWatch	nova-wp-dashboard	Monitoreo y alertamiento
5. Diseño de red

Se creó una VPC propia para aislar los recursos del proyecto.

La red fue segmentada en subredes públicas y privadas:

Tipo de subred	Uso
Subredes públicas	Capa web, ALB y EC2 WordPress
Subredes privadas	Base de datos RDS

Las subredes públicas cuentan con ruta hacia Internet Gateway para permitir acceso al sitio web mediante el Application Load Balancer.

Las subredes privadas se utilizaron para proteger la base de datos, evitando su exposición directa a Internet.

6. Capa de aplicación

La capa de aplicación está compuesta por instancias EC2 con Linux, Apache/PHP y WordPress.

Se implementaron al menos dos instancias EC2 administradas por un Auto Scaling Group. Esto permite mantener disponibilidad ante fallos de una instancia y facilita el escalamiento ante aumento de demanda.

7. Balanceo de carga

Se utilizó un Application Load Balancer para recibir el tráfico HTTP desde Internet y distribuirlo hacia las instancias EC2 saludables.

El balanceador se asoció al Target Group:

nova-wp-tg

El health check fue configurado sobre la ruta:

/health.html

Esto permite validar que las instancias WordPress se encuentren operativas antes de recibir tráfico.

8. Base de datos

La base de datos fue implementada mediante Amazon RDS for MySQL.

Campo	Valor
Instancia	nova-wp-db
Motor	MySQL
Base de datos	wordpress
Endpoint	nova-wp-db.cgpwpzsek2f0.us-east-1.rds.amazonaws.com
Acceso público	No

RDS fue ubicado en red privada y solo permite conexiones desde la capa de aplicación EC2 mediante el puerto MySQL 3306.

9. Almacenamiento estático en S3

Para desacoplar los archivos multimedia del servidor EC2, se integró WordPress con Amazon S3.

Bucket utilizado:

nova-wp-storage-moises-20260702

La integración se realizó mediante el plugin WP Offload Media Lite, permitiendo copiar y servir archivos estáticos desde S3.

Esto mejora la independencia entre instancias EC2 y contenido multimedia, además de preparar la arquitectura para una futura integración con CloudFront.

10. Seguridad

La seguridad se aplicó principalmente mediante Security Groups y segmentación de red.

Recurso	Control aplicado
ALB	Recibe HTTP desde Internet
EC2	Recibe HTTP solo desde el ALB
RDS	Recibe MySQL 3306 solo desde EC2
SSH	Restringido a la IP del estudiante
S3	Lectura controlada de objetos estáticos
IAM	Uso de LabRole por restricciones de AWS Academy
11. Alta disponibilidad y escalabilidad

La arquitectura incluye alta disponibilidad mediante:

Dos instancias EC2.
Dos zonas de disponibilidad.
Application Load Balancer.
Auto Scaling Group con mínimo 2 instancias.
Health checks en Target Group.
Autohealing ante falla de instancia.

La escalabilidad se implementó mediante una política de Auto Scaling basada en CPU, permitiendo aumentar capacidad ante mayor demanda.

12. Monitoreo

Se configuró Amazon CloudWatch para monitorear métricas principales:

CPU de EC2.
Conexiones de RDS.
Almacenamiento libre en RDS.
Hosts saludables del ALB.
Tiempo de respuesta del ALB.

También se configuró una alarma de CPU alta para EC2 con umbral de 70%.

13. Costos

La arquitectura fue estimada en AWS Pricing Calculator.

Concepto	Valor
Costo mensual estimado	49.69 USD
Costo anual estimado	596.28 USD

Las optimizaciones propuestas incluyen rightsizing de EC2, lifecycle en S3, control del máximo del Auto Scaling Group, uso de RDS Single-AZ en laboratorio y eliminación de recursos no utilizados.

14. Problemas encontrados y soluciones
Problema	Solución aplicada
Target Group aparecía unhealthy	Se corrigió la asociación de subredes públicas a la tabla de rutas con Internet Gateway
WordPress no respondía correctamente	Se validó health check y conectividad
RDS no conectaba con WordPress	Se permitió MySQL 3306 desde el Security Group de EC2
No se pudo crear rol IAM personalizado	Se utilizó LabRole por restricción de AWS Academy
Necesidad de servir medios desde S3	Se instaló y configuró WP Offload Media Lite
15. Conclusión

La arquitectura implementada cumple con el objetivo de desplegar WordPress en AWS de forma funcional, segura y escalable. Se logró integrar EC2, RDS, ALB, Auto Scaling, S3 y CloudWatch en una solución demostrable para Comercial Nova.

La solución permite publicar contenido en WordPress, almacenar archivos estáticos en Amazon S3, monitorear recursos críticos y estimar costos operativos mensuales.
