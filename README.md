# Image to WebP Converter

Convierte automáticamente todas las imágenes de una carpeta al formato **WebP** utilizando Python y la biblioteca **Pillow**.

## Características

- Convierte imágenes a formato WebP.
- Soporta múltiples formatos de entrada:
  - JPG
  - JPEG
  - PNG
  - BMP
  - TIFF
  - GIF
- Conserva la calidad de imagen configurable.
- Crea automáticamente la carpeta de salida.
- Compatible con Windows, Linux y macOS.
- Se puede ejecutar con Docker, sin instalar Python ni dependencias en el host.

## Requisitos

- Python 3.8 o superior
- Pillow

## Instalación (uso local)

Omite esta sección si vas a usar Docker.

Clona el repositorio:

```bash
git clone https://github.com/ralfdba/image-to-webp.git
cd image-to-webp
```

Instala las dependencias:

```bash
pip install Pillow
```

O usando un archivo `requirements.txt`:

```bash
pip install -r requirements.txt
```

## Estructura del proyecto

```
image-to-webp/
│
├── images/            # Imágenes originales
├── webp/              # Imágenes convertidas
├── main.py            # Script principal
├── Dockerfile         # Imagen con Python y Pillow
├── requirements.txt
└── README.md
```

## Uso local

Coloca las imágenes dentro de la carpeta `images`.

Ejecuta el script:

```bash
python main.py
```

Las imágenes convertidas aparecerán en la carpeta:

```
webp/
```

## Uso con Docker (sin instalar dependencias)

El `Dockerfile` construye una imagen local con Python y Pillow ya instalados. En el host solo necesitas Docker. La imagen se construye una vez y permanece en tu equipo hasta que la elimines; cada ejecución usa un contenedor que se borra al terminar (`--rm`).

**Requisito previo:** la carpeta `images/` debe existir y contener las imágenes **antes** de lanzar el contenedor. El repositorio no la crea (está en `.gitignore`); si no existe, no se convertirá nada.

```bash
mkdir -p images   # si aún no existe, y copia aquí tus imágenes
```

Desde la raíz del repositorio:

**1. Construir la imagen** (una sola vez, o si cambia `requirements.txt`):

```bash
docker build -t images2webp .
```

**2. Ejecutar la conversión:**

Linux / macOS

```bash
docker run --rm --user "$(id -u):$(id -g)" -v "$PWD":/app images2webp
```

Windows (PowerShell)

```powershell
docker run --rm -v "${PWD}:/app" images2webp
```

Las imágenes convertidas aparecerán en `webp/`, replicando la estructura de subcarpetas de `images/`.

Notas:

- `-v "$PWD":/app` monta el repositorio sobre el código de la imagen: lee `images/`, escribe `webp/` en tu carpeta y usa tu `main.py` actual. Por eso cambiar `quality`, `method` o `lossless` no requiere reconstruir la imagen.
- `--user "$(id -u):$(id -g)"` hace que los archivos generados pertenezcan a tu usuario y no a `root` (en Linux).
- Para borrar la imagen cuando ya no la necesites: `docker rmi images2webp`.

## Configuración

Dentro del script puedes modificar los siguientes parámetros:

```python
quality=85
method=6
```

### Calidad

| Valor | Descripción |
|--------|-------------|
| 100 | Máxima calidad |
| 90 | Excelente calidad |
| 85 | Recomendado |
| 75 | Mayor compresión |
| 50 | Archivos muy pequeños |

### Método de compresión

| Valor | Velocidad | Compresión |
|--------|-----------|------------|
| 0 | Muy rápida | Baja |
| 3 | Media | Buena |
| 6 | Lenta | Máxima |

## Compresión sin pérdida

Si deseas mantener la calidad original:

```python
img.save(
    output_path,
    "WEBP",
    lossless=True,
    method=6
)
```

## Formatos soportados

- JPG
- JPEG
- PNG
- BMP
- TIFF
- GIF

## Ejemplo

Antes:

```
images/
├── foto1.jpg
├── logo.png
└── paisaje.jpeg
```

Después:

```
webp/
├── foto1.webp
├── logo.webp
└── paisaje.webp
```

## Dependencias

- Pillow (fijada en `requirements.txt`)

Si usas Docker no necesitas instalarla en el host.

## Autor

rfalfarop at gmail Desarrollado en Python utilizando Pillow.