Mil disculpas, entiendo perfectamente. Aquí tienes **únicamente** el texto limpio del informe, sin ninguna instrucción mía adentro.

Copia todo lo que está dentro del siguiente recuadro gris y pégalo tal cual en tu archivo `configuracion_s3_wordpress.md`:

```markdown
# Configuración de almacenamiento estático WordPress + Amazon S3

## 1. Objetivo

El objetivo de esta configuración fue integrar WordPress con Amazon S3 para almacenar archivos estáticos del portal Comercial Nova, tales como imágenes, documentos PDF y otros assets multimedia.

Esta integración permite desacoplar los archivos estáticos del servidor EC2, mejorar la disponibilidad del contenido y preparar la arquitectura para una futura integración con Amazon CloudFront.

---

## 2. Bucket S3 utilizado

| Recurso | Valor |
|---|---|
| Servicio | Amazon S3 |
| Bucket | nova-wp-storage-moises-20260702 |
| Región | us-east-1 / Norte de Virginia |
| Uso | Archivos estáticos de WordPress |
| Tipo de archivos | Imágenes, PDF y assets |
| Versionado / ciclo de vida | Configurado según evidencia del laboratorio |

---

## 3. Configuración de permisos

Para permitir que los archivos estáticos puedan visualizarse desde una URL S3, se configuró una política de lectura pública controlada sobre los objetos del bucket.

La política aplicada permite únicamente la acción:

```text
s3:GetObject

```

sobre los objetos del bucket:

```text
arn:aws:s3:::nova-wp-storage-moises-20260702/*

```

No se permitió escritura pública sobre el bucket.

---

## 4. Plugin utilizado en WordPress

Se utilizó el plugin: **WP Offload Media Lite**.

Este plugin permite copiar automáticamente los archivos cargados desde la biblioteca de medios de WordPress hacia Amazon S3.

---

## 5. Instalación del plugin en EC2

Como la arquitectura utiliza dos instancias EC2 detrás de un Application Load Balancer, el plugin fue instalado en ambas instancias mediante **AWS Systems Manager Run Command**.

El comando se ejecutó sobre las dos instancias WordPress del Auto Scaling Group, obteniendo resultado correcto en ambos destinos.

---

## 6. Configuración en WordPress

La integración se configuró mediante parámetros en el archivo: `/var/www/html/wp-config.php`.

Se definieron los siguientes valores principales:

* **provider:** aws
* **bucket:** nova-wp-storage-moises-20260702
* **region:** us-east-1
* **copy-to-s3:** true
* **serve-from-s3:** true
* **remove-local-file:** false
* **use-server-roles:** true

Con esta configuración, WordPress copia los archivos multimedia al bucket S3 y permite que sean servidos desde URLs de S3.

---

## 7. Validación realizada

Se validó la integración mediante las siguientes pruebas:

1. Activación del plugin WP Offload Media Lite.
2. Carga de archivos desde la biblioteca de medios de WordPress.
3. Verificación de los objetos dentro del bucket S3.
4. Apertura de una URL pública S3 desde el navegador.
5. Publicación de una entrada WordPress con imagen almacenada en S3.

---

## 8. Archivos de prueba utilizados

Se cargaron como mínimo tres archivos de prueba:

| Tipo | Ejemplo |
| --- | --- |
| **Imagen** | imagen-nova.gif / imagen-nova.jpg / imagen-nova.png |
| **Documento** | archivo PDF de evidencia |
| **Asset adicional** | imagen o recurso multimedia adicional |

---

## 9. URL S3 validada

Ejemplo de URL S3 validada:

`https://nova-wp-storage-moises-20260702.s3.us-east-1.amazonaws.com/wp-content/uploads/2026/07/06014746/imagen-nova-150x150.gif`

---

## 10. Resultado

La integración WordPress + Amazon S3 quedó funcional. Los archivos cargados desde WordPress fueron copiados al bucket S3 y se validá que al menos una URL S3 sirviera correctamente el archivo desde el navegador.

Esto cumple el requisito de almacenamiento estático solicitado para la variante del caso final.

```

---

