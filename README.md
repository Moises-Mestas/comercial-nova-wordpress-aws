Aquí tienes el documento de tu proyecto reorganizado, con una estructura mucho más limpia, profesional y visualmente atractiva, ideal para una entrega académica de ingeniería. Se optimizó la jerarquía, se consolidaron los datos en tablas estructuradas y se resaltaron los puntos clave para mejorar la lectura.

---

# Informe Técnico: Implementación de WordPress en AWS

**Caso de Estudio:** Portal Web Corporativo - Comercial Nova

**Curso:** Virtualización de Servicios Tecnológicos

---

## 1. Datos del Proyecto

| Campo | Detalle |
| --- | --- |
| **Estudiante** | Moises Joaquin Mestas Maque |
| **Plataforma** | AWS Academy |
| **Región AWS** | US East (Norte de Virginia) / `us-east-1` |
| **Entorno** | Laboratorio Académico |

---

## 2. Descripción del Problema y Alcance

### El Desafío

**Comercial Nova** requiere el despliegue de un nuevo portal web corporativo y de contenidos utilizando **WordPress**. El objetivo primordial es migrar hacia una infraestructura en la nube que garantice altos estándares de **disponibilidad, seguridad, escalabilidad** y un estricto **control de costos** operativos.

### Alcance de la Solución

Para cumplir con los requerimientos, se diseñó e implementó una arquitectura funcional en AWS que abarca:

* **Cómputo de alta disponibilidad:** Despliegue automatizado mediante un grupo de escalado.
* **Red segmentada:** Infraestructura aislada mediante subredes públicas y privadas.
* **Persistencia de datos:** Base de datos relacional administrada y almacenamiento de objetos para archivos estáticos.
* **Seguridad y Monitoreo:** Control estricto de accesos a nivel de red y políticas de alertas operacionales.
* **Gestión Financiera:** Estimación y proyección de costos mensuales/anuales.

---

## 3. Arquitectura del Sistema

La arquitectura distribuye el tráfico web entrante a través de un balanceador de carga hacia múltiples servidores de aplicación, aislando la base de datos en una capa privada.

### Flujo General del Tráfico

```text
[ Usuario / Internet ]
          │
          ▼
[ Application Load Balancer ] (Público)
          │
          ▼
[ Auto Scaling Group ] (Mín: 2, Máx: 4)
   ├── EC2 WordPress Instancia 1 (Subred Pública A)
   └── EC2 WordPress Instancia 2 (Subred Pública B)
          │
          ▼
[ Amazon RDS MySQL ] (Privado)

```

> **Servicios Complementarios de Soporte:**
> * **Amazon S3:** Respaldo y almacenamiento de elementos multimedia.
> * **AWS IAM (LabRole):** Gestión de políticas y permisos de las instancias.
> * **Amazon CloudWatch:** Monitoreo y alertas basadas en infraestructura.
> 
> 

---

## 4. Mapeo de Servicios y Recursos Creados

### Infraestructura de Red y Cómputo

| Servicio AWS | Propósito en la Solución | Recurso Creado (`Name Tag`) |
| --- | --- | --- |
| **Amazon VPC** | Red virtual principal aislada. | `nova-wp-vpc` |
| **Subnets** | Segmentación por zonas de disponibilidad (AZ). | `nova-public-a`, `nova-public-b`<br>

<br>`nova-private-a`, `nova-private-b` |
| **Internet Gateway** | Salida a internet para el ALB y las EC2. | *(Creado y asociado a la VPC)* |
| **Route Tables** | Enrutamiento de subredes públicas hacia el IGW. | *(Configurado para capas públicas)* |
| **Amazon EC2** | Servidores de aplicación (Apache, PHP, WordPress). | Instancias dinámicas mediante ASG |
| **Launch Template** | Plantilla de configuración para el despliegue EC2. | `nova-wp-lt` |
| **Auto Scaling Group** | Elasticidad de instancias (Deseada: 2, Mín: 2, Máx: 4). | `nova-wp-asg` |
| **Application LB** | Distribución de tráfico HTTP balanceado. | `nova-wp-alb` |
| **Target Group** | Registro y Health Checks de instancias activas. | `nova-wp-tg` |

### Almacenamiento, Base de Datos y Monitoreo

| Servicio AWS | Propósito en la Solución | Recurso Creado (`Name Tag`) |
| --- | --- | --- |
| **Amazon RDS** | Base de datos administrada MySQL para WordPress. | `nova-wp-db` |
| **Amazon S3** | Almacenamiento de evidencias, logs y respaldos. | *(Bucket configurado)* |
| **CloudWatch** | Panel centralizado de métricas operacionales. | `nova-wp-dashboard` |
| **CloudWatch Alarms** | Alerta de consumo de cómputo crítico (>70% CPU). | `nova-alarm-ec2-cpu-high` |

---

## 5. Accesos y Validación del Sitio

El correcto funcionamiento de la plataforma se encuentra validado y accesible a través de los siguientes endpoints:

