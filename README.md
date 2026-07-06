
# Comercial Nova WordPress AWS con almacenamiento estático en S3

## 1. Datos generales

| Campo | Detalle |
|---|---|
| Proyecto | Comercial Nova WordPress AWS |
| Curso | Virtualización de Servicios Tecnológicos |
| Plataforma | Amazon Web Services |
| Entorno | AWS Academy |
| Región | US East (Norte de Virginia) / us-east-1 |
| Aplicación | WordPress |
| Base de datos | Amazon RDS for MySQL |
| Almacenamiento estático | Amazon S3 |

---

## 2. Problema planteado

La empresa Comercial Nova requiere implementar un portal web corporativo y de contenidos utilizando WordPress en la nube.

La solución debe mejorar la disponibilidad, seguridad, escalabilidad, monitoreo y control de costos. Además, como requisito de la variante del caso final, los archivos estáticos de WordPress, como imágenes, documentos PDF y otros assets, deben almacenarse y servirse desde Amazon S3.

---

## 3. Objetivo del proyecto

Implementar una arquitectura funcional de WordPress en AWS que incluya:

- Capa de aplicación en Amazon EC2.
- Base de datos administrada en Amazon RDS.
- Balanceo de carga mediante Application Load Balancer.
- Alta disponibilidad mediante Auto Scaling Group.
- Almacenamiento estático en Amazon S3.
- Seguridad mediante VPC, subredes y Security Groups.
- Monitoreo con Amazon CloudWatch.
- Estimación y optimización de costos.

---

## 4. Arquitectura implementada

La solución se implementó en una arquitectura por capas:

