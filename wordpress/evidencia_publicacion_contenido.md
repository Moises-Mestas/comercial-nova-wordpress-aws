# Evidencia de publicación de contenido en WordPress

## 1. Objetivo

Documentar la prueba funcional del portal WordPress de Comercial Nova desplegado en AWS.

La evidencia demuestra que el sitio WordPress se encuentra operativo, accesible desde el navegador, conectado a Amazon RDS y con publicación de contenido usando archivos estáticos almacenados en Amazon S3.

---

## 2. Sitio WordPress desplegado

El sitio fue publicado mediante el DNS del Application Load Balancer:


http://nova-wp-alb-1031980542.us-east-1.elb.amazonaws.com/

La entrada publicada de prueba fue:

http://nova-wp-alb-1031980542.us-east-1.elb.amazonaws.com/index.php/2026/07/03/comercial-nova-implementa-su-portal-en-aws/
3. Entrada publicada

Se publicó una entrada de prueba en WordPress relacionada con la implementación del portal Comercial Nova en AWS.

Título de la entrada:

Comercial Nova implementa su portal en AWS

Contenido de referencia:

Comercial Nova implementó su portal WordPress en AWS integrando Amazon S3 para el almacenamiento de archivos estáticos como imágenes, documentos PDF y otros recursos multimedia.
4. Validaciones realizadas

Se realizaron las siguientes validaciones:

N.º	Validación	Resultado
1	Acceso al sitio desde el navegador	Correcto
2	Acceso al panel de administración de WordPress	Correcto
3	Publicación de una entrada	Correcto
4	Inserción de imagen en la entrada	Correcto
5	Imagen almacenada y servida desde Amazon S3	Correcto
6	Sitio accesible mediante Application Load Balancer	Correcto
5. Evidencia de archivo servido desde S3

Se validó una URL de archivo estático servido desde Amazon S3:

https://nova-wp-storage-moises-20260702.s3.us-east-1.amazonaws.com/wp-content/uploads/2026/07/06014746/imagen-nova-150x150.gif

Esta evidencia confirma que WordPress no depende únicamente del almacenamiento local de EC2 para servir archivos multimedia.

6. Relación con la arquitectura AWS

La publicación de contenido valida el funcionamiento conjunto de los siguientes servicios:

Servicio	Función validada
Amazon EC2	Ejecución de WordPress
Amazon RDS MySQL	Persistencia de datos del sitio
Application Load Balancer	Acceso público y distribución de tráfico
Amazon S3	Almacenamiento de archivos estáticos
CloudWatch	Monitoreo posterior de métricas
7. Evidencias visuales

Las capturas relacionadas se encuentran en el informe final y en la carpeta de evidencias del repositorio.

Capturas recomendadas:

Panel de administración de WordPress.
Entrada publicada.
Imagen insertada en la entrada.
URL S3 funcionando desde navegador.
Sitio cargando desde el DNS del ALB.
8. Resultado

La prueba funcional fue completada correctamente. WordPress quedó operativo en AWS, accesible mediante el Application Load Balancer, conectado a RDS MySQL y con contenido multimedia servido desde Amazon S3.
