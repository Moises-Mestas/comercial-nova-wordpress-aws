# Instalación y configuración de WordPress - Comercial Nova AWS

## 1. Objetivo

Documentar la instalación y configuración del portal WordPress de Comercial Nova sobre una arquitectura en AWS con alta disponibilidad, base de datos administrada y almacenamiento estático en Amazon S3.

---

## 2. Arquitectura de WordPress

WordPress fue desplegado sobre instancias Amazon EC2 Linux ubicadas en subredes públicas, detrás de un Application Load Balancer y administradas por un Auto Scaling Group.

La base de datos fue implementada mediante Amazon RDS for MySQL, ubicada en subredes privadas y con acceso restringido únicamente desde la capa de aplicación EC2.

Los archivos estáticos cargados desde WordPress fueron integrados con Amazon S3 mediante el plugin WP Offload Media Lite.

---

## 3. Servicios utilizados

| Servicio | Uso |
|---|---|
| Amazon EC2 | Servidores web para WordPress |
| Amazon RDS MySQL | Base de datos de WordPress |
| Application Load Balancer | Distribución de tráfico HTTP |
| Auto Scaling Group | Alta disponibilidad y autohealing |
| Amazon S3 | Almacenamiento de archivos estáticos |
| CloudWatch | Monitoreo de métricas y alarmas |

---

## 4. URL del sitio WordPress

El portal WordPress quedó disponible mediante el DNS del Application Load Balancer:

http://nova-wp-alb-1031980542.us-east-1.elb.amazonaws.com/

Entrada publicada de prueba:

http://nova-wp-alb-1031980542.us-east-1.elb.amazonaws.com/index.php/2026/07/03/comercial-nova-implementa-su-portal-en-aws/
5. Base de datos utilizada

WordPress se conectó a una instancia Amazon RDS for MySQL.

Campo	Valor
Servicio	Amazon RDS
Motor	MySQL
Instancia	nova-wp-db
Base de datos	wordpress
Usuario	adminwp
Endpoint	nova-wp-db.cgpwpzsek2f0.us-east-1.rds.amazonaws.com
Acceso público	No
Acceso permitido	Solo desde Security Group EC2
6. Configuración de archivos estáticos

Para almacenar archivos multimedia fuera de las instancias EC2, se configuró Amazon S3 como almacenamiento estático de WordPress.

Campo	Valor
Bucket	nova-wp-storage-moises-20260702
Región	us-east-1
Plugin	WP Offload Media Lite
Función	Copiar y servir medios desde S3
7. Alta disponibilidad

La arquitectura implementó alta disponibilidad mediante:

Dos instancias EC2 para la capa WordPress.
Application Load Balancer para distribuir tráfico.
Target Group con health check en /health.html.
Auto Scaling Group con mínimo 2 instancias.
RDS en red privada con backup automático habilitado.
8. Seguridad aplicada

Se aplicaron controles básicos de seguridad:

EC2 recibe tráfico HTTP únicamente desde el Security Group del ALB.
RDS permite MySQL 3306 únicamente desde el Security Group de EC2.
SSH restringido a la IP del estudiante.
S3 utilizado para objetos estáticos con política controlada.
Uso de rol de laboratorio LabRole por restricciones de AWS Academy.
9. Prueba funcional

La prueba funcional consistió en:

Acceder al sitio mediante el DNS del Application Load Balancer.
Ingresar al panel de administración de WordPress.
Publicar una entrada de prueba.
Insertar una imagen almacenada en Amazon S3.
Validar que el contenido se visualiza correctamente desde navegador cliente.
10. Resultado

WordPress quedó desplegado correctamente en AWS, conectado a RDS MySQL, disponible mediante ALB y con archivos estáticos integrados con Amazon S3.

El portal permite publicar contenido y servir archivos multimedia desde S3, cumpliendo los requisitos funcionales del caso Comercial Nova.
