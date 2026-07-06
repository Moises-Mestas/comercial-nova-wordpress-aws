# Estimación y optimización de costos - Comercial Nova WordPress AWS

## 1. Objetivo

Documentar la estimación mensual de costos de la arquitectura WordPress implementada en AWS para Comercial Nova, así como las principales acciones de optimización propuestas.

---

## 2. Herramienta utilizada

La estimación fue realizada mediante:


AWS Pricing Calculator

Región considerada:

US East (Norte de Virginia) - us-east-1

3. Servicios incluidos en la estimación
La estimación considera los principales servicios utilizados en la arquitectura:
Servicio	Uso
Amazon EC2	Instancias para la capa WordPress
Amazon RDS for MySQL	Base de datos administrada
Elastic Load Balancing	Application Load Balancer
Amazon S3	Almacenamiento de archivos estáticos
Amazon CloudWatch	Dashboard y alarma

5. Costo mensual estimado
Servicio	Costo mensual estimado
Amazon EC2	18.22 USD
Amazon RDS for MySQL	14.71 USD
Elastic Load Balancing	16.45 USD
Amazon S3	0.21 USD
Amazon CloudWatch	0.10 USD

Costo mensual total estimado: 49.69 USD
Costo anual estimado: 596.28 USD

5. Componentes de mayor costo
Los servicios con mayor impacto económico fueron:
Amazon EC2.
Elastic Load Balancing.
Amazon RDS for MySQL.
Estos componentes representan la mayor parte del costo mensual debido al uso continuo de instancias, base de datos y balanceador durante 730 horas mensuales.

6. Acciones de optimización propuestas
6.1 Rightsizing de EC2
Mantener instancias pequeñas como t2.micro mientras CloudWatch evidencie bajo consumo de CPU. Si el uso aumenta, se puede evaluar una instancia superior o ajustar políticas de Auto Scaling.

6.2 Políticas de ciclo de vida en S3
Aplicar reglas de lifecycle para mover respaldos o archivos antiguos a clases más económicas como S3 Standard-IA, reduciendo costos de almacenamiento a mediano plazo.

6.3 Control del Auto Scaling Group
Mantener un máximo controlado de instancias, por ejemplo 4, para evitar crecimiento innecesario del costo ante picos temporales.

6.4 RDS Single-AZ para laboratorio
Usar RDS Single-AZ en ambientes académicos o de prueba para reducir costos. Para producción se recomienda Multi-AZ por alta disponibilidad.

6.5 Apagado o eliminación de recursos no utilizados
Al finalizar el laboratorio, detener o eliminar recursos como EC2, RDS y ALB para evitar consumo innecesario.

7. Comparación arquitectura inicial vs optimizada
Componente	Arquitectura inicial	Optimización propuesta
EC2	2 instancias 24/7	Rightsizing y apagado fuera de laboratorio
RDS	Instancia MySQL activa 24/7	Single-AZ en laboratorio y Multi-AZ solo en producción
ALB	Activo 730 horas/mes	Mantener solo si se requiere alta disponibilidad
S3	S3 Standard	Lifecycle hacia Standard-IA
CloudWatch	Dashboard y alarma	Mantener métricas críticas y evitar logs innecesarios

9. Conclusión
La estimación mensual permitió identificar los servicios de mayor impacto económico y proponer acciones concretas para optimizar la inversión cloud. La arquitectura mantiene un equilibrio entre disponibilidad, seguridad, escalabilidad y control de costos para un escenario académico funcional.
