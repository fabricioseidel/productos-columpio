# Creador de Productos — Columpio Kids

Esta es una aplicación web estática de página única (SPA) diseñada para gestionar de forma visual e intuitiva el catálogo de productos de **Columpio Kids** (tienda de artículos para bebés y niños) y facilitar su exportación en formatos compatibles como **Uber Eats Grocery CSV** y JSON.

## 🚀 Características

- **Visualización y Búsqueda:** Lista completa de productos con búsqueda en tiempo real por nombre, código de barras o categoría.
- **Creación y Edición Guiada:** Formulario dividido en pestañas (Card 1 y Card 2) para ingresar detalles del producto, variantes, descripción de exportación, precios, unidades de medida y estado activo/inactivo.
- **Carga de Imágenes Directa:** Integración directa con **Cloudinary** para subir y previsualizar imágenes al instante.
- **Exportación en Lote:**
  - Descarga de archivo CSV listo para integrar con **Uber Eats Grocery** (solo productos activos con imagen).
  - Exportación de todo el catálogo en formato JSON.
- **Configuración Dinámica:** Panel de ajustes para configurar en caliente la conexión con **Supabase** y **Cloudinary** directamente en la interfaz. Los parámetros se guardan de forma segura en el almacenamiento local del navegador (`localStorage`).

---

## 🛠️ Configuración e Integración

Para habilitar el funcionamiento completo, haz clic en la pestaña **Ajustes ⚙️** en la barra de navegación superior e introduce tus credenciales de Supabase y Cloudinary.

### 1. Base de Datos (Supabase)

La aplicación asume que tu base de datos de Supabase cuenta con las siguientes tablas y estructuras correspondientes:

#### Tabla: `products`
Esta tabla contiene toda la información de los productos del catálogo.

| Campo | Tipo de Dato | Descripción |
|---|---|---|
| `id` | `bigint` (PK, Autodecrementable) | Identificador único del producto. |
| `name` | `text` (Not Null) | Nombre del artículo. |
| `barcode` | `text` (Not Null) | Código de barras o SKU único. |
| `category` | `text` (Nullable) | Nombre de la categoría a la que pertenece. |
| `description` | `text` (Nullable) | Descripción para la tienda/exportación. |
| `sale_price` | `numeric` (Default `0`) | Precio de venta normal. |
| `offer_price` | `numeric` (Nullable) | Precio en oferta especial (opcional). |
| `measurement_unit` | `text` (Default `'un'`) | Unidad de medida (`un`, `g`, `kg`, `ml`, `l`). |
| `measurement_value` | `numeric` (Default `1`) | Valor de cantidad (ej: `500` en gramos). |
| `is_active` | `boolean` (Default `true`) | Define si está a la venta. |
| `image_url` | `text` (Nullable) | Enlace seguro a la imagen en Cloudinary. |
| `features` | `jsonb` (Nullable) | Guarda en JSON variantes `{ "variante": "..." }` y subcategorías `{ "subcategoria": "..." }`. |
| `updated_at` | `timestamp with time zone` | Fecha de la última actualización. |

#### Tabla: `categories` *(Opcional)*
Se utiliza para listar las categorías disponibles en el selector. Si la tabla no existe o está vacía, el creador cargará automáticamente categorías infantiles predeterminadas como fallback.
- `id` (bigint, PK)
- `name` (text, Not Null)
- `is_active` (boolean, default true)

---

### 2. Carga de Imágenes (Cloudinary)

Para permitir que los usuarios suban fotos desde sus dispositivos, debes configurar un preset de carga sin firma (**Unsigned Upload Preset**):
1. Inicia sesión en tu cuenta de Cloudinary y dirígete a **Settings ⚙️ (Ajustes)**.
2. Ve a la sección **Upload** y baja hasta **Upload presets**.
3. Haz clic en **Add upload preset**.
4. Configura el **Signing Mode** en **Unsigned**.
5. Asigna una carpeta predeterminada si lo deseas (ej: `productos`) y guarda los cambios.
6. Copia el nombre del **Upload preset** recién creado e introdúcelo en la interfaz junto con tu **Cloud Name**.

---

## 🌐 Despliegue

Al ser un desarrollo basado puramente en HTML5, CSS3 y Vanilla Javascript (sin dependencias de empaquetadores externos), el despliegue es inmediato:

- **GitHub Pages:** Sube los archivos a un repositorio de GitHub y activa GitHub Pages desde la sección de ajustes.
- **Vercel / Netlify:** Importa tu repositorio directamente; estas plataformas detectarán `index.html` automáticamente como punto de entrada de la aplicación.