```text
Usuario → Internet → Application Load Balancer → EC2 WordPress → Amazon RDS MySQL
                                                ↘ Amazon S3
````

Diagrama de arquitectura:

```text
arquitectura/diagrama.png
```

Documentación técnica:

```text
arquitectura/justificacion_arquitectura.md
```

---

## 5. Servicios AWS utilizados

| Servicio                    | Uso dentro de la solución                |
| --------------------------- | ---------------------------------------- |
| Amazon VPC                  | Red principal del proyecto               |
| Subnets públicas y privadas | Segmentación de red                      |
| Internet Gateway            | Acceso a Internet para recursos públicos |
| Amazon EC2                  | Servidores WordPress                     |
| Launch Template             | Plantilla de creación de instancias      |
| Auto Scaling Group          | Alta disponibilidad y autohealing        |
| Application Load Balancer   | Distribución de tráfico HTTP             |
| Amazon RDS for MySQL        | Base de datos administrada               |
| Amazon S3                   | Almacenamiento de archivos estáticos     |
| IAM / LabRole               | Permisos del entorno AWS Academy         |
| Amazon CloudWatch           | Monitoreo y alarmas                      |
| AWS Pricing Calculator      | Estimación de costos                     |

---

## 6. Recursos principales implementados

| Recurso            | Nombre                          |
| ------------------ | ------------------------------- |
| VPC                | nova-wp-vpc                     |
| Subred pública A   | nova-public-a                   |
| Subred pública B   | nova-public-b                   |
| Subred privada A   | nova-private-a                  |
| Subred privada B   | nova-private-b                  |
| Security Group ALB | nova-sg-alb                     |
| Security Group EC2 | nova-sg-ec2                     |
| RDS                | nova-wp-db                      |
| Bucket S3          | nova-wp-storage-moises-20260702 |
| Launch Template    | nova-wp-lt                      |
| Auto Scaling Group | nova-wp-asg                     |
| Load Balancer      | nova-wp-alb                     |
| Target Group       | nova-wp-tg                      |
| Dashboard          | nova-wp-dashboard               |

Inventario completo:

```text
aws/inventario_recursos_aws.md
```

---

## 7. URL del sitio WordPress

Sitio principal:

```text
http://nova-wp-alb-1031980542.us-east-1.elb.amazonaws.com/
```

Entrada publicada:

```text
http://nova-wp-alb-1031980542.us-east-1.elb.amazonaws.com/index.php/2026/07/03/comercial-nova-implementa-su-portal-en-aws/
```

Documentación de WordPress:

```text
wordpress/instalacion_configuracion.md
wordpress/evidencia_publicacion_contenido.md
```

---

## 8. Integración WordPress con Amazon S3

Para cumplir el requisito de almacenamiento estático, se configuró WordPress para enviar archivos multimedia hacia Amazon S3 mediante el plugin WP Offload Media Lite.

| Elemento           | Detalle                                     |
| ------------------ | ------------------------------------------- |
| Bucket             | nova-wp-storage-moises-20260702             |
| Región             | us-east-1                                   |
| Plugin             | WP Offload Media Lite                       |
| Archivos validados | Imagen, PDF y asset adicional               |
| Función            | Copiar y servir archivos estáticos desde S3 |

URL S3 validada:

```text
https://nova-wp-storage-moises-20260702.s3.us-east-1.amazonaws.com/wp-content/uploads/2026/07/06014746/imagen-nova-150x150.gif
```

Documentación S3:

```text
almacenamiento_estatico/configuracion_s3_wordpress.md
almacenamiento_estatico/evidencia_urls_archivos_s3.md
```

---

## 9. Seguridad aplicada

La arquitectura aplicó controles básicos de seguridad:

* EC2 recibe tráfico HTTP desde el Security Group del ALB.
* RDS permite conexiones MySQL 3306 solo desde el Security Group de EC2.
* RDS no tiene acceso público.
* SSH fue restringido a la IP del estudiante.
* S3 fue configurado para servir objetos estáticos de WordPress.
* IAM se trabajó con el rol disponible en AWS Academy debido a restricciones del laboratorio.

Documentación de seguridad:

```text
seguridad/matriz_accesos.md
```

---

## 10. Alta disponibilidad y escalabilidad

La solución implementa alta disponibilidad mediante:

* Dos instancias EC2 para WordPress.
* Dos subredes públicas en zonas de disponibilidad distintas.
* Application Load Balancer.
* Target Group con health check en `/health.html`.
* Auto Scaling Group con mínimo 2 instancias.
* Política de escalamiento basada en CPU.

---

## 11. Monitoreo y alertamiento

Se configuró Amazon CloudWatch para monitorear la arquitectura.

| Elemento               | Valor                   |
| ---------------------- | ----------------------- |
| Dashboard              | nova-wp-dashboard       |
| Alarma                 | nova-alarm-ec2-cpu-high |
| Métrica principal      | CPUUtilization          |
| Umbral                 | Mayor a 70%             |
| Servicios monitoreados | EC2, RDS y ALB          |

Documentación de monitoreo:

```text
monitoreo/alertas_configuradas.md
```

---

## 12. Costos estimados

La estimación se realizó con AWS Pricing Calculator.

| Servicio               | Costo mensual estimado |
| ---------------------- | ---------------------: |
| Amazon EC2             |              18.22 USD |
| Amazon RDS for MySQL   |              14.71 USD |
| Elastic Load Balancing |              16.45 USD |
| Amazon S3              |               0.21 USD |
| Amazon CloudWatch      |               0.10 USD |

**Costo mensual total estimado:** 49.69 USD
**Costo anual estimado:** 596.28 USD

Documentación de costos:

```text
costos/optimizacion_costos.md
```

---

## 13. Acciones de optimización propuestas

1. Aplicar rightsizing sobre las instancias EC2.
2. Usar lifecycle en S3 para mover archivos antiguos a clases de menor costo.
3. Limitar el máximo del Auto Scaling Group.
4. Usar RDS Single-AZ en ambientes académicos o de prueba.
5. Apagar o eliminar recursos no utilizados al finalizar el laboratorio.

---

## 14. Evidencias de funcionamiento

Las evidencias principales se encuentran en:

```text
evidencias/pruebas_funcionamiento.md
evidencias/capturas_servicios/
```

Capturas incluidas:

* VPC y subredes.
* Tabla de rutas pública.
* Security Groups.
* EC2 en estado running.
* Target Group healthy.
* Auto Scaling Group configurado.
* RDS disponible.
* WordPress admin.
* Entrada publicada.
* Plugin WP Offload Media Lite.
* Objetos en S3.
* URL S3 funcionando.
* Dashboard CloudWatch.
* Alarma CloudWatch.
* Estimación de costos.

---

## 15. Informe final

El informe final del proyecto se encuentra en la raíz del repositorio:

```text
Informe_Final_Comercial_Nova_WordPress_AWS_S3.pdf
Informe_Final_Comercial_Nova_WordPress_AWS_S3.docx
```

El informe contiene la descripción técnica completa, arquitectura, evidencias, costos, problemas encontrados, soluciones aplicadas, conclusiones y lecciones aprendidas.

---

## 16. Problemas encontrados y soluciones

| Problema                                 | Solución aplicada                                                                       |
| ---------------------------------------- | --------------------------------------------------------------------------------------- |
| Target Group aparecía unhealthy          | Se corrigió la asociación de subredes públicas a la tabla de rutas con Internet Gateway |
| WordPress no respondía correctamente     | Se validó el health check `/health.html`                                                |
| RDS no aceptaba conexión desde WordPress | Se permitió MySQL 3306 desde el Security Group de EC2                                   |
| No se pudo crear rol IAM personalizado   | Se utilizó LabRole por restricciones de AWS Academy                                     |
| Necesidad de servir archivos desde S3    | Se instaló y configuró WP Offload Media Lite                                            |

---

## 17. Limitaciones conocidas

* El entorno AWS Academy limita algunas acciones IAM, como la creación de roles personalizados.
* El sitio se desplegó con HTTP; para producción se recomienda HTTPS con AWS Certificate Manager.
* El bucket S3 permite lectura de objetos estáticos para validar el requisito académico; para producción se recomienda CloudFront con Origin Access Control.
* RDS se implementó en configuración académica; para producción se recomienda Multi-AZ.
* La arquitectura fue diseñada para fines académicos y demostrativos.

---

## 18. Mejoras futuras

* Implementar HTTPS con AWS Certificate Manager.
* Integrar Amazon CloudFront para distribución global de contenido.
* Usar Origin Access Control para proteger el bucket S3.
* Crear roles IAM personalizados con permisos mínimos.
* Automatizar la infraestructura con Terraform o CloudFormation.
* Implementar backups adicionales y pruebas de restauración.
* Agregar monitoreo avanzado de logs y métricas de aplicación.

---

## 19. Lecciones aprendidas

Durante el desarrollo del proyecto se aprendió a integrar varios servicios AWS para construir una solución WordPress funcional y escalable. También se reforzó la importancia de configurar correctamente las rutas de red, los Security Groups, la conectividad hacia RDS y el almacenamiento estático en S3.

El caso permitió evidenciar que una solución cloud no solo debe funcionar, sino también estar documentada, monitoreada, protegida y sustentada mediante costos y decisiones técnicas.

---

## 20. Estado final

| Requisito            | Estado     |
| -------------------- | ---------- |
| WordPress funcional  | Completado |
| Mínimo 2 EC2         | Completado |
| VPC y subredes       | Completado |
| RDS MySQL            | Completado |
| S3 estático          | Completado |
| ALB                  | Completado |
| Auto Scaling         | Completado |
| CloudWatch Dashboard | Completado |
| CloudWatch Alarm     | Completado |
| Estimación de costos | Completado |
| Informe PDF          | Completado |
| Evidencias en GitHub | Completado |