* **URL Pública del Portal (WordPress):**
[http://nova-wp-alb-1031980542.us-east-1.elb.amazonaws.com/](http://nova-wp-alb-1031980542.us-east-1.elb.amazonaws.com/)
* **URL de Validación del Balanceador (Health Check):**
[http://nova-wp-alb-1031980542.us-east-1.elb.amazonaws.com/health.html](http://nova-wp-alb-1031980542.us-east-1.elb.amazonaws.com/health.html)

### Evidencias Documentadas

1. Despliegue exitoso de la interfaz de instalación y panel de administración de WordPress mediante el DNS del ALB.
2. Publicación de entrada de prueba en el sitio web de manera persistente.
3. Target Group reportando **2 instancias EC2 en estado "Healthy"**.
4. Gráficas operacionales activas en el Dashboard de CloudWatch.

---

## 6. Pilares Operacionales de la Infraestructura

### A. Seguridad Aplicada (`Security Groups` e `IAM`)

* **Capa de Aplicación Protegida:** Las instancias EC2 aceptan tráfico web (HTTP) **únicamente** si proviene del Security Group asignado al Application Load Balancer (`nova-sg-alb`).
* **Aislamiento de Base de Datos:** Amazon RDS está configurado como no público y solo acepta conexiones en el puerto 3306 provenientes del Security Group de las EC2 (`nova-sg-ec2`).
* **Acceso Administrativo Restringido:** El acceso por SSH hacia las instancias quedó limitado exclusivamente a la dirección IP pública del estudiante.
* **Principio de Menor Privilegio:** Se utilizó el rol estructurado `LabRole` provisto por AWS Academy, mitigando el uso de credenciales de usuario raíz (`root`).
* **Resguardo de Objetos:** El bucket de Amazon S3 se configuró con el bloqueo estricto de acceso público.

### B. Alta Disponibilidad y Escalabilidad

* **Tolerancia a Fallos:** Las instancias EC2 se distribuyen en Zonas de Disponibilidad distintas (us-east-1a y us-east-1b). Si una zona o instancia falla, el tráfico se redirige inmediatamente a la activa.
* **Escalado Dinámico:** El Auto Scaling Group incrementará la cantidad de servidores hasta un máximo de 4 si la demanda lo requiere, basándose en políticas de consumo de CPU.

### C. Monitoreo y Observabilidad

El cuadro de mando `nova-wp-dashboard` recopila de forma continua los siguientes indicadores:

* **EC2:** Porcentaje de uso del procesador (`CPUUtilization`).
* **RDS:** Uso de CPU, conexiones simultáneas (`DatabaseConnections`) y almacenamiento remanente (`FreeStorageSpace`).
* **ALB:** Conteo de hosts sanos (`HealthyHostCount`), volumen de peticiones (`RequestCount`) y tiempos de respuesta (`TargetResponseTime`).

---

## 7. Análisis Financiero (AWS Pricing Calculator)

Proyección detallada de costos calculada para la arquitectura propuesta:

| Servicio AWS | Costo Mensual Estimado (USD) |
| --- | --- |
| Amazon EC2 | 18.22 |
| Amazon RDS for MySQL | 14.71 |
| Elastic Load Balancing (ALB) | 16.45 |
| Amazon S3 | 0.21 |
| Amazon CloudWatch | 0.10 |
| **Costo Mensual Total** | **49.69 USD** |
| **Costo Anual Proyectado** | **596.28 USD** |

### Estrategias de Optimización Propuestas

* **Rightsizing Constante:** Mantener el uso de familias económicas (`t2.micro`) mientras las métricas de CloudWatch no superen los umbrales estándar.
* **Ciclos de Vida en S3:** Implementar reglas para trasladar respaldos antiguos a clases de almacenamiento de menor costo (como *Glacier Instant Retrieval*).
* **Límites de Escalado:** Restringir el número máximo de instancias en el ASG para evitar sobrecostos por picos de tráfico artificiales.
* **Políticas de Apagado:** Detener o terminar los recursos una vez concluidas las fases de evaluación académica.

---

## 8. Consideraciones Finales del Proyecto

### Limitaciones del Entorno Académico

* Restricción en la edición o creación de roles y políticas personalizadas de IAM debido a los límites de la cuenta de AWS Academy.
* Uso de protocolo inseguro (HTTP) para fines demostrativos de laboratorio.

### Plan de Mejoras Futuras

* **Cifrado de Extremo a Extremo:** Implementación de HTTPS mediante la integración de *AWS Certificate Manager (ACM)* en el Load Balancer.
* **Distribución de Contenido:** Incorporación de *Amazon CloudFront* como CDN para acelerar la carga global y reducir peticiones directas al origen.
* **Infraestructura como Código (IaC):** Automatización completa del aprovisionamiento de la VPC y los recursos mediante scripts de *Terraform* o *AWS CloudFormation*.

### Lección Aprendida Clave

> La piedra angular para el despliegue exitoso radicó en la correcta articulación de las tablas de enrutamiento con el Internet Gateway. Sin esta vinculación, las instancias EC2 quedan aisladas y el Application Load Balancer es incapaz de completar los Health Checks esenciales para servir la aplicación web.
