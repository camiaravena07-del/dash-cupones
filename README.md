# Dashboard Margen & Cupones

Prototipo interactivo (pre–Looker Studio) que lee un Excel de ventas y muestra 3 hojas:
Resumen, Detalle por SKU y Resumen de Cupones — con filtros de store, marca, período y horas.

El dashboard **lee el Excel directamente en el navegador**. No hay que correr nada:
reemplazas `data.xlsx`, recargas la página, y ves los datos nuevos.

---

## Cómo publicarlo en GitHub Pages (una sola vez)

1. Crea un repositorio nuevo (público o privado con Pages habilitado).
2. Sube estos 3 archivos a la raíz: `index.html`, `data.xlsx`, `README.md`.
3. En el repo: **Settings → Pages → Build and deployment → Source: Deploy from a branch**,
   rama `main`, carpeta `/ (root)` → **Save**.
4. Espera ~1 minuto. Tu dashboard queda en:
   `https://<tu-usuario>.github.io/<nombre-repo>/`

## Cómo cambiar de datos (lo que harás cada vez)

1. Toma el Excel que te llega del reporte (ej. `Surprice_2026-...xlsx`).
2. **Renómbralo a `data.xlsx`.**
3. En GitHub: entra a `data.xlsx` → botón de editar/borrar → sube el nuevo con el mismo nombre.
   (o arrastra el nuevo `data.xlsx` a la raíz del repo y confirma el reemplazo)
4. Espera ~1 min a que GitHub Pages republique y recarga la página. Listo.

> El archivo **siempre debe llamarse `data.xlsx`** y tener la hoja **`Detalle`** con el
> mismo formato del reporte (las columnas del export estándar). Es lo único que importa.

## Probar sin subir nada (local o mientras decides)

Abre `index.html` y usa el botón **"Cargar otro Excel"** o **arrastra un .xlsx** a la ventana.
Así puedes cargar cualquier archivo al vuelo sin tocar el repo — útil para comparar marcas.

> Nota: si abres `index.html` con doble-click (file://), la carga automática de `data.xlsx`
> no funciona por seguridad del navegador; usa el botón de cargar. En GitHub Pages (https)
> la carga automática sí funciona.

---

## Qué calcula (definiciones)

- **Ingresos** = productos post-cupón (`ingreso_real`) + envío → cuadra con `order_amount` de Magento.
- **Margen descuento** = margen solo con special price (listas de precio), sin cupón.
- **Margen real** = margen con cupones/reglas de carrito ya aplicados. *Los márgenes se calculan
  solo sobre productos* (el archivo no trae costo de courier; incluir el envío inflaría el margen).
- **Cupón** = cualquier sales rule aplicada en carrito, con o sin código.
- **SKU base** = `estilo_color` (sin talla).
- Solo se consideran órdenes con `order_status = complete`.

⚠️ **IVA**: si el `product_cost` de Oracle es neto y el precio incluye IVA, el margen sale
sobreestimado. Validar con finanzas antes de tomar decisiones sobre márgenes.

## Estructura

```
index.html    ← el dashboard (todo el código + SheetJS por CDN). Esto lo modificamos si hay ajustes.
data.xlsx     ← tu export renombrado. Esto lo reemplazas tú.
README.md     ← este archivo.
```

Cuando lo llevemos a Looker Studio, la lógica de cálculo ya está validada aquí y solo se
traduce a campos calculados (ver `setup_looker_margen.md`).
