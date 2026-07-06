# Matriz de accesos y seguridad - Comercial Nova WordPress AWS

## 1. Objetivo

Documentar los controles de acceso aplicados en la arquitectura WordPress en AWS para Comercial Nova, considerando el principio de mínimo privilegio, segmentación de red y protección de servicios críticos.

---

## 2. Principio de mínimo privilegio

La arquitectura fue diseñada para permitir únicamente los accesos necesarios entre componentes:

- Los usuarios acceden al sitio mediante el Application Load Balancer.
- Las instancias EC2 reciben tráfico HTTP únicamente desde el Security Group del ALB.
- Amazon RDS permite conexiones MySQL únicamente desde el Security Group de EC2.
- Amazon S3 se utiliza para archivos estáticos de WordPress.
- IAM se gestionó mediante el rol disponible en AWS Academy debido a restricciones del laboratorio.

---

## 3. Security Groups implementados

| Security Group | Recurso asociado | Reglas principales | Justificación |
|---|---|---|---|
| nova-sg-alb | Application Load Balancer | HTTP 80 desde Internet | Permite acceso público al portal WordPress mediante el balanceador. |
| nova-sg-ec2 | Instancias EC2 WordPress | HTTP 80 desde nova-sg-alb | Evita acceso directo público a la capa de aplicación. |
| nova-sg-ec2 | Instancias EC2 WordPress | SSH 22 restringido a la IP del estudiante | Endurecimiento básico de acceso administrativo. |
| Security Group RDS / default ajustado | Amazon RDS MySQL | MySQL/Aurora 3306 desde nova-sg-ec2 | Permite que solo WordPress se conecte a la base de datos. |
| S3 bucket policy | Amazon S3 | Lectura de objetos estáticos | Permite servir imágenes y archivos estáticos desde URL S3. |

---

## 4. Matriz de comunicación entre componentes

| Origen | Destino | Puerto / Protocolo | Estado | Motivo |
|---|---|---|---|---|
| Internet | Application Load Balancer | HTTP 80 | Permitido | Acceso público al sitio WordPress. |
| Application Load Balancer | EC2 WordPress | HTTP 80 | Permitido | Distribución de tráfico hacia instancias saludables. |
| EC2 WordPress | Amazon RDS MySQL | TCP 3306 | Permitido | Conexión de WordPress a la base de datos. |
| EC2 WordPress | Amazon S3 | HTTPS 443 | Permitido | Envío y recuperación de archivos estáticos. |
| IP del estudiante | EC2 WordPress | SSH 22 | Permitido restringido | Administración técnica controlada. |
| Internet | Amazon RDS | TCP 3306 | Denegado | La base de datos no debe exponerse públicamente. |
| Internet | EC2 WordPress directo | HTTP 80 | Denegado luego de pruebas | El acceso debe realizarse mediante el ALB. |

---

## 5. Seguridad en Amazon RDS

Amazon RDS fue configurado para operar como base de datos MySQL para WordPress.

Controles aplicados:

- Acceso a RDS restringido a la capa de aplicación EC2.
- Puerto MySQL/Aurora 3306 permitido solo desde el Security Group de EC2.
- RDS no expuesto públicamente.
- Uso de endpoint privado dentro de la VPC.
- Backup automático habilitado según configuración del servicio.

Endpoint utilizado:

```text
nova-wp-db.cgpwpzsek2f0.us-east-1.rds.amazonaws.com
6. Seguridad en Amazon S3
El bucket utilizado fue:
nova-wp-storage-moises-20260702

Controles aplicados:
Bucket dedicado para archivos estáticos de WordPress.
Versionado o ciclo de vida configurado según evidencia del laboratorio.
Política de lectura pública controlada para servir archivos estáticos desde S3.
Escritura pública no permitida.
Carga de archivos gestionada desde WordPress mediante plugin WP Offload Media Lite.

7. IAM y rol utilizado
En AWS Academy no fue posible crear roles IAM personalizados debido a restricciones del laboratorio.
Por ello se utilizó el rol disponible:

LabRole
Este rol permitió a las instancias EC2 interactuar con servicios necesarios del laboratorio, incluyendo operaciones hacia S3.
Restricción documentada:
No se pudo crear un rol IAM personalizado por falta de permisos iam:CreateRole en AWS Academy.

8. Endurecimiento básico aplicado
Control	Estado
SSH restringido a IP del estudiante	Aplicado
RDS sin acceso público	Aplicado
EC2 detrás de ALB	Aplicado
HTTP directo a EC2 usado solo para pruebas	Removido o documentado
S3 con lectura pública solo para objetos estáticos	Aplicado
Uso de usuario root evitado	Aplicado
IAM personalizado	Limitado por AWS Academy

9. Riesgos y mejoras futuras
Riesgo / limitación	Mejora propuesta
Sitio publicado por HTTP	Habilitar HTTPS con AWS Certificate Manager.
Bucket S3 con objetos públicos	Usar CloudFront con Origin Access Control en producción.
Rol IAM genérico de laboratorio	Crear rol IAM mínimo para EC2-S3 en ambiente productivo.
RDS Single-AZ en laboratorio	Usar RDS Multi-AZ para producción.
Administración por SSH	Priorizar Session Manager.

10. Conclusión
La arquitectura aplicó controles de seguridad básicos y coherentes con una práctica académica en AWS Academy. Se priorizó el principio de mínimo privilegio en Security Groups, se restringió el acceso a RDS desde EC2, se utilizó ALB como punto de entrada y se documentaron las limitaciones del entorno de laboratorio.
