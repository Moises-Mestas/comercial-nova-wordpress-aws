# Capturas de servicios - Comercial Nova WordPress AWS

Estas capturas corresponden a las evidencias principales del despliegue de WordPress en AWS con integración de archivos estáticos en Amazon S3.

| Archivo | Descripción |
|---|---|
| `01_vpc_nova_wp_vpc.png` | VPC nova-wp-vpc creada y configurada. |
| `02_subredes_publicas_privadas.png` | Subredes públicas y privadas en diferentes zonas de disponibilidad. |
| `03_tabla_rutas_publica_igw.png` | Tabla de rutas pública asociada a Internet Gateway. |
| `04_security_group_alb.png` | Security Group del ALB con acceso HTTP. |
| `05_security_group_ec2.png` | Security Group de EC2 con reglas mínimas. |
| `06_security_group_rds_mysql_desde_ec2.png` | Regla de RDS permitiendo MySQL 3306 desde EC2. |
| `07_ec2_instancias_running.png` | Instancias EC2 de WordPress en ejecución. |
| `08_target_group_healthy.png` | Target Group del ALB con instancias saludables. |
| `09_auto_scaling_group_configurado.png` | Auto Scaling Group configurado para WordPress. |
| `10_alb_asg_detalle_funcionamiento.png` | Detalle de alta disponibilidad con ALB/ASG. |
| `11_rds_disponible.png` | Instancia RDS disponible. |
| `12_rds_endpoint_backup_configuracion.png` | Detalle de configuración de RDS y backup. |
| `13_wordpress_panel_admin.png` | Panel de administración de WordPress operativo. |
| `14_wordpress_entrada_publicada.png` | Entrada publicada visible desde el sitio WordPress. |
| `15_plugin_wp_offload_media_activo.png` | Plugin WP Offload Media Lite instalado/activo. |
| `16_s3_bucket_objetos_raiz.png` | Bucket S3 con objetos/carpeta de WordPress. |
| `17_s3_objetos_archivos_estaticos.png` | Objetos estáticos visibles dentro del bucket S3. |
| `18_url_s3_funcionando.png` | Archivo estático servido desde URL S3. |
| `19_wordpress_con_imagen_s3.png` | Entrada WordPress con imagen/medio relacionado a S3. |
| `20_s3_lifecycle_o_versionado.png` | Evidencia de política lifecycle o versionado de S3. |
| `21_s3_bucket_policy_public_read.png` | Política de bucket S3 para lectura controlada. |
| `22_cloudwatch_dashboard.png` | Dashboard de CloudWatch con métricas. |
| `23_cloudwatch_alarma_cpu.png` | Alarma de CloudWatch con umbral visible. |
| `24_costos_pricing_calculator.png` | Estimación de costos en AWS Pricing Calculator. |
| `25_ssm_run_command_plugin_ec2.png` | Run Command ejecutado en instancias para configuración/plugin. |
