# Carta digital de Bellaqueo

La carta vive en una página gratuita de GitHub Pages. El QR apunta a esa dirección, que no cambia nunca. Los productos y precios los editas en una hoja de Google (o en el archivo `carta.csv`), y la página muestra siempre lo último.

**Archivos**

| Archivo | Para qué sirve |
|---|---|
| `index.html` | La página de la carta (diseño, colores, encabezado y pie). |
| `carta.csv` | La carta: productos y precios. También es el respaldo si la hoja de Google no responde. |
| `README.md` | Esta guía. |

---

## 1. Publicar la página (una sola vez, unos 10 minutos)

1. Crea una cuenta gratis en [github.com](https://github.com). Mejor si usas un correo del local y no uno personal: así la carta no depende de una sola persona. Anota el usuario y la contraseña donde el dueño también pueda verlos.
2. Arriba a la derecha: **+ → New repository**. En *Repository name* escribe `carta`, marca **Public** y pulsa **Create repository**.
3. En la página que aparece, haz clic en **uploading an existing file**. Arrastra `index.html` y `carta.csv`, y pulsa **Commit changes**.
4. Entra a **Settings → Pages**. En *Build and deployment* elige **Source: Deploy from a branch**, **Branch: main** y la carpeta **/ (root)**. Pulsa **Save**.
5. Espera uno o dos minutos y recarga. Arriba aparecerá *Your site is live at* `https://TUUSUARIO.github.io/carta/`. **Ese es el enlace del QR.** Ábrelo en tu celular para comprobar que se ve la carta.

## 2. Generar el QR (una sola vez)

Usa un generador de **QR estático**: guarda el enlace dentro del código y no vence nunca. No uses QR "dinámicos" de páginas con prueba gratis, porque los desactivan cuando termina la prueba.

- **QRCode Monkey** ([qrcode-monkey.com](https://www.qrcode-monkey.com)): pega el enlace, pulsa *Create QR Code* y descárgalo en PNG, o en SVG para la imprenta.
- **Canva**: en *Apps*, busca "Código QR" y pega el enlace.
- **Python**:
  ```bash
  pip install "qrcode[pil]"
  python -c "import qrcode; qrcode.make('https://TUUSUARIO.github.io/carta/').save('qr-carta.png')"
  ```

Antes de imprimir, escanéalo con un Android y con un iPhone. Imprímelo de al menos 3 cm de ancho, en negro sobre blanco y con margen blanco alrededor.

## 3. Editar la carta

### Opción A: desde Google Sheets (recomendada, sirve desde el celular)

**Una sola vez:**

1. En [sheets.google.com](https://sheets.google.com) crea una hoja en blanco.
2. **Archivo → Importar → Subir**, elige `carta.csv` y como ubicación **Reemplazar hoja de cálculo**. Pulsa **Importar datos**.
3. Pulsa **Compartir**. En *Acceso general* elige **Cualquier persona que tenga el enlace** con el rol **Lector**, y pulsa **Copiar enlace**.
4. En GitHub abre `index.html`, pulsa el lápiz (*Edit this file*) y busca esta línea, que está al inicio:
   ```html
   <script>window.HOJA_DE_GOOGLE = "";</script>
   ```
   Pega el enlace entre las comillas y pulsa **Commit changes**.
5. Prueba: cambia un precio en la hoja, espera unos minutos y recarga la carta en el celular.

**Desde entonces:** editas la hoja (también desde la app de Hojas de cálculo) y la carta se actualiza sola. No hace falta tocar GitHub.

> No pongas costos ni datos internos en esa hoja: cualquiera que tenga el enlace puede verla.

### Opción B: directamente en GitHub (sin Google)

Abre `carta.csv` en GitHub, pulsa el lápiz, cambia lo que necesites y pulsa **Commit changes**. La carta se actualiza en uno o dos minutos.

### Cómo llenar la hoja

| Columna | Qué poner |
|---|---|
| `categoria` | Whisky, Gin, Pisco… Las categorías salen en el mismo orden que en la hoja. Si la dejas vacía, la fila usa la categoría de la fila anterior. |
| `producto` | El nombre. Una fila por producto. |
| `detalle` | Opcional. Sale debajo, en letra pequeña (por ejemplo, *Rosé* o *750 ml*). |
| `precio` | Solo el número: `160`, `1350`, `12.50`. Si escribes texto (por ejemplo, *Consultar*), sale tal cual. |
| `disponible` | Escribe `no` y el producto aparece como **Agotado**. Vacío = disponible. |
| `destacar` | Escribe `sí` en cualquier fila de una categoría y esa categoría sale en recuadro rojo, como el ron Flor de Caña. |

No cambies los nombres de la primera fila (los encabezados). Para quitar un producto, borra su fila.

### El respaldo

Si la hoja de Google no responde, la página muestra `carta.csv` con un aviso para que el cliente confirme los precios en barra. Cada vez que cambies varios precios, actualiza también el respaldo: en la hoja, **Archivo → Descargar → Valores separados por comas (.csv)**; luego en GitHub, **Add file → Upload files**, sube el archivo con el nombre `carta.csv` y pulsa **Commit changes**.

## 4. Reglas para que el QR funcione para siempre

- **No cambies el nombre de tu usuario de GitHub ni del repositorio `carta`.** GitHub no redirige estas páginas cuando se renombra el usuario o el repositorio, y el QR dejaría de funcionar.
- No borres el repositorio y déjalo público. Puedes editar los archivos todas las veces que quieras.
- Si cambias de hoja de Google, solo pega el nuevo enlace en `index.html`. El QR sigue siendo el mismo.
- Si GitHub te pide activar la verificación en dos pasos, guarda los códigos de recuperación junto con la contraseña. Sin ellos podrías quedarte sin acceso para editar.
- Si algún día quieres llevar la carta a otra plataforma, no cambies el QR. Reemplaza el contenido de `index.html` por esto y el QR impreso llevará a la nueva dirección:
  ```html
  <!DOCTYPE html>
  <meta http-equiv="refresh" content="0; url=https://NUEVA-DIRECCION">
  ```

## Si algo falla

- **Sale "No se pudo cargar la carta":** revisa que `carta.csv` esté en el repositorio, junto a `index.html`.
- **Sale el aviso de "última versión guardada":** la página no pudo leer la hoja. Revisa que el acceso sea *Cualquier persona que tenga el enlace* y que el enlace esté bien pegado entre las comillas. Si sigue igual, usa la opción B.
- **Probar en tu computadora:** si abres `index.html` con doble clic, el navegador no deja leer `carta.csv`. En la carpeta de los archivos ejecuta `python -m http.server` y abre <http://localhost:8000>.
