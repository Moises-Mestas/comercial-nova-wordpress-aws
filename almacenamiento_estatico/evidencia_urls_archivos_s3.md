# Evidencia de archivos estáticos servidos desde Amazon S3

## 1. Objetivo

Este documento presenta la evidencia de validación de archivos estáticos cargados desde WordPress hacia Amazon S3 para el portal Comercial Nova.

La prueba permite demostrar que los archivos multimedia no solo fueron cargados en WordPress, sino que también fueron almacenados y servidos desde el bucket S3 configurado para la arquitectura.

---

## 2. Bucket utilizado

| Campo | Valor |
|---|---|
| Bucket S3 | nova-wp-storage-moises-20260702 |
| Región | us-east-1 |
| Uso | Almacenamiento de archivos estáticos de WordPress |
| Integración | WordPress + WP Offload Media Lite |

---

## 3. Archivos validados

Se validó la carga de al menos tres archivos desde WordPress hacia Amazon S3:

| N.º | Tipo de archivo | Evidencia esperada |
|---:|---|---|
| 1 | Imagen .jpg/.png/.gif | Objeto visible en bucket S3 |
| 2 | Documento .pdf | Objeto visible en bucket S3 |
| 3 | Asset adicional | Objeto visible en bucket S3 |

---

## 4. URL S3 validada

Ejemplo de archivo servido desde Amazon S3:

```text
https://nova-wp-storage-moises-20260702.s3.us-east-1.amazonaws.com/wp-content/uploads/2026/07/06014746/imagen-nova-150x150.gif

La URL fue abierta correctamente desde el navegador, validando que el archivo estático se sirve desde Amazon S3.


5. Evidencias capturadas

Las capturas relacionadas a esta validación se encuentran en la carpeta de evidencias del repositorio y en el informe final PDF.

Evidencias principales:

Plugin WP Offload Media Lite activado.
Bucket S3 configurado.
Objetos visibles dentro del bucket.
URL S3 funcionando desde navegador.
Entrada WordPress publicada con imagen almacenada en S3.



6. Resultado de la prueba

La integración WordPress + Amazon S3 fue validada correctamente. Los archivos cargados desde WordPress fueron enviados al bucket S3 y al menos una imagen fue servida directamente desde una URL pública de S3.
Esto confirma el cumplimiento del requisito de almacenamiento estático solicitado en la variante del caso final.
