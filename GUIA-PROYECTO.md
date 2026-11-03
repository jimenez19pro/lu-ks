# Lu-Ks y Lu-Ks Tienda — Guía para continuar el proyecto

Esta guía es para **Claude** (en una cuenta nueva) y para **Jose**. Contiene todo lo necesario para seguir trabajando en las dos apps sin perder nada.

> **Claude: lee esta guía completa antes de responder o tocar código.**

---

## 1. Cómo trabajar con Jose (muy importante)

- **Dueño:** Jose Montes, Colombia (hora de Bogotá). Escribe en **español**; respóndele en español sencillo, sin tecnicismos.
- **Solo tiene iPhone, no tiene computador.** Todo lo hace desde Safari en el celular.
- **No hay conexión directa a GitHub desde Claude.** Jose sube los archivos a mano. Por eso cada cambio se entrega así:
  1. Claude modifica el código, lo construye y **lo prueba** antes de entregarlo.
  2. Claude le envía a Jose **solo los archivos que cambiaron**, diciendo exactamente **en qué carpeta** va cada uno (raíz del repo o carpeta `tienda`).
  3. Jose los sube en github.com/jimenez19pro/lu-ks → botón **⋯** (al lado del botón verde *Code*) → **Upload files** → **Commit changes**.
  4. Siempre se actualiza también **`codigo-fuente.zip`** en la raíz (es el respaldo del código).
- Dale **pasos numerados y cortos**. Jose suele mandar **capturas de pantalla**; revísalas y confirma antes de que siga.
- Antes de que Jose **publique reglas de Firebase**, pídele una captura y revísala línea por línea.
- Al terminar una mejora, Jose suele pedir **más sugerencias**. Propón ideas nuevas (no repitas las descartadas, ver sección 10).
- Cuidado con los dos `index.html`: el de la **raíz** es Lu-Ks y el de la carpeta **`tienda/`** es Lu-Ks Tienda. Si se sube uno en el lugar del otro, se daña la app.

---

## 2. Enlaces

| Qué | Dónde |
|---|---|
| App Lu-Ks (finanzas personales) | https://jimenez19pro.github.io/lu-ks/ |
| App Lu-Ks Tienda (negocios) | https://jimenez19pro.github.io/lu-ks/tienda/ |
| Repositorio (público) | https://github.com/jimenez19pro/lu-ks |
| Firebase (proyecto `lu-ks-ia`) | https://console.firebase.google.com |
| Contacto público de las apps | WhatsApp 573137904093 · jose.jimeneza@cecar.edu.co |

---

## 3. Qué hace cada app

### Lu-Ks (finanzas personales, multiusuario, gratis)
PWA instalable (Agregar a pantalla de inicio). Cada persona crea su cuenta (correo/contraseña o Google) y sus datos quedan en Firebase.

Funciones principales: movimientos (gastos/ingresos/transferencias), cuentas y tarjetas, presupuestos, metas de ahorro, deudas y plan para salir de deudas, compras a cuotas, pagos recurrentes, reportes y gráficas interactivas, búsqueda, fondo de emergencia, 50/30/20, 4x1000, "Tu año", saludo y frases, animación de inicio (splash), sonidos, **temas de color** (6 colores), modo claro/oscuro, **bilingüe** (español/inglés), asistente con IA (cada usuario pone **su propia clave de Gemini**: chat, notas y escaneo de recibos), importar/exportar respaldo JSON, recorrido de bienvenida, "Anotar con Siri" (`?nota=`), historias semanales e "Invita a un amigo" (enlace `?de=Nombre` y QR), **notificaciones push** (FCM), **garantías y facturas** (con foto y aviso de vencimiento), página de presentación (landing) y de privacidad, aviso de actualización.

- **Cuentas sueltas:** si hay movimientos o pagos recurrentes en una cuenta que no existe en Cuentas (p. ej. el "Efectivo" que la app ofrecía por defecto), Mis cuentas y Gestión rápida muestran un aviso con el botón **Arreglar**, que los pasa a una cuenta real y puede mantener su saldo (código en `gastos.html`, funciones `huerf`, `orfOpen`, `orfGo`). La opción fantasma "Efectivo" ya no aparece si el usuario tiene una cuenta de tipo efectivo.
- **Montos con puntos de mil** mientras se escribe en todos los campos de dinero (`MONEYID`, `fmtMil`); `num()` ya entiende "1.250.000".
- La pantalla de notificaciones es sencilla para todos. La parte técnica se abre tocando **7 veces la campana 🔔** (modo técnico; se guarda como `dev` en Ajustes del usuario).
- En **Perfil → Para tu negocio** aparece **Lu-Ks Tienda**: abierta para quien tiene licencia y con etiqueta **PRO** para los demás (ver sección 7).

### Lu-Ks Tienda (para tiendas de barrio, de pago)
App aparte en la carpeta `tienda/` (su propio ícono naranja, manifest y service worker). Usa el mismo Firebase.

Funciones: caja del día y cierre de caja (base, esperado vs contado, resumen del día por WhatsApp), ventas por productos o monto rápido, cálculo de vuelto, recibo por WhatsApp (todos los mensajes de WhatsApp tienen formato profesional: recibo con N.º, comprobante de abono, estado de cuenta, pedido y resumen detallado; funciones reciboTxt, abonoTxt, cobroTxt, pdEnviar, resumenTxt), y además se envían como **comprobantes en imagen (PNG) o PDF** desde una hoja de vista previa (funciones renderDoc con canvas, docRecibo/docAbono/docCobro/docPedido/docResumen, makePdf sin librerías, compartir/shareFile con el menú Compartir del iPhone), **precios por cantidad** (hasta 4 por producto, p. ej. "Paca" 12 und a $32.000; al vender se aplican solos combinando paquetes de mayor a menor y el resto a precio normal; botones "＋ Paca" en el carrito; se guardan en el producto como `pk:[{n,q,pv}]` y se copian en cada ítem de la venta; funciones `pks`, `pkCalc`, `lt`, `pkTxt`, `detTxt`, `pkRow`, `pkHint`), **foto a la factura del proveedor con IA** (solo el dueño; Inventario → "📸 Foto a la factura del proveedor" o desde "Llegó mercancía"; usa la clave de Gemini del dueño guardada en `t_conf/ia` (campo `k`), que se toma sola de Lu-Ks personal si ya la tiene ahí o se pega en Ajustes → 🤖 Leer facturas con IA; la IA devuelve JSON con productos, unidades y costo por unidad; pantalla de revisión donde se elige el producto de cada línea, se crean productos nuevos con precio sugerido según el margen, se ve si subió el costo y se elige el pago: de la caja, transferencia, aparte, a crédito con el proveedor o "ya la anoté"; recuerda cómo se llama cada producto en la factura en `fa` del producto para reconocerlo la próxima vez; funciones `factura`, `fxLeer`, `fxArmar`, `drawFx`, `fxGuardar`, `fxMatch`, `iaKey`, `iaSetup`, `gemT`), **lectora de códigos Bluetooth** (funciona como teclado: un `keydown` global detecta escritura muy rápida terminada en Enter/Tab y la toma como escaneo; en Caja abre una venta y suma el producto con bip, como en el D1; en una venta abierta suma al carrito; en Inventario abre el producto u ofrece crearlo; en "Llegó mercancía" elige el producto; en el editor llena el código; funciones `lectora`, `ventaCode`, `invCode`, `entradaCode`) y **Escanear inventario** (Inventario → "▮▯▮ Escanear inventario": se pasa producto por producto; si es nuevo abre el formulario con el código y busca el nombre en Open Food Facts; si ya existe deja corregir las existencias; funciones `cargar`, `ldCode`, `offFill`), fiados (cupo, abonos, "para cobrar hoy" con recordatorio por WhatsApp), inventario (costos, margen, existencias, alerta de "por acabarse", **escáner de código de barras** con BarcodeDetector (en iPhone se carga el polyfill `barcode-detector@3.0.8` de jsDelivr/unpkg, basado en zxing-wasm, mucho más rápido que ZXing-js; analiza solo la franja del recuadro a alta resolución, enfoque continuo, zoom 1.5 si se puede, linterna 🔦 si el celular la permite; si no carga, usa ZXing-js de respaldo; funciones `scan`, `loadBD`, `bdLoop`, `camTune`, `scanZX`), **ventas por libra/kilo**), aviso de **"te subió el costo"** con precio sugerido, gastos por categoría (**gastos fijos** —arriendo, empleados, servicios— van aparte: no se restan de la ganancia del día y solo bajan la caja si se pagan "De la caja"; método "👛 Aparte"), proveedores (crédito, pagos, día de visita, **pedido por WhatsApp**), **Cobro por transferencia** (Ajustes → llave Bre-B, Nequi, Daviplata, titular y foto del QR del banco guardada en `t_conf/qr`; pantalla "📲 Mostrar QR / llave para cobrar" en ventas por transferencia y en fiados; los datos de pago salen en el estado de cuenta y en recibos de fiado; funciones `payData`, `cobroQR`, `pagoTxt`), reportes (ganancia estimada, métodos de pago, 14 días, más vendidos, cierres), **empleados** (invitación por enlace, aprobación del dueño, no ven ganancias ni costos, no pueden borrar), respaldo JSON, borrar datos, **licencias** (pantalla de pago).

---

## 4. Estructura del código (dentro de `codigo-fuente.zip`)

```
build_pub.py          → construye la carpeta pub/ (lo que se publica)
gastos.html           → app base de Lu-Ks (original); build_pub.py le inyecta lo público
pubsrc/
  config.js           → configuración web de Firebase (PÚBLICA, no tocar)
  vapid.txt           → clave pública VAPID para push
  dict.json, i18n.js  → traducciones al inglés (MutationObserver)
  shim.js             → capa de datos Firestore (emula la API de Google Sheets que usaba gastos.html), login y cuenta
  imp.js  onb.js  siri.js  land.js  story.js  push.js  gar.js  lic.js  post.js → módulos de Lu-Ks
  ui.js ui.css       → diseño nuevo de Lu-Ks: transiciones, números que cuentan, gestos (cerrar ventanas deslizando, deslizar movimiento para editar, deslizar para actualizar)
  sw.js manifest.json privacidad.html íconos shot*.jpg
  tienda/
    index.html        → TODA la app Lu-Ks Tienda (un solo archivo)
    sw.js manifest.json íconos
    reglas-firestore.txt → copia de las reglas de Firebase vigentes
pruebas/              → mockfb.js (Firebase falso) y pruebas con Playwright
```

**Cómo construir:** descomprimir, ejecutar `python3 build_pub.py` → genera `pub/index.html` (Lu-Ks) y `pub/tienda/…`.
**Qué se sube a GitHub:** `pub/index.html` (raíz), los archivos de `pub/tienda/` (carpeta `tienda`) y un `codigo-fuente.zip` nuevo con el código fuente actualizado (raíz). El resto de archivos del repo (sw.js, config.js, manifest, íconos, privacidad) solo se suben si cambian.

**Datos en Firestore**
- Lu-Ks: `users/{uid}/tabs/{Pestaña}__{n}` (filas en JSON, trozos de 700) y `users/{uid}/garantias/{id}`. Pestañas: Gastos, Presupuestos, Categorias, Recurrentes, Metas, Deudas, Cuentas, ListaCategorias, Cuotas, Ajustes.
- Tienda: `users/{dueño}/t_prod, t_venta, t_cli, t_fmov, t_gasto, t_prov, t_pmov, t_caja, t_conf`, equipo en `t_miem` y `t_sol`, enlace del empleado en `users/{empleado}/t_link/tienda`.
- Licencias: `licencias/{uid}` y precio en `config/tienda` (campo `precio`).

---

## 5. Firebase (proyecto `lu-ks-ia`)

- **Plan Spark (gratis, sin tarjeta).** Si algún día hay muchas tiendas usando la app a diario, podría necesitarse el plan Blaze (cobra por uso).
- **Authentication:** correo/contraseña y Google activados. Dominio autorizado: `jimenez19pro.github.io`.
- **La clave web de `config.js` es pública por diseño.** No rotarla ni revocarla (dañaría la app). GitHub mandó una alerta de "secreto"; se puede cerrar como *Won't fix*. Restringir la clave es opcional y de baja prioridad.
- **Messaging (push):** campañas desde Firebase → Messaging. En iPhone, las notificaciones solo funcionan con la app instalada en la pantalla de inicio.

**Reglas de Firestore vigentes (publicadas):**
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{uid}/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == uid;
    }
    match /users/{uid}/{col}/{doc} {
      allow read, create, update: if request.auth != null
        && col in ['t_prod','t_venta','t_cli','t_fmov','t_gasto','t_prov','t_pmov','t_caja','t_conf']
        && exists(/databases/$(database)/documents/users/$(uid)/t_miem/$(request.auth.uid));
    }
    match /users/{uid}/t_miem/{emp} {
      allow read: if request.auth != null && request.auth.uid == emp;
    }
    match /users/{uid}/t_sol/{emp} {
      allow create, update, delete: if request.auth != null && request.auth.uid == emp;
    }
    match /licencias/{uid} {
      allow read: if request.auth != null && request.auth.uid == uid;
    }
    match /config/{doc} {
      allow read: if request.auth != null;
    }
  }
}
```
Truco para el iPhone: en el editor de reglas es difícil borrar todo; es más fácil **agregar** bloques nuevos justo antes de las dos llaves finales (las reglas se suman).

---

## 6. Cómo publicar un cambio (resumen para Jose)

1. Claude te manda los archivos y te dice dónde va cada uno.
2. Abres github.com/jimenez19pro/lu-ks.
3. **Raíz:** botón **⋯** → **Upload files** → eliges los archivos → **Commit changes**.
4. **Carpeta tienda:** primero entras a la carpeta **tienda** (arriba debe decir *lu-ks / tienda*) y ahí haces lo mismo.
5. Esperas unos 2 minutos, cierras la app y la vuelves a abrir (sale el aviso de actualización).

---

## 7. Licencias de Lu-Ks Tienda (cómo cobrar)

- Jose ya tiene **su licencia permanente** (documento en `licencias` con su código, campo `nombre`).
- Quien no tiene licencia ve una pantalla de pago con **su código** y un botón para escribirle a Jose por WhatsApp.
- **Para activar a un cliente (hoy, manual):** Firebase → Firestore Database → **Datos** → (raíz, ícono 🏠) → colección **licencias** → **+ Agregar documento** → ID = código del cliente → campo `nombre` (string). Si es mensual, campo `hasta` (string) con formato `AAAA-MM-DD`, por ejemplo `2026-11-19`. Al vencer, la app le avisa sola y sus datos quedan guardados.
- **Precio visible (opcional):** colección `config` → documento `tienda` → campo `precio` (string), por ejemplo `$15.000 al mes · 1er mes gratis`.
- Los empleados de una tienda con licencia no pagan aparte.
- Precio sugerido (aún no decidido por Jose): 1 mes gratis; $15.000/mes; $150.000/año; "fundador" $10.000/mes para las primeras 10 tiendas. Cobro por Nequi/Daviplata/Bre-B.

---

## 8. Pruebas automáticas (para Claude)

En `pruebas/`: `mockfb.js` (Firebase falso: auth, firestore con batch, onSnapshot de colecciones y documentos, messaging) y scripts de Playwright (`ttienda.js`, `tt3.js` = 8 mejoras + empleados, `tlic.js` = licencias, `tadm.js` = panel admin, `tpk.js` = precios por cantidad, `tfx.js` = factura con IA (simula la respuesta de Gemini), `tld.js` = lectora y escanear inventario (simula la lectora escribiendo rápido), `tcam.js` = escáner de cámara con cámara falsa y lector simulado, `tcaja.js` = vender escaneando con la cámara (modo continuo), `tux.js` = diseño nuevo de Lu-Ks (botón +, gestos, números, inglés), `tgar.js`, `tdev.js`, `tcol.js`, etc.).
- Copiar `pruebas/*` a la raíz del proyecto, construir (`python3 build_pub.py`) y ejecutar `node ttienda.js`.
- Sirven `pub/` con `python3 -m http.server`, bloquean service workers (`serviceWorkers:'block'`) y reemplazan Firebase por el mock.
- En la tienda, las pruebas le dan licencia a la cuenta de prueba escribiendo `licencias/{uid}` con un batch.

---

## 9. Lo que NO se debe hacer

- **Nunca** subir `lu-ks-mis-datos.json` (datos personales de Jose) ni ningún dato personal al repo (es público).
- No cambiar `config.js` ni rotar la clave de Firebase.
- No guardar claves de Gemini en el repo (cada usuario pone la suya en su cuenta).
- No reemplazar el `index.html` de la raíz con el de la tienda ni al revés.
- No publicar reglas de Firebase sin revisar una captura antes.

---

## 10. Decisiones y preferencias de Jose

- Monetizar **más adelante**, cuando Lu-Ks tenga unos 50–100 usuarios activos (ver Firebase → Authentication → Usuarios). Lu-Ks personal sigue gratis; Lu-Ks Tienda es la parte de pago.
- App Store: aplazado hasta tener los US$99 del programa de Apple.
- **No quiere:** lectura de SMS bancarios, logos o marcas registradas, calendario, PIN, rachas, lista de mercado, restringir la clave de Firebase por ahora. Los saldos de las cuentas los edita él.
- **Ideas que ya descartó (no volver a ofrecer):** dictar movimientos por voz (se hizo, no le convenció y se quitó), etiquetas en movimientos, modo viaje/otra moneda, dividir cuentas, simulador "¿y si…?", resumen como imagen, reto de 52 semanas; tampoco eligió las primeras ideas de monetización (Plus, emprendedor, familia, PDF, asesor IA, alertas, retos/cursos, instituciones, pago único, patrocinios, asesorías) y eligió "versiones para negocios" → de ahí nació Lu-Ks Tienda.
- **No le interesaron "por el momento"** (no insistir, quizá más adelante): control de tarjeta de crédito, detector de movimientos repetidos, cierre del mes, exportar a Excel, accesos rápidos, aviso de actualización exacto, foto del recibo en movimientos, escanear factura electrónica, alcancía de redondeo, sobres de efectivo, calculadora de ahorro. En la Tienda: PVP sugerido desde la factura, bonificaciones, costo promedio, favoritos del monto rápido, caja rápida, "lo que te subió en el mes".
- Otras ideas propuestas y **no elegidas todavía**: ¿debo declarar renta?, plan para prima y cesantías, cuántas horas de trabajo cuesta un gasto, comparar dos meses, lo que te subió de precio; para la tienda: pedidos y domicilios, promociones y combos, informe mensual en PDF.

---

## 11. Versión 2 de la tienda — PUBLICADA (20 oct 2026)

Ya está en producción:
1. **Panel de administrador** (Lu-Ks Tienda → Ajustes → 🛠️ Panel de administrador): solicitudes, activar/renovar (1 mes, fundador, 1 año, prueba 30 días, gratis siempre; se suma desde el vencimiento), desactivar, cobrar por WhatsApp, ingreso mensual, cupos de fundador y editar precios (`config/tienda`: precio, precioMes, precioAno, precioFund, cuposFund).
2. **Prueba gratis automática de 30 días** (botón en la pantalla de pago; una sola vez por cuenta; guarda `prueba:true` y `vence` como Timestamp).
3. **Aviso de renovación** 5 días antes de vencer el plan o la prueba.
4. **Productos por vencer** (fecha de vencimiento al llegar mercancía, lotes por fecha, aviso en Caja, Oferta / Ya se vendió / Se botó → pérdida sin afectar la caja).
5. `lic.js` entiende pruebas (`vence`).

- Las reglas de la sección 5 ya incluyen (agregado al final) el bloque de `isAdmin`, `admins`, `licencias/{lid}`, `solicitudes` y `config` escritura para admin. Copia completa en `pubsrc/tienda/reglas-firestore.txt`.
- Colección **`admins`** creada en la raíz con el documento de Jose (ID = su código `INbBN72DK3UHDOLWj5g4tKXkk5P2`, campo `Nombre`).
- **Activar clientes ahora se hace desde el Panel de administrador** (ya no hace falta crear documentos a mano en Firestore).

## 12. Precios por cantidad — PUBLICADO (20 oct 2026)
En Inventario → producto → "🏷️ Precios por cantidad". Muestra cuánto sale cada unidad, cuánto ahorra el cliente y cuánto ganas. En la venta se aplica solo (ej. 18 gaseosas = 1 paca + 1 media docena). Recibo, comprobante, reportes y ganancia ya lo tienen en cuenta. Solo cambió `tienda/index.html`; no requiere cambios en las reglas de Firebase.

## 13. Foto a la factura con IA — PUBLICADO (20 oct 2026)
El dueño le toma foto a la factura del proveedor (hasta 4 fotos) y la IA llena lo que llegó. Antes de sumar se revisa todo. Cada tienda usa su propia clave gratis de Gemini (no hay servidor; poner una clave de Jose para todos la dejaría expuesta). Solo cambió `tienda/index.html`; no requiere cambios en las reglas de Firebase.

## 14. Lectora de códigos y Escanear inventario — PUBLICADO (20 oct 2026)
Jose tiene iPhone 12 Pro (Lightning): se le recomendó una lectora **Bluetooth** compatible con iOS en modo HID/teclado (no USB). Con la lectora conectada el teclado del iPhone puede no aparecer; casi todas las lectoras tienen un código o doble toque para mostrarlo. Solo cambió `tienda/index.html`.

## 15. Escáner de cámara más rápido — 21 oct 2026
Jose reportó que la cámara demoraba o no reconocía. Safari de iPhone no trae BarcodeDetector y se usaba ZXing-js (lento). Ahora se usa el polyfill `barcode-detector@3.0.8` (zxing-cpp en WebAssembly). Solo cambió `tienda/index.html`.

## 16. Vender escaneando (modo caja continuo) — 21 oct 2026
Caja → "📷 Vender escaneando" (aparece cuando algún producto tiene código) o el 📷 dentro de una venta: la cámara queda abierta, cada producto suma con bip y destello verde; el mismo código solo se vuelve a contar cuando sale del cuadro (6 lecturas vacías seguidas); barra con último producto, total, "↩️ Quitar el último" y "✅ Cobrar". Producto por peso o código desconocido cierran la cámara y siguen el flujo normal. Funciones `scanVenta`, `ventaMulti`, `scanCartUI`; `found(code,manual)` con `SC.multi`.

## 17. Lu-Ks: diseño más fluido y profesional — 21 oct 2026
- **Inicio:** encabezado en una línea (círculo con la inicial → Ajustes, fecha, saludo y un solo botón de menú); la sincronización es un puntico verde (amarillo si hay pendientes) y el texto "Sincronizado" ya no sale cuando todo está bien. Tarjeta principal `heroCard()` "Te quedan $X" (ingresos − gastos del período; si no hay ingresos usa el presupuesto; si no, "Gastaste"). `statCards()` ahora muestra "Saldo total" y "Gastos" o "Ahorrado". Vacíos compactos (`.slim`) para cuentas y presupuestos. Siluetas de carga `skel()`.
- **Iconos:** las categorías usan iconos de línea en un círculo del color de la categoría (`ICP`, `ci(k,tamaño)`); si el usuario cambió el emoji o creó una categoría propia, se muestra su emoji en el mismo círculo.
- **Colores:** los gastos ya no salen en rojo; el rojo queda para alertas (presupuesto pasado, saldos negativos).
- **Anotar:** el botón + abre directo el formulario con el valor enfocado; dentro hay botones "IA" (Notas AI) y "Foto" (escanear recibo, si hay clave de Gemini).
- **Nombres:** pestañas Inicio / Movimientos / Reportes / Ajustes; Reportes: Resumen / Descargar; tendencia sin historial muestra un mensaje amable.
- **Error corregido:** los sonidos no funcionaban porque la variable `LKAC` se usaba para el audio y para los colores; el audio ahora usa `LKAU`.
- Traducciones nuevas agregadas a `pubsrc/dict.json`.
- **Teclado en iPhone (22 oct):** el formulario se abría con animación y el teclado salía a la vez, así que iOS lo subía de más. Ahora `#sheet` abre sin animación (`noanim`) y con el teclado abierto las ventanas se ajustan al área visible usando `visualViewport` (clase `kb`, en `ui.js`). Prueba: `tkb.js`.
- **Pantalla de inicio (22 oct):** se veía una "interferencia" abajo durante el splash: la barra inferior con desenfoque (`backdrop-filter`) se pintaba por encima en iOS. Ahora `#splash~nav` y `#splash~#app` quedan ocultos mientras existe el splash (en `ui.css`). Prueba: `tsp.js`.

## 18. Animaciones de inicio y video promocional — 22 oct 2026
- **Lu-Ks:** nueva pantalla de bienvenida (`#splash` en `gastos.html`): ícono que aparece, gráfica que se dibuja con brillo, nombre que sube letra por letra, lema y barrita de carga; dura ~2,4 s, se salta tocando y respeta "reducir movimiento".
- **Lu-Ks Tienda:** pantalla de bienvenida nueva (`#tsp` al inicio de `tienda/index.html`): se arma la tiendita (local, toldo por franjas, ventanas que se encienden) y sube "Lu-Ks Tienda" con el lema "Caja · Fiados · Inventario".
- **Video promocional** (vertical 1080×1920, 22 s, sin música): fuente en `promo/promo.html` (animación CSS con las capturas de las apps) y `promo/render.js` (graba cuadro por cuadro con Playwright); luego `ffmpeg -framerate 30 -i fr/%04d.jpg -c:v libx264 -crf 20 -pix_fmt yuv420p LuKs-promo.mp4`. Las capturas (`l-home.png`, `l-mov.png`, `t-caja.png`, `t-venta.png`) se toman con `tshot.js` y `tour.js`.
- `film.js` en pruebas: genera una tira de fotogramas de las animaciones de inicio.
- **Audio del video (22 oct):** música original generada por código (sin derechos de terceros) en `promo/audio/musica.py` (120 BPM, Re mayor, sincronizada con las escenas: campana del logo, whoosh en transiciones, golpe en "¿Tienes una tienda?", bips del escáner a 11,6 s y 12,4 s, acorde final). Mezcla: `ffmpeg -i LuKs-promo.mp4 -i musica.wav -map 0:v -map 1:a -c:v copy -af loudnorm=I=-14:TP=-1.5 -c:a aac -b:a 192k -shortest LuKs-promo-audio.mp4`.
- **Voz del video (22 oct):** voz de ElevenLabs (Eleven v4, voz "Jaider", en el texto se escribe "Lucas" para que pronuncie bien Lu-Ks). `promo/voz/mezcla.sh` corta cada frase de `tomaA.mp3` y la pone en su escena (frase 2 acelerada 8 %), baja la música bajo la voz con `sidechaincompress` (voz unos 17 dB por encima) y deja el total en -14 LUFS → `LuKs-promo-voz.mp4`.
- **Videos por app (22 oct):** `promo/gen.py` genera `video-luks.html` y `video-tienda.html` (25 s cada uno, el celular cambia de pantalla por escena; capturas `l2-*.png` con `tshotl.js` y `t-*.png` con `tshot.js`). `node promo/render2.js video-luks.html 25 frl` graba los cuadros. Música: `python3 promo/audio/musica2.py luks 25 0 musica-luks.wav` y `python3 promo/audio/musica2.py tienda 25 2 musica-tienda.wav` (la de la Tienda va 2 semitonos más alta, con bips a 4,6 / 5,4 / 6,2 s).
- **Voces de los videos por app (22 oct):** `promo/voz/mezcla-luks.sh` (6 frases de `luks.mp3`) y `promo/voz/mezcla-tienda.sh` (6 frases de `tienda.mp3`). En la Tienda la música se genera sin bips (`musica2.py tienda 25 2 musica-tienda-sinbip.wav nobip`) y los bips van en una pista aparte (`bips.wav`) que no baja con la voz.
- **IA ocupada (22 oct):** Jose recibió "503" (servidores de Gemini saturados). Ahora, en la Tienda (`gemT`) y en Lu-Ks (`gem` en `shim.js`), un 500/502/503/504 o "overloaded" reintenta 2 veces (esperando 1,8 s y 3,6 s) y luego pasa al siguiente modelo (`gemini-flash-latest` → `gemini-2.5-flash` → `gemini-flash-lite-latest` → `gemini-2.0-flash`). Si todo falla, la Tienda muestra el mensaje y el botón "🔄 Reintentar con la misma foto" (`FXF`, acción `fxRetry`). Prueba: `tfx503.js`.
- **Monto rápido con varios productos (22 oct):** en la venta, pestaña "⚡ Monto rápido", el botón "＋ Agregar y anotar otro" (`ACT.rapAdd`) agrega el valor escrito como un ítem sin producto (`p:""`, `pc:0`) al carrito y deja el campo listo para el siguiente; si el nombre y el precio se repiten, suma cantidad. Lo que quede escrito sin agregar se cobra como antes (`V.rapido`). En "Lo que más se vende" estos ítems se agrupan por nombre y su ganancia se estima con el margen de Ajustes. Prueba: `trap.js`.
- **Escáner sin internet (22 oct):** `tienda/sw.js` (caché `luks-tienda-v4`) guarda al instalarse el polyfill `barcode-detector@3.0.8` y `zxing_reader.wasm` (zxing-wasm 2.2.4, de fastly.jsdelivr.net) y los sirve primero desde el celular (`LIBRX`, cache-first, `ignoreVary`). Si se cambia la versión del lector en `index.html`, hay que cambiarla también en `LIB` de `sw.js` y subir el número de caché. Prueba: `toff.js` (con `PW_EXPERIMENTAL_SERVICE_WORKER_NETWORK_EVENTS=1`).
- **Historial de facturas (22 oct):** al sumar una factura leída con IA se guarda en `users/{dueño}/t_fact/{id}` (proveedor, N.º, fecha de la factura, total, forma de pago, productos con cantidad y costo, referencia al gasto o compra a crédito) y sus fotos comprimidas (<900 KB cada una) en `t_fimg/{id}_{n}`. Se ven en "🧾 Ver facturas anteriores" (pantalla de la foto), "🧾 Facturas de este proveedor" y "Ver en el historial" al terminar. Detalle con foto ampliable, compartir foto y borrar del historial (no toca inventario ni pagos). Solo el dueño (no se agregó a `COLS`, así que no afecta a los empleados ni requiere reglas nuevas). Si se lee una factura con el mismo N.º y proveedor, avisa que ya se sumó (`FX.dup`) y pide confirmar. Funciones `fxHist`, `facts`, `verFact`, `factShare`, `factDel`.

## 19. Página de ventas de Lu-Ks Tienda — 22 oct 2026
- Dirección: `https://jimenez19pro.github.io/lu-ks/tienda/conoce.html` (para compartir en WhatsApp, estados y redes). Al compartirla sale con imagen (`tienda/og.jpg`, 1200×630) y título.
- Archivos en la carpeta `tienda`: `conoce.html` (las 4 capturas y la portada del video van dentro del archivo), `video.mp4` (video de la Tienda con voz) y `og.jpg`.
- Fuente: `landing/conoce.src.html` + `landing/arma.py` (mete las imágenes y el enlace de WhatsApp y produce `conoce.html`; `video.mp4` debe estar en la carpeta `landing` al armar). `landing/og.html` es la plantilla de la imagen para compartir.
- Contenido: "Tu tienda, en orden", botones "Probar 30 días gratis" (→ `./`, la app) y WhatsApp 313 790 4093 con mensaje listo; video; 6 funciones; capturas deslizables; 3 pasos; precios (Mensual $15.000, Anual $150.000, Fundador $10.000 primeras 10 tiendas — cambiar en `conoce.src.html` si cambian en el panel); preguntas frecuentes; botón flotante de WhatsApp. Tema claro/oscuro automático.
- `tienda/sw.js`: ya no intercepta videos (`.mp4`/`.webm`) para que el video se reproduzca bien en iPhone.

## 20. Cuadre de caja por empleado (turnos) — 22 oct 2026
- En Caja, cuando la tienda tiene empleados, cada persona ve "🕐 Empezar mi turno": cuenta la plata de la caja (la app sugiere lo que debería haber) y empieza. Mientras está abierto ve cuánto ha vendido y cuánto debería haber; con "🔒 Terminar mi turno" escribe lo que contó y la app dice si cuadró, sobró o faltó. Luego puede mandar el cuadre al WhatsApp del dueño (`waDueno` de Ajustes).
- El turno solo cuenta lo que hizo esa persona entre la hora de inicio y la de cierre: ventas en efectivo, abonos en efectivo, gastos en efectivo y pagos a proveedores en efectivo.
- El dueño ve "👷 Turnos de hoy" en Caja (toca uno para ver el detalle, compartir o borrar) y "👷 Turnos por persona" en Reportes (turnos, vendido, faltantes y diferencia acumulada del período).
- Datos: se guardan en la colección `t_caja` con id `T…` y `tipo:"turno"` (`uq` = uid de la persona, `por`, `fecha`, `ini`, `fin`, `base`, `contado`, `esperado`, `vendido`, `n`, `ef`, `tr`, `fi`, `abEf`, `gEf`, `ppEf`). **No requiere cambiar las reglas de Firebase.** El cierre de caja del día y la lista "Cierres de caja" ignoran los turnos.
- Desde esta versión, las ventas, fiados/abonos, gastos y movimientos de proveedores nuevos guardan `uq` (uid de quien los hizo) para que el turno sepa de quién son, aunque dos personas tengan el mismo nombre. Los registros viejos se reconocen por el nombre (`por`).
- Funciones: `turnos`, `miTurno`, `tCalc`, `turnoCard`, `turnoIni`, `turnoGo`, `turnoFin`, `turnoCerrar`, `verTurno`, `turnoTxt`, `turnoShare`, `turnoDel`, `turnosRep`. Prueba: `tturno.js`.
- **Forma de trabajar (pedido de Jose, 22 oct):** dar una lista de varias ideas, que él elija las que quiera, y entregarlas todas juntas en una sola subida (subir archivos a cada rato le resulta engorroso).

## 21. "Llegó mercancía" con buscador y producto nuevo — 23 oct 2026
- Jose reportó que en "Llegó mercancía" solo podía elegir de la lista (un selector largo con 62 productos) y no podía agregar uno a mano.
- Ahora el campo Producto es un buscador (`#e-s`): escribes parte del nombre y salen hasta 6 coincidencias (`eLista`); al tocar una queda elegida (`ePick`, guarda el id en el campo oculto `#e-p`). Siempre aparece "➕ Crear «lo que escribiste» como producto nuevo" (`eNuevo`, solo el dueño): abre el formulario de producto con el nombre ya puesto y al guardar vuelve a "Llegó mercancía" (si le pusiste existencias en el formulario, no se vuelven a sumar).
- Si escaneas un código que no existe, abre el formulario de producto nuevo con ese código (antes solo avisaba). Los empleados ven el aviso de que lo cree el dueño.
- La fecha de vencimiento ya no se ve aplastada en iPhone cuando está vacía (`input[type=date].in{min-height:48px}`).
- Pruebas: `tentrada.js` (nueva); `tt3.js` y `tld.js` actualizadas al buscador.
- Ideas que Jose no eligió (22–23 oct, no volver a ofrecer igual): pedido sugerido, cobro de fiados en serie, resumen del domingo, invita y gana, tiendas por vencer, conteo de inventario, cambiar precios en bloque, productos quietos, recargas y servicios, catálogo para domicilios, meta del día, anular venta, lista de precios para imprimir, envases retornables, retiros del dueño, venta por voz, puntos para clientes, encargos, horas fuertes, avisos al dueño, adelantos a empleados, paca→unidad, combos, historial de cambios, varias tiendas, impresora, mes contra mes, códigos propios, ayuda "?", gastos compartidos, tu año en Lu-Ks, plan de la prima, modo viaje, conteo de billetes, oferta para estados, quién vende más barato, tienda de demostración. (Todavía no respondió a la última lista.)

## 22. Proveedor nuevo al revisar la factura — 23 oct 2026
- Jose reportó que en "Revisa la factura" no podía crear un proveedor nuevo: si la IA no leía el nombre del proveedor, el selector solo traía "Sin proveedor" y los ya creados (y "A crédito" exige proveedor).
- Ahora el selector `#fx-p` siempre tiene "➕ Agregar proveedor nuevo" (o "➕ Nuevo: <nombre leído>" si la IA lo leyó). Al elegirlo aparecen "Nombre del proveedor nuevo" (`#fx-pn`, con lo que leyó la IA) y "Celular del vendedor (opcional)" (`#fx-pt`); se guardan en `FX.provN` / `FX.provT`. Al sumar, si el nombre está vacío pide escribirlo; si ya existe un proveedor con ese nombre usa ese en vez de duplicarlo. Prueba: `tfxprov.js`.

## 23. Foto a la factura: ya no se queda cargando — 23 oct 2026
- Jose reportó que "Foto a la factura" se quedaba cargando y nunca terminaba. Causa: la petición a Gemini no tenía tiempo límite; si la señal se caía o el iPhone pausaba la app (por ejemplo, al salir a otra app mientras leía), la promesa nunca terminaba.
- Ahora (`gemT` en `tienda/index.html`):
  - Cada intento tiene un límite de 45 s (`FXTMO`, con `AbortController`). Si se pasa, prueba el siguiente modelo (`GMT`). En total máximo 150 s (`FXMAX`) y luego muestra "Google tardó demasiado…" con el botón Reintentar.
  - Si se corta la conexión, reintenta una vez a los 2 s.
  - Si sales de la app más de 3 s mientras lee y vuelves, retoma la lectura sola (`visibilitychange`, `run.resume`).
  - Mientras lee muestra el tiempo que lleva ("⏱️ 12 s · suele tardar entre 10 y 40 segundos") y un botón "✖️ Cancelar lectura" (`fxCancel`). Si cierras la ventana, la lectura se cancela y no se abre la revisión después.
  - Las fotos se envían más livianas (máx. 1600 px, calidad 0,8) para que suban más rápido con datos, y la respuesta se limita a 8192 tokens.
  - Cada lectura es un `FXRUN` ({t0, ac, dead, done, resume, iv}); `fxFin` detiene el contador. Las respuestas de reintentos internos se envuelven con `fxDone` para no procesar el error dos veces.
- Prueba: `tfxlento.js` (modelo colgado, corte de red, todo colgado, cancelar, cerrar mientras lee, salir y volver a la app). Para pruebas se puede bajar el límite con `window.__fxTmo` / `window.__fxMax`.
- Pendiente opcional: Lu-Ks (`gem` en `shim.js`) no tiene todavía este límite de tiempo.

## 24. Paquete de 5 mejoras (pedido "hazlos todos") — 25 oct 2026
**1. Buscador en todas las listas (Tienda).** Componente único `pkr(id,o)` (+ `pkrList`, `pkrPick`, `pkrNew`, `pkrGet`, `provItems`, `mkProv`). Pinta un campo de texto `#<id>-s` (con `data-pkr`), un oculto `#<id>` con el id elegido y la lista `#<id>-l` (máx. 6 coincidencias, primero las que empiezan igual). Al tocar o escribir se abre la lista; "➕ Crear «texto» como …" crea lo que falta. `pkrGet(id)` al guardar: devuelve el id elegido; si solo se escribió un nombre igual a uno existente, usa ese; si es nuevo y la lista permite crear (`o.make`), lo crea. Se usa en:
   - Fiar (`v-cli`, en `cliPicker`): cliente nuevo con celular opcional (`#v-ct`); `V.cliT`/`V.cliTel` conservan lo escrito cuando se redibuja el carrito. `pickCli` crea el cliente al guardar.
   - Recibir abono (`a-cli`): solo clientes que deben; si escribes uno que no debe, avisa.
   - Producto (`p-prov`), gasto de mercancía (`g-p`, ahora visible aunque no haya proveedores), compra/pago a proveedor (`m-p`; ya no exige crear el proveedor antes) y pedido (`pd-p`, al elegir queda "Pedido para X" con "Cambiar proveedor").
   - "Revisa la factura" (`fx-p`) sigue con su selector propio (ver sección 22).
**2. Lu-Ks: la IA ya no se queda cargando.** `gem` en `pubsrc/shim.js`: 40 s por intento (`GTMO`), máximo 90 s (`GMAX`), cambia de modelo si se demora, reintenta una vez si se corta la red, retoma si sales de la app y vuelves (`GACT`, `visibilitychange`). Errores nuevos `timeout` y `network` con mensajes claros en Notas AI, escanear recibo y chat (`gastos.html`). Prueba: `tgemluks.js`.
**3. Deshacer (Tienda).** El aviso de abajo (`snack`) tiene un botón "↩️ Deshacer" (`#snack-u`, `SNU`) durante 7 s después de guardar una venta, un fiado, un abono o un gasto nuevo. La venta/fiado usa `anularVenta(v)` (devuelve unidades y lotes y borra el cargo del fiado). Solo el dueño lo ve (las reglas no dejan borrar a los empleados). El aviso se oculta al abrir otra ventana y los mensajes cortos (`toast`) suben si el aviso está visible. Prueba: `tundo.js`.
**4. Teclado (Tienda).** En las ventanas, Enter pasa al siguiente campo (el teclado muestra "Siguiente"; en el último, "Listo" y se cierra). En los buscadores, Enter elige el primer resultado. Excepciones: `scan-in`, `w-q`, `w-v`, `v-q` (`KBX`). Con el teclado abierto la ventana se ajusta al espacio visible (`#sh.kb`, `visualViewport`) y el campo activo queda a la vista (`kbVer`). Prueba: `tkbt.js`.
**5. Revisión general (Tienda).** `trev.js` arma una tienda de 62 productos, 5 clientes y 3 proveedores y fotografía 25 pantallas (claro/oscuro). Arreglos: el aviso de abajo tapaba los botones de las ventanas; en "Llegó mercancía" los campos Cantidad/Costo quedaban desalineados (`.two>label{align-self:flex-end}`); la lista de productos de "Llegó mercancía" ahora aparece al tocar el buscador y no ocupa espacio desde el inicio.

## 25. Arreglo: se veía la pantalla de atrás con el teclado abierto — 25 oct 2026
- Jose mandó captura: en "Nuevo producto" con el teclado numérico de iPhone, entre el final de la ventana y el teclado se veía la pantalla de atrás (movimientos de la Caja). Causa: con el teclado abierto la ventana se recortaba al alto visible (`visualViewport.height`) y el teclado de iOS es translúcido.
- Tienda (`#sh.kb`): ahora la ventana va de `visualViewport.offsetTop` hasta abajo (`bottom:0`) y lleva `padding-bottom` igual al alto del teclado + 24 px, así el fondo de la ventana sigue detrás del teclado y el último campo se puede subir.
- Lu-Ks (`pubsrc/ui.js` y `ui.css`, `.sheet.kb`): igual; el panel interno ocupa hasta abajo con `padding-bottom` = alto del teclado + 20 px; `kbOff(x)` limpia los estilos; el campo activo se sube si queda tapado. Pruebas: `tkb.js` (Lu-Ks) y `tkbt.js` (Tienda).

## 26. Diseño tipo iPhone (iOS 26, vidrio) en las dos apps — 25 oct 2026
- Pedido de Jose: que el sistema se vea como iOS. Eligió: las dos apps, estilo "vidrio" de iOS 26 y color azul del iPhone.
- **Tienda:** capa de estilos al final del `<style>` de `tienda/index.html` (bloque "Diseño iOS 26 (vidrio)"). Colores del sistema de Apple (fondo #F2F2F7 / negro, tarjetas #FFF / #1C1C1E, azul #007AFF / #0A84FF, rojo #FF3B30, verde #248A3D / #30D158), fuente del sistema (`-apple-system`, en iPhone es San Francisco), tarjeta principal con degradado azul, botones en cápsula, campos rellenos sin borde, barra de abajo flotante de vidrio (`backdrop-filter`), ventanas flotantes de vidrio con esquinas de 34 px, avisos (`toast`, `snack`) de vidrio. Los recibos en imagen también pasaron a azul (`ACC` y degradados de la sección "comprobantes"). El ícono y la animación de entrada siguen naranja (marca).
- **Lu-Ks:** nuevo `pubsrc/ios.css` (se agrega en `build_pub.py` después de `ui.css`) con lo mismo; respeta el modo claro/oscuro manual (`data-theme`). Nuevo color "Azul iOS" (`LKAC.ios`) que es el predeterminado; los otros colores siguen en Ajustes. Migración única: la primera vez pone "Azul iOS" (`lk-ios26`) y lo guarda también en la nube (`lk-ios26p` → `ajSave("color","ios")`) para que no lo cambie un color guardado antes.
- Capturas de revisión: `trev.js` (Tienda) y `tshotl2.js` (Lu-Ks), en claro y oscuro.
- Para volver al diseño anterior: quitar el bloque iOS de la Tienda y `ios.css` del build; en Lu-Ks elegir otro color en Ajustes.

## 27. Barra de abajo clásica de iPhone — 25 oct 2026
- Jose dijo que la barra flotante de vidrio "queda rara" (se veía la lista por debajo y alrededor, y los íconos de emoji en gris). Ahora, en las dos apps, la barra va pegada abajo de lado a lado, casi sólida (`color-mix(var(--bg) 94%)` + desenfoque), con una línea fina arriba y respetando la zona del gesto de inicio del iPhone.
- Tienda: los íconos de la barra son dibujos de línea (SVG) tipo iPhone en vez de emojis (billete, libreta, caja, recibo, gráfica); la pestaña activa va en azul. Lu-Ks: mismo estilo, con el botón + central.

## 28. Paquete "todas" (25 oct 2026): íconos de línea, pantalla, ícono nuevo, página y videos
1. **Íconos de línea en la Tienda:** los emojis de la pantalla (botones, tarjetas, filas, títulos, avisos) se cambian al dibujarse por íconos SVG de línea tipo iPhone. Diccionario `LI` (emoji → trazo), `liFix`/`liNode` y un `MutationObserver` sobre `body`. Solo cambia el emoji que va al **inicio** de un texto; no toca campos, el resumen en texto (`pre-wrap`), el escáner ni la animación de entrada. Los mensajes de WhatsApp y los recibos siguen con emojis. Fuente de los trazos: `work/icons.py`. Para no cambiar un elemento: `data-noli`.
2. **Pantalla (Tienda → Ajustes → 📱 Pantalla):** Apariencia (Automático / Claro / Oscuro → `lkt-theme`, atributo `data-theme` en `<html>`) y Tamaño de letra (Normal / Grande / Muy grande → `lkt-fs`, `data-fs`, `zoom` 1.12 / 1.24 en `#main` y `#sh`). Se aplica antes de pintar (script en `<head>`). Funciones `pantallaHTML`, `setPantalla`. Prueba: `tpant.js`.
   **Lu-Ks:** fila nueva "Tamaño de letra" (A / A / A, `luks-fs`, `data-fs`, `zoom` en `#app` y `.sheet>div`) debajo de "Tema" en Ajustes; traducción "Text size" en `dict.json`. Prueba: `tlfs.js`.
3. **Ícono nuevo estilo iOS 26:** Lu-Ks = fondo azul (#3EA2FF→#0047D6) con tarjeta de vidrio y gráfica que sube; Tienda = fondo azul-violeta (#5AC8FA→#2F7BFF→#5E3BE0) con local de toldo blanco. Fuentes SVG en `work/icon/luks.svg` y `work/icon/tienda.svg` (render con `work/icon/r.js`); PNG en `pubsrc/` (1024, 512, 192, apple-touch 180) y `pubsrc/tienda/` (512, 192, 180). Íconos anteriores en `work/icon/old/`. Manifiestos con fondo negro. Animaciones de entrada recoloreadas (Tienda: SVG `ts-mark`; Lu-Ks: reglas `#splash` en `ios.css`). **En el iPhone el ícono de la pantalla de inicio solo cambia si se borra y se vuelve a agregar la app.**
4. **Página de ventas:** `landing/conoce.src.html` en azul iOS con fuente del sistema (sin Google Fonts); capturas nuevas (`t-*.jpg` desde `promo/t-*.png`), portada nueva del video (`poster.jpg`, segundo 5) y `og.jpg` azul (`landing/og.html`). Subir `tienda/conoce.html`, `tienda/og.jpg` y `tienda/video.mp4`.
5. **Videos:** `promo/promo.css` y `promo/gen.py` recoloreados (fondo negro, azul #0A84FF, íconos nuevos); capturas regeneradas con `tshot.js` (Tienda) y `tshotl.js` (Lu-Ks). Mismo proceso: `python3 gen.py` → `node render2.js video-luks.html 25 frl` / `video-tienda.html 25 frt` → ffmpeg → música (`musica-luks.wav`, `musica-tienda.wav`) y voz (`voz/mezcla-luks.sh`, `voz/mezcla-tienda.sh`). Salidas: "Video Lu-Ks.mp4", "Video Lu-Ks Tienda.mp4", "… con voz.mp4" (−14 LUFS). Versiones naranjas anteriores en `promo/old-videos/`.

## 29. Sonidos revisados — 25 oct 2026
- **Tienda:** el "bip" del escáner creaba el audio en el momento de leer el código (no en un toque), y el iPhone no deja sonar audio que no se activó con un toque; además nunca lo reanudaba. Ahora `bipCtx()` crea/reanuda el audio, `bipUnlock()` lo activa en el primer toque (pointerdown/touchend/click) con un sonido mudo, se reanuda al volver a la app (`visibilitychange`), y el bip es más claro (2600 Hz, 0,13 s, como en los videos). Usa `audioSession.type="ambient"`: respeta el modo silencio del iPhone.
- **Lu-Ks:** `sndCtx()` ahora reanuda el audio si no está "running" (Safari usa el estado "interrupted" al volver de segundo plano), se reanuda al volver a la app y se activa en el primer toque con un sonido mudo.
- Prueba: `tsnd.js` (bip con cámara y con lectora, sonido al guardar en Lu-Ks, botón Probar, interruptor en No, audio tras volver a la app).

## 30. Revisión completa de Lu-Ks — 25 oct 2026
- Pedido de Jose: "en Lu-Ks revisa que todo funcione bien".
- **Flujos con valores** (`tlkflow.js`, 28 controles, todos bien): cuentas (débito, tarjeta, efectivo), gasto por el formulario con categoría y cuenta, ingreso, transferencia entre cuentas (saldos de origen y destino), gasto con tarjeta (deuda), editar monto (saldo se ajusta), eliminar (saldo vuelve), presupuesto, meta con abono, deuda "yo debo" y "me deben", pago recurrente, Inicio/Reportes/Movimientos muestran los valores, búsqueda, y que todo se conserva al recargar.
- **Recorrido automático** (`tlkcrawl.js`): toca los botones de las 6 vistas (Inicio, Finanzas, Ahorros y deudas, Movimientos, Reportes, Ajustes) y los de cada ventana que abren (105 toques) — sin errores. Salta los peligrosos (eliminar, cerrar sesión, importar…).
- **Revisión visual** (`tlkvis.js`, claro y oscuro): pantallas y 13 ventanas con el diseño iOS — bien. Historia "Tu semana" (`tlkstory.js`): avanza y se cierra bien. Tamaño del texto e idioma ES/EN (`tlkap2.js`): bien.
- **Arreglo:** la fila "Tamaño de letra" que se agregó a Lu-Ks en la sección 28 duplicaba una opción que ya existía (**Ajustes → Apariencia → Tamaño del texto**: Pequeño / Normal / Grande / Muy grande, variable `--z`) y chocaba con ella. Se quitó (fila, `lfsApply`, reglas `data-fs` de `ios.css` y la traducción). La Tienda sí conserva su opción en Ajustes → Pantalla.
- Pruebas viejas actualizadas: `tadm.js` (ahora elige el producto en el buscador de "Llegó mercancía"). `timp.js` necesita `lu-ks-mis-datos.json`, que no está en el proyecto (datos personales, no se comparte). `tmsg.js` ya fallaba antes.

## 31. Paquete de 6 mejoras (25 oct 2026, noche)
1. **Íconos de línea en Lu-Ks:** nuevo `pubsrc/li.js` (se agrega en `build_pub.py` después de `ui.js`) con el mismo sistema de la Tienda (emoji al inicio del texto → SVG de línea). No toca historias (`.lk-st`), animación de entrada, bienvenida (`#onb`), campos ni textos `pre-wrap`. Mapa extendido (banco, meta, salvavidas, ancla, maletín, repetir, paleta, micrófono, globo, brújula, correo, bocina, personas, montaña…). Estilos en `ios.css` (`.liw`, `.stile`, `.ico`).
2. **Gestos en la Tienda:** deslizar una fila (`#main .row[data-a][data-id]`) a la izquierda la abre (editar/ver); bajar la ventana `#sh` más de 110 px la cierra; jalar hacia abajo arriba de todo actualiza (`.ptr`). Solo con el dedo (touch).
3. **Movimiento suave en la Tienda:** transición al cambiar de pestaña (`#main.tt`), botones que se encogen al tocarlos, filas que se resaltan, y los montos grandes (`.hero .big`, `.kpi .v`) suben contando al abrir la pestaña (`cuenta()`). Respeta "reducir movimiento".
4. **Carga elegante en la Tienda:** `skelHTML()` (tarjetas grises animadas `.sk`). Si en ese celular ya se había entrado antes (`lkt-sk`), se muestra de una vez al abrir mientras confirma la cuenta y carga los datos (sin toques hasta que cargue); se borra al cerrar sesión.
5. **Revisión completa de la Tienda:** `ttflow.js` (día completo con números conocidos: ventas en efectivo/transferencia/fiado/por libra/monto rápido, abono, gastos de la caja y aparte, compra a crédito y pago, cuadre 35.000, cierre, reportes, recarga — 22 controles bien); `ttcrawl.js` (163 toques en las 6 vistas y sus ventanas, 0 errores); `tgest.js` (gestos, animaciones y esqueleto); `trev.js` (claro/oscuro).
6. **Página de presentación de Lu-Ks:** es la que ve quien abre `jimenez19pro.github.io/lu-ks/` sin sesión (`pubsrc/land.js`, `LKLAND`). Capturas nuevas en azul (ES e inglés, `tshotland.js` → `work/landshots/`), sección "Mírala en 25 segundos" con el video (`lu-ks-video.mp4` + `lu-ks-poster.jpg` en la raíz), letra del sistema e íconos de línea. Etiquetas Open Graph en `build_pub.py` (imagen `og-luks.jpg`, plantilla `work/og/og-luks.html`) para que el enlace salga con imagen en WhatsApp. `sw.js` de Lu-Ks ya no intercepta videos.

## Pendiente: dominio propio (pospuesto por Jose, 25 oct 2026)
- Le interesa, pero lo deja para cuando tenga el dinero. Precios aproximados: .com 45.000–60.000 COP/año; .co 120.000–150.000 COP/año.
- Pasos cuando lo retome: comprar (Namecheap / Porkbun / Cloudflare) → DNS: registros A a 185.199.108.153, .109.153, .110.153, .111.153 y CNAME `www` → `jimenez19pro.github.io` → GitHub **Settings → Pages → Custom domain** + **Enforce HTTPS** (y verificar el dominio en la cuenta) → **Firebase → Authentication → Authorized domains**: agregar el dominio (si no, falla "Continuar con Google") → actualizar URLs absolutas (`og:url`/`og:image` en `build_pub.py`, `landing/conoce.src.html`). Los usuarios deben volver a iniciar sesión y reinstalar el ícono; los datos están en la nube.

## 32. Tienda en computador — 25 oct 2026
- Solo la Tienda (Jose eligió no hacerlo en Lu-Ks). Se activa sola con pantallas de 960 px o más (`@media (min-width:960px)`); en el celular nada cambia.
- Barra lateral izquierda (232 px) con "Lu-Ks Tienda", pestañas y ayuda de atajos (`.kbh`). Contenido más ancho (1040 px), 4 tarjetas por fila. Ventanas como diálogos centrados. La venta (`#sh.wide`) muestra productos a la izquierda (en 2 columnas) y carrito/pago a la derecha (`.vgrid`, `.vcol`, `.vcart`).
- Atajos: F2 nueva venta, F3 fiar, F4 cobrar (guardar venta), F8 gasto, Esc cerrar ventana, / buscar, 1–5 cambiar de pestaña (solo fuera de campos de texto). La lectora USB/Bluetooth ya funcionaba (teclado rápido + Enter). Prueba: `tpc.js`.
- Para instalarla en el PC: abrir el enlace en Chrome o Edge → ícono de instalar en la barra de direcciones (o menú ⋮ → Instalar Lu-Ks Tienda).

## 33. Tienda en computador: 5 funciones más — 26 oct 2026
1. **Imprimir recibo** (`imprimirVenta`): botón "🖨️ Imprimir" en el detalle de la venta y, en computador, el botón del aviso después de vender. Tirilla de 80 mm por iframe + `print()`; sirve con impresora normal o de recibos instalada en el PC.
2. **Inventario en tabla** (`invTabla`, solo pantallas ≥ 960 px): columnas ordenables (Producto, Precio, Costo, Ganancia, Existencias) tocando el título (`INVS`), clic en una fila la abre. **Descargar Excel** (`invExport`, .xlsx con SheetJS `xlsx@0.18.5` cargado desde jsDelivr solo cuando se usa).
3. **Cargar desde Excel** (`invImport`, `ixLeer`, `ixOk`, `invPlantilla`): .xlsx/.xls/.csv; reconoce columnas Nombre, Precio, Costo, Existencias/Cantidad, Mínimo, Código por su título; no repite productos (mismo nombre o código); salta filas sin precio; entiende "2.600". Muestra vista previa antes de agregar.
4. **Pantalla para el cliente** (`cdOpen`, `cdUpdate`, solo en computador): ventana aparte (llevarla al segundo monitor) que muestra lo que va comprando y el total; al cobrar dice "¡Gracias por su compra!". Debe permitir ventanas emergentes.
5. **Consultar precio** (`precio`, F6 o botón en Inventario): escanear (cámara o lectora) o escribir nombre/código; precio en grande, existencias, precios por cantidad y botón "Venderlo".
- Prueba: `tpc2.js` con `xlsx-mock.js` (en este entorno no se puede descargar SheetJS; la librería real se carga en el navegador del usuario).

## Decisión de Jose (26 oct 2026)
- **Lu-Ks queda como está:** no proponer más mejoras para Lu-Ks; solo arreglos si algo falla. Las ideas nuevas van para Lu-Ks Tienda.

## 34. Permisos por empleado — 26 oct 2026
- Dueño: Ajustes → toca un empleado → interruptores: Fiar, Recibir abonos, Gastos y proveedores, Ver el inventario, Cerrar la caja. Vender siempre está permitido; por defecto todo activo.
- Se guarda en `t_miem/{empleado}.perm`; le llega al instante (`modoEmp` → `PERM`). `can(k)`, clases `body.np-*`, verificación en clics (`GK`), pestañas, F3/F8 y fiado en `guardarVenta`. Prueba: `tperm.js`. Control de la app (reglas de Firebase sin cambios).

## 35. Atajos de teclado más fáciles — 27 oct 2026
- Jose: en su computador los atajos "no hacían nada". Causa probable: en muchos portátiles F2/F3/F8 son teclas de brillo/volumen y necesitan Fn. Ahora hay atajos con letras (V venta, F fiar, G gasto, P precio, / buscar; solo cuando no se está escribiendo; se esperan 140 ms para no confundirlos con la lectora) y la lista de la barra lateral es tocable (`.kbh [data-kb]`, `kbDo`). F2–F8 siguen funcionando. Prueba: `tpc3.js`.

## 36. App nativa de Lu-Ks con Expo — etapa 1 (27 oct 2026)
- Jose pidió convertir las apps en apps nativas con Expo, **empezando por Lu-Ks** (la Tienda después).
- Plan: **Etapa 1** app que abre la web de Lu-Ks por dentro (con ajustes nativos) → **Etapa 2** pasar pantallas a nativo, Google y notificaciones nativas → **Etapa 3** publicar (App Store US$99/año, Google Play US$25).
- Repositorio nuevo **`jimenez19pro/lu-ks-app`** (no toca las apps publicadas). Expo SDK 58, React Native 0.88. Archivos en la raíz: `App.js`, `index.js`, `app.json` (Lu-Ks, `com.jimenez19pro.luks`, esquema `luks://`), `eas.json`, `package.json`, `package-lock.json`, `icon.png`, `icono-fondo.png`, `README.md`.
- `App.js`: WebView a `https://jimenez19pro.github.io/lu-ks/`. Código inyectado (`BRIDGE`): `window.LUKS_APP`; el botón `[data-au="google"]` no abre la ventana de Google (no funciona en apps) y muestra un aviso con la opción "¿Olvidaste tu contraseña?" para crear contraseña; las descargas `a[download]` con `blob:`/`data:` (respaldo JSON, fotos de garantías) se mandan a la app, que las guarda (expo-file-system) y abre Compartir (expo-sharing); `window.__lkBack` cierra la última `.sheet.on` (botón Atrás de Android). Navegación: solo `jimenez19pro.github.io` y los dominios de Firebase quedan adentro; WhatsApp, correo y otras páginas se abren afuera. Pantalla "Sin conexión" con Reintentar. Agente de usuario `LuKsApp/1.0.0`.
- Limitaciones de la etapa 1: sin "Continuar con Google", sin notificaciones push (el service worker no corre dentro de la app), sin modo sin conexión.
- Desde el entorno de Claude no hay acceso a los servidores de Expo (expo.dev); npm sí. Para instalar con Expo: `EXPO_OFFLINE=1 npx expo install <paquete>`. Para revisar que compila: `EXPO_OFFLINE=1 CI=1 npx expo export --platform ios --platform android`.
- Prueba del puente con la web real (Firebase falso): `pruebas/tbridge.js` (15 controles).
- Cómo se prueba en el iPhone sin computador: cuenta gratis en expo.dev → proyecto conectado al repo de GitHub → cada subida publica una actualización (EAS Workflows) → se abre en la app **Expo Go**.
- **Funcionando (27 oct 2026):** cuenta Expo de Jose, organización **`lu-ks-app`**, proyecto **`lu-ks`** (Project ID `6974df26-5e12-43c2-b9ed-94661531dc85`), conectado a GitHub `jimenez19pro/lu-ks-app`. Archivo `.eas/workflows/publicar.yml`: cada subida a `main` publica una actualización (EAS Update, rama `main`, `runtimeVersion` = versión del SDK para que abra en Expo Go). Jose la abre en **Expo Go** (Proyectos → Lu-Ks → main). Probado: entra, teclado, WhatsApp, sonidos (el primer sonido necesita un toque antes, por iOS). El botón flotante de engranaje que se ve en Expo Go es el menú de Expo Go; no sale en la app real.
- Para seguir: Jose solo sube los archivos cambiados a `lu-ks-app` y en 2–3 minutos la actualización está en Expo Go (cerrar y abrir). Lo que necesite módulos nativos que Expo Go no trae (Google nativo, widgets, Siri) solo se puede probar con una compilación propia (en iPhone requiere la cuenta de Apple de US$99).

## 37. App nativa de Lu-Ks — etapa 2 (28 oct 2026)
Jose eligió 3 de 4 ideas (no eligió "vibración"): **Inicio nativo**, **abrir más rápido** y **recordatorios en el celular**. Archivos nuevos en `lu-ks-app` (todos en la raíz): `inicio.js`, `puente.js`, `copia.js`, `recordatorios.js`, `almacen.js`; cambiaron `App.js`, `app.json` (plugin `expo-notifications`), `package.json`, `package-lock.json`, `README.md`. Librerías nuevas (todas funcionan en Expo Go): `react-native-svg`, `expo-linear-gradient`, `expo-notifications`.
- **Puente (`puente.js`, función `puente({version, os, recordatorios})`)**: el código que se inyecta en la web. Además de lo de la etapa 1, manda mensajes `state` ({show, out, tab, snap}) cada vez que cambia algo (MutationObserver + 90 ms; vuelve a revisar a los 800 ms). `snap` se arma **leyendo el Inicio ya dibujado por la web** (textos, colores calculados, íconos SVG de categorías y barra, tema `--bg --sf --sf2 --ink --mut --ac --neg --pos --ln --z`), así el idioma, el ojito (montos ocultos), los colores y el tamaño de letra salen iguales sin repetir cálculos. Los números que "cuentan" se leen de `data-cu`. `show` = pestaña `home`, vista `HV=fin`, hay `.hd`, y nada tapa la pantalla (se prueba con `elementFromPoint` en 3 puntos; `#splash` y `#toast` no cuentan). `out` = la pantalla `#auth` está visible (ojo: es `position:fixed`, por eso se usa `getClientRects`, no `offsetParent`). Toques: `window.__lkTap(selector)` hace clic en el elemento de la web; `__lkRefresh()` = `MCP.invalidate`; `__lkState()` fuerza reenvío.
- **Inicio nativo (`inicio.js`)**: encabezado (inicial → Ajustes, fecha y puntico de sincronización, saludo → cambiar nombre, menú), frase, Finanzas/Ahorros y deudas, período (chip y ‹ ›), tarjeta principal con degradado del color del tema y ojito, General (Mis cuentas con tarjetas de crédito en su color y aviso de "cuenta suelta" con Arreglar, Saldo total/Gastos, Hoy puedes gastar con ¿Me alcanza?/Plan de sueldo, proyección del mes), Presupuestos, Próximos pagos (con Registrar), Últimos movimientos y barra de abajo igual a la web con el +. Deslizar hacia abajo actualiza. En `App.js` el Inicio nativo va **encima** del WebView: se ve cuando `show` es verdadero; al tocar algo que abre ventana u otra pestaña se oculta al instante y la web hace lo suyo; el ojito, ‹ ›, y plegar secciones se quedan en el nativo. "Ahorros y deudas" sigue siendo web.
- **Abrir más rápido (`copia.js`)**: después de abrir, la app baja `index.html` (con `If-None-Match`/ETag de GitHub Pages), mete adentro `config.js` y los 4 scripts de Firebase (`armar`), quita las tipografías de Google y guarda `copia-lu-ks.html`. La próxima vez carga con `source={{html, baseUrl:'https://jimenez19pro.github.io/lu-ks/'}}` → misma dirección, misma sesión. Si en 15 s no arranca (no llega `out`, `snap` ni otra pestaña), la marca como mala y abre la página normal. También guarda la última foto del Inicio (`inicio.json`) y la muestra al instante al abrir (punto amarillo hasta que llegan los datos nuevos). Al cerrar sesión se borra.
- **Recordatorios (`recordatorios.js`)**: la web manda `rem` con pagos fijos (próximos 3 meses; el de este mes no si ya se pagó, con la misma regla de nombres de `lkFrase`), pagos de tarjeta (`a.rec && a.pago`), próxima cuota (`cuoCalc().nx`) y garantías (`LKGAR.all()`, fecha + meses). La app programa avisos locales (`expo-notifications`): víspera 7 p. m. y el día 8 a. m.; garantías 30/7/0 días a las 9 a. m.; máximo 60 (iOS permite 64); sin montos si el ojito está cerrado; en inglés si la app está en inglés. Reprograma solo si cambian los datos o cambia el día. La primera vez que hay pagos pregunta "¿Te recuerdo tus pagos?". En Ajustes aparece la fila "⏰ Recordatorios en el celular" (Sí/No y Probar) debajo de Sonidos, solo dentro de la app (`#lkapp-rem`). Preferencias en `prefs.json` (`rem`, `remFirma`).
- **Pruebas** (en `pruebas/`, correr desde la raíz del proyecto con `LUKSAPP=../lu-ks-app`): `tinicio.js` (48 controles: datos del Inicio, cuándo se muestra, toques, recordatorios, fila en Ajustes, oscuro, salir; guarda `snaps/*.json` y capturas), `tcopia.js` (13: arma la copia y la abre sin acceso a scripts de internet, sesión al recargar), `trecordatorios.js` (14: avisos con los datos de `snaps/rem.json`). Vista previa del Inicio nativo en el navegador: copiar el proyecto, instalar `react-native-web react-dom @expo/metro-runtime`, un `index.js` que pinte `<Inicio snap={...}>` y `npx expo export --platform web` (así se tomaron las capturas para comparar con la web).
- **Arreglos (28 oct 2026):** la fila de Ajustes ahora dice "Recordatorios" con ícono de línea (antes se cortaba el título). La animación de bienvenida de la web (`#splash`) quedaba escondida detrás del Inicio nativo y aparecía al tocar Ajustes: ahora, si la app abre mostrando el Inicio guardado, el puente la quita al cargar (`puente({sinSplash:true})` → `sinAnimacion()`). Prueba: `SINSPLASH=1 node tinicio.js`.

## 38. App nativa de Lu-Ks — Movimientos y formulario nativos (29 oct 2026)
Jose eligió 1 y 2 de la lista (Movimientos nativo y formulario de anotar nativo); la 3 (Lu-Ks Tienda nativa) queda para después. Archivos en `lu-ks-app`: nuevos `movimientos.js` y `formulario.js`; cambiaron `App.js`, `puente.js`, `inicio.js`, `app.json` (plugin `@react-native-community/datetimepicker`), `package.json`, `package-lock.json`, `README.md`. Librería nueva: `@react-native-community/datetimepicker` (viene en Expo Go).
- **Puente**: `state` ahora trae `kind` ('home' | 'tx'), `snap` (Inicio) y `tx` (Movimientos). `snapTx()` lee la pestaña Movimientos ya dibujada por la web: encabezado y balance, período, Filtro (con conteo), buscador (`Q`), recientes (`[data-qr]`), Este período/Todo el historial, resumen de resultados (`.qsum`), Categorías/Cuentas (`[data-sv]`), tarjetas (`.tile[data-tl]`) y la lista `#txlist` (días + filas). Máximo 300 filas (`TXMAX`, `__lkMore()` suma 300); los íconos de las filas van una sola vez en `list.icons`. `__lkQ(texto, guardar)` busca igual que el buscador web (`Q`, `qPaint`, `qRecSave`). Mientras se ve Inicio o Movimientos, la web no hace la animación de "contar números" (`sinConteo` envuelve `lkCount`) y las barras se leen de `data-w`.
- **Formulario**: `__lkFormData()` manda categorías (con ícono, subcategorías y traducción `LKTR`), cuentas (`allAcc` + `accDisp`, "Sin cuenta" primero), cuenta principal, hoy/ayer, tipos y si hay Foto (misma prueba que la web: `claude.use('sample').limits().images`). `__lkSave(d)` llena el formulario web escondido (`TP`, `#f-amt`, `#f-mer`, `#f-cat`, `#f-sub`, `#f-acc`, `#f-dst`, `#f-date` + `PFDATE`) y toca `#save`: así se guarda con la misma lógica (categoría automática con `guess`, cola sin conexión, sonidos). `__lkFormWeb('ai'|'scan'|'cats'|'acc')` abre Notas IA, escanear recibo, Ajustes → Categorías o Nueva cuenta de la web. Con `formNativo:true`, el `#add` de la web (pestañas que siguen siendo web) también abre el formulario nativo (clic capturado). Los avisos `#toast` de la web se reenvían a la app (`toast`) y se muestran encima de lo nativo.
- **App.js**: debajo va la web; encima, `Inicio` o `Movimientos` según `kind`. Las pestañas Inicio y Movimientos cambian sin ocultar lo nativo (`onNav`); Reportes y Ajustes son web. Si llega `show` justo después de tocar algo, se aplica a los 360 ms (antes se ignoraba). El formulario es un `Modal` `pageSheet` (hoja de iPhone, se cierra deslizando).
- **Pruebas**: `tmov.js` (41 controles: datos del formulario, + de la web, guardar gasto/subcategoría/fecha de ayer/categoría automática/ingreso/transferencia, Movimientos nativo, buscar, recientes, tarjetas, cuentas, filtro, detalle, 300 filas y cargar más, Inicio ↔ Movimientos); `tinicio.js` actualizado (Movimientos ya es nativo, 49 controles). `trecordatorios.js` ya no depende del número de cuota.
- **Arreglo (30 oct 2026):** la fila "Recordatorios" de Ajustes parpadeaba al tocar un color (la web vuelve a dibujar Ajustes con `render()` y la fila se volvía a poner ~100 ms después). Ahora el puente la vuelve a poner en el mismo instante: dentro del `MutationObserver` y envolviendo `render` (`conFila`). Prueba: `tparpadeo.js` (toca los 7 colores y revisa cuadro por cuadro que la fila no falte).

## 39. App nativa de Lu-Ks — editar, deslizar y Reportes nativos (30 oct 2026)
Jose eligió 2, 3 y 4 (descartó por ahora "Lu-Ks Tienda nativa"). Archivos en `lu-ks-app`: nuevo `reportes.js`; cambiaron `App.js`, `puente.js`, `movimientos.js`, `formulario.js`, `package.json`, `package-lock.json`, `README.md`. Librería nueva: `expo-clipboard` (viene en Expo Go). El deslizar se hizo con `PanResponder` de React Native (sin librerías extra).
- **Editar**: `__lkEditData(id)` manda `edit` (tipo, monto, descripción, categoría, subcategoría, cuenta, destino, fecha) y los datos del formulario; el clic en `#cedit` (detalle del movimiento) se captura y abre el formulario nativo. `__lkSave({..., edit:id})` pone `EDIT` = ese movimiento y `PFDATE` solo si cambió la fecha → la web usa `updateRow`. El formulario en modo edición dice "Editar movimiento", "Guardar cambios", no muestra IA/Foto y tiene "Eliminar movimiento" (con confirmación).
- **Eliminar**: `__lkDel(id)` → `CUR`, `DELQ=true`, `delRow()` (la misma de la web). Movimientos pendientes (sin conexión) muestran el aviso de la web y no se editan ni borran.
- **Deslizar**: en Movimientos nativo cada fila se desliza a la izquierda (Editar azul, Eliminar rojo); solo una abierta a la vez; se cierra al hacer scroll o al tocarla.
- **Reportes nativo ("espejo")**: `snapRep()` recorre `#app` y arma un árbol: cajas (`v`) con estilos calculados (flex, gap, márgenes, rellenos, fondo, bordes, radios, posición absoluta, grid → filas con columnas), textos en línea (`p` con pedazos: texto, íconos y puntos de color), gráficas `svg` con colores y letras ya resueltos (`svgListo`: sin `var(--…)` ni `style`), campos (`in`) y botones con su ruta (`#app > :nth-child(…)`). Elementos dentro de filas flex se tratan como cajas; círculos (ancho = alto y radio ≥ mitad) conservan su tamaño; botones de ancho completo siguen anchos. `__lkIn(sel, texto)` y `__lkEnter(sel)` escriben en campos de la web (chat financiero). En `reportes.js` los toques no ocultan lo nativo (si se abre una ventana, el puente lo detecta y se oculta); "Copiar al portapapeles" (`#expcopy`) copia el CSV con `expo-clipboard`. En Reportes tampoco se "cuentan" los números (`sinConteo`).
- **Arreglo importante en `puente.js`**: dentro de la plantilla de texto (backticks) las expresiones regulares necesitan doble barra (`\\s`, `\\w`, `\\(`); con una sola, JavaScript la quita (`/\s+/` quedaba `/s+/` y borraba las "s"). Al editar el puente, revisar con `grep -n '[^\\]\\[sSwd()]' puente.js` (no debe salir nada).
- **Pruebas**: `trep.js` (35 controles: Reportes con todas sus secciones, gráficas SVG limpias, chip de hormiga, Descargar y CSV, chat, herramientas que abren ventana, editar sin duplicar, cambiar fecha, "Editar" del detalle, eliminar; también con `DARK=1`). `tmov.js` actualizado (Reportes ya es nativo). Todas: `tinicio` 49, `tmov` 41, `trep` 35, `tcopia` 13, `trecordatorios` 14, `tparpadeo` OK.
- **Arreglos en Reportes nativo (30 oct 2026, tarde):** en el iPhone la gráfica de barras mostraba una sola barra y los íconos amontonados: `react-native-svg` no ubica bien un `<svg>` dentro de otro ni textos vacíos. `svgListo` ahora convierte los `<svg>` anidados en `<g transform="translate scale">` y quita `<text>` vacíos. Los botones de la web centran su contenido (flecha de enviar del chat). Después del punto de color de la leyenda del pastel va un espacio. Las barras y meses de las gráficas son tocables (zonas `z` sobre el SVG; `__lkTap` usa `dispatchEvent` cuando el elemento no tiene `.click()`). `trep.js` ahora tiene 40 controles.

## 40. App nativa de Lu-Ks — Ahorros, Ajustes, detalle, cuentas, menú (30 oct 2026, noche)
Jose pidió "Realiza todo" de esta lista: Ahorros y deudas nativo, detalle del movimiento nativo, ficha de cada cuenta, Ajustes nativo y menú de navegación nativo. Archivos en `lu-ks-app`: nuevos `hojas.js` y `selector.js`; cambiaron `App.js`, `puente.js`, `inicio.js`, `movimientos.js`, `formulario.js`, `reportes.js`, `package.json`, `package-lock.json`, `README.md`. Librerías nuevas (vienen en Expo Go): `expo-image-picker`, `expo-document-picker`.
- **Espejo generalizado** (`snapEsp`, antes `snapRep`): ahora también para la vista "Ahorros y deudas" del Inicio (`kind:'aho'`) y la pestaña Ajustes con sus subpáginas (`kind:'con'`). En la app hay un espejo por tipo (`esp[kind]`). El espejo ahora entiende `select` (lista nativa; `__lkSel`), casillas/radios (`chk`), tipos de campo (teclado numérico, correo, contraseña), degradados (`grad` → `LinearGradient`, por ejemplo los círculos de "Color de la app"), anillos de selección hechos con `box-shadow` (`ring` → borde) y cuadritos de íconos (cuadrados ≤ 64 px con fondo conservan su tamaño). No requiere `h1`.
- **Detalle del movimiento**: el puente envuelve `openCat` (`conDetalle`): si el movimiento no está pendiente, en vez de abrir `#csheet` manda `det` (monto con signo, fecha y hora, categoría, cuenta/destino con ícono, chips de categorías). `__lkCat(id,k)` cambia la categoría (simula el chip `data-cat`), `__lkDup(id)` abre el formulario nativo lleno como nuevo con fecha de hoy (`edit` con `nuevo:1`; el formulario lo trata como nuevo), Editar y Eliminar como antes. En Movimientos e Inicio los movimientos abren esta hoja (`__lkDet`).
- **Ficha de cuenta**: el Inicio nativo llama `__lkCuenta(i)` al tocar una cuenta (ya no abre el editor web). Manda saldo (o usado/cupo/disponible/% y día de pago en tarjetas), entró/salió del mes (transferencias cuentan como entrada en la cuenta destino), y hasta 60 movimientos con signo relativo a la cuenta. "Ver en Movimientos" = `__lkVerCuenta(n)` (`FL.acc`), "Editar cuenta" = `__lkEditAcc(i)` (`openAcc`).
- **Menú**: el puente envuelve `navOpen` (`conMenu`) y manda `menu` con `NAVS`/`NAVI` (íconos `svg()`, traducidos). Buscador local igual a la web (`nz`). Al elegir: `__lkNav(k)` (`navGo`) y la app desplaza la pantalla espejo hasta el título que empieza con `NAVI[k][2].h` (`irA`).
- **Fotos y archivos**: el puente reemplaza `HTMLInputElement.prototype.click` para `type=file` (la web dentro de la app no puede abrir el selector sin un toque directo): manda `pick` (accept, multiple, capture) y la app usa la cámara/Fotos (`expo-image-picker`, calidad 0,6, JPEG) o Archivos (`expo-document-picker`), y devuelve con `__lkPicked(id, [{name,type,b64}])`, que arma `File`s con `DataTransfer`, los pone en el campo y dispara `change`. Sirve para Foto del recibo, garantías e importar respaldo.
- **Ojo con las barras invertidas**: en `puente.js` (plantilla con backticks) las expresiones regulares van con doble barra (`\\s`, `\\d`, `\\b`, `\\(`). Revisar con `grep -n '[^\\]\\[sSwdb()]' puente.js` (no debe salir nada).
- **Pruebas**: nueva `tmas.js` (37 controles: Ahorros nativo, volver a Finanzas, Ajustes nativo con Sonidos y Gestión rápida, círculos de color con degradado y anillo, detalle con datos, cambiar categoría, repetir, detalle de transferencia, cuenta débito y tarjeta, Ver en Movimientos, menú, ir a Ahorros/4x1000/Reportes, selector de fotos). `tinicio.js` y `tmov.js` actualizadas. Todas: `tinicio` 49, `tmov` 41, `trep` 40, `tmas` 37, `tcopia` 13, `trecordatorios` 14, `tparpadeo` OK (también con `DARK=1`).

## 41. App nativa de Lu-Ks — ventanas nativas, garantías, Notas IA y lista para publicar (31 oct 2026)
Jose pidió "Realiza todo": formularios nativos (cuentas, presupuestos, metas, pagos recurrentes), garantías nativo, Notas IA nativo y dejar la app lista para publicar. Archivos en `lu-ks-app`: cambiaron `App.js` (VERSION 1.5.0), `puente.js`, `reportes.js`, `formulario.js`, `app.json`, `eas.json`, `README.md`; nuevos `store.config.json`, `.eas/workflows/tiendas.yml` y la carpeta `publicar/` (COMO-PUBLICAR.md, TEXTOS-TIENDA.md, capturas, gráfico e íconos). En el repo `lu-ks` cambió `privacidad.html` (sección "App de iPhone y Android").
- **Ventanas de la web como hojas nativas** (`HojaEspejo` en `reportes.js`): el puente busca la ventana `.sheet.on` de más arriba (`hojaArriba`, por `z-index`; menos `#sheet` y `#csheet`, que ya son nativas) y manda su espejo en `state.hoja` ({id, tree, th}). La app la muestra como hoja de iPhone (`Modal` `pageSheet`, se cierra deslizando → `__lkCerrarHoja(id)`), con lo nativo de atrás todavía visible (`covered()` ignora esa ventana). Así funcionan: nueva cuenta (`a1sheet` tipo → `a2sheet` → `bksheet` bancos), presupuesto (`bsheet`), meta (`gsheet`, fecha con calendario nativo `CampoFecha`), deuda (`dsheet`), pago recurrente (`rsheet`, listas nativas), garantías (`xsheet`, con fotos: `img` y `bgimg`) y Notas IA (`nsheet`). `__lkIn(sel, v, cambio)` también dispara `change` (fechas y listas).
- **Notas IA → formulario nativo**: el puente envuelve `prefill` (`conPrefill`): lo que entendió la IA se lee del formulario web, se cierra `#sheet` y se abre el formulario nativo lleno (`edit` con `nuevo:1` y `tit`).
- **Interruptores**: cajas sin texto ≤ 80 px con fondo/borde conservan su tamaño (antes "Cuenta principal" salía como un punto).
- **Google escondido en la app**: CSS inyectado oculta `[data-au="google"]` y la rayita "o" (Apple rechaza botones de inicio de sesión que no funcionan y pediría "Iniciar sesión con Apple").
- **Publicar**: `app.json` sin permiso de micrófono (`expo-image-picker` con `microphonePermission:false`, Android `blockedPermissions RECORD_AUDIO`), textos de cámara/fotos más claros. `eas.json`: la compilación de tienda usa el canal `main` (el mismo al que publica `publicar.yml`; si no, la app de tienda no recibiría actualizaciones), Android a prueba interna como borrador. `tiendas.yml` arma iOS y Android y los envía (`submit` con `build_id`) solo cuando cambia `publicar/LANZAR.txt` (ese archivo NO se entregó para no disparar un armado sin cuentas). Falta cuando Jose tenga las cuentas: `ascAppId` en `eas.json` → `submit.production.ios`, llave API de App Store Connect y cuenta de servicio de Google subidas en expo.dev. Capturas con datos de ejemplo ("Ana"), 1290×2796 y 1080×1920, generadas con la vista previa web de las pantallas nativas.
- **Pruebas**: nueva `tventanas.js` (21 controles: cuenta con banco y saldo, presupuesto, meta con fecha, recurrente con listas, cerrar deslizando, garantías con foto, Notas IA → formulario nativo). Las que esperaban "se oculta lo nativo" al abrir una ventana ahora aceptan la hoja nativa. Todas: `tventanas` 21, `tinicio` 49, `tmov` 41, `trep` 40, `tmas` 37, `tcopia` 13, `trecordatorios` 14, `tparpadeo` OK (también con `DARK=1`). `tbridge.js` es de la etapa 1 y ya no aplica.

## 42. App nativa de Lu-Ks — diseño nuevo estilo "banco moderno" (31 oct 2026, tarde)
Jose mandó dos videos de ejemplo (una app de banco hecha en Flutter) y pidió "Todas" las ideas: Inicio con encabezado oscuro, botones rápidos, barra de gastos por colores, carrusel de cuentas, agregar cuenta con vista previa en vivo, bienvenida animada y barra de abajo flotante. No se copió el logo ni la marca del video, y no se piden números de tarjeta (Lu-Ks no los necesita). Archivos en `lu-ks-app`: nuevos `cuentas.js`, `nuevacuenta.js`, `bienvenida.js`; cambiaron `App.js` (VERSION 1.6.0), `puente.js`, `inicio.js`, `formulario.js`, `README.md` y las capturas de `publicar/`. No hay librerías nuevas (animaciones con `Animated` de React Native).
- **Puente**: `snap()` ahora manda `gastos` (gastos del período por categoría con `sumBy(mtx())`, color `CATS[k][2]`, ícono, de mayor a menor) y `rapido` (textos de los botones). `cuentaData(i, n)` arma la ficha de una cuenta (antes dentro de `__lkCuenta`) y agrega color (`a.color` o `PAL`), banco, apodo, corte, principal y tipo traducido. `__lkCuentas(at)` manda todas las cuentas (`cuentas`) para el carrusel. `__lkVerCat(k)` abre Movimientos filtrado por esa categoría. `__lkAccForm(i)` manda `accform` (datos del formulario como `AF`/`afNew`, bancos con su logo `bkTile`, colores `CLR`, redes y el saldo de hoy al editar). `__lkSaveAcc(d)` llena `AE`, `ADEL`, `AF` y llama `saveAcc2` (misma validación y guardado de la web) y responde `accsaved` {ok, i, msg}; el mensaje de error se toma envolviendo `toast` mientras guarda. Con la app, `openAcc` se reemplaza (`conCuentaNativa`): "Agregar cuenta" y "Editar cuenta" de cualquier parte abren el formulario nativo. `state()` manda `land` cuando se ve la portada (`.ld-h`) para mostrar la bienvenida nativa.
- **Inicio** (`inicio.js`): fondo oscuro con degradado (mezcla del color de acento con `#0A0E1F`), saldo grande con `Monto` (cuenta desde $0 la primera vez y anima cada cambio; respeta el separador de miles del texto), botones rápidos (Anotar = formulario, Transferir = formulario con tipo transferencia y cuenta principal, Cuentas = carrusel, Más = menú). La hoja clara (`st.hoja`) sube sobre el encabezado: el encabezado se mueve a la mitad de velocidad y se desvanece; arriba aparece una barra compacta con el saldo. Tarjeta "Gastos" con barra multicolor (crece al abrir) y leyenda tocable.
- **Barra de abajo** (`BarraAbajo` en `inicio.js`, la usan todas las pantallas nativas): píldora flotante con las 4 pestañas y un resaltado que se desliza con resorte desde la pestaña anterior (`ultimoTab`); el + va aparte a la derecha. `altoBarra` = 72 + borde inferior.
- **Carrusel** (`cuentas.js`): `Modal` a pantalla completa, `FlatList` horizontal con `snapToInterval`; cada tarjeta escala, gira y se aclara según su posición. Debajo: saldo animado (usado/cupo/disponible en tarjetas), entró/salió del mes, información, Ver en Movimientos, Editar cuenta y movimientos. `Plastico` dibuja la tarjeta (degradado del color, brillo, logo del banco, red Visa/Mastercard/Amex dibujada, texto oscuro si el color es claro). Tocar una cuenta en Inicio abre el carrusel en esa cuenta.
- **Formulario de cuenta** (`nuevacuenta.js`): hoja `pageSheet` con vista previa en vivo (color del banco en débito, color elegido en crédito, dorado en efectivo), tipo, banco (lista con buscador), apodo, red, color, cupo, días de corte y pago (la tarjeta se voltea y muestra el dorso con esos días), recordatorio, saldo/deuda de hoy, cuenta principal y "Opciones avanzadas" (nombre en tus movimientos). Valida igual que la web. Al guardar: pantalla oscura con la tarjeta, 3 pasos (Revisar → Guardar → Lista) y barra de progreso; cuando la web confirma, "¡Cuenta agregada!" con "Ver cuenta" (abre el carrusel en ella) y "Listo". En edición: "Guardar cambios" y "Eliminar cuenta".
- **Bienvenida** (`bienvenida.js`): sobre la portada de la web. Dos piezas azules se juntan, aparece el ícono, un degradado barre en diagonal, el ícono sube y aparecen el mensaje, "Crear mi cuenta gratis", "Ya tengo cuenta" e idioma (tocan los botones de la web). La animación completa sale una vez por apertura.
- **Pruebas**: nueva `tdiseno.js` (22 controles: portada → bienvenida, formulario nativo al agregar cuenta, bancos/colores/redes, crear débito/tarjeta/efectivo, tarjeta sin cupo rechazada con su mensaje, gastos por categoría, botones rápidos, carrusel con usado/cupo/disponible/saldo, editar sin tocar la deuda, eliminar, ver categoría). `tventanas.js` actualizada (la cuenta ya no es ventana web). Todas: `tdiseno` 22, `tventanas` 16, `tinicio` 49, `tmov` 41, `trep` 40, `tmas` 37, `tcopia` 13, `trecordatorios` 14, `tparpadeo` OK (también con `DARK=1`).

## 43. Arreglos del diseño nuevo (31 oct 2026, noche)
Jose reportó: (1) no se veía la animación de entrada, (2) las tarjetas no se movían como en el video y (3) al pasar de Inicio a Ajustes se superponía "la configuración anterior con la opción de Movimientos". Archivos: `App.js`, `puente.js`, `inicio.js`, `cuentas.js`, `bienvenida.js`.
- **Causa de (3)**: la primera vez que se abre una pestaña nativa después de abrir la app todavía no hay su "espejo"; la app se ocultaba y por un instante se veía la página web (Ajustes con la barra de abajo vieja: Inicio, Movimientos, +…), y luego la pantalla nativa aparecía encima. Arreglo: `onNav` ya no oculta lo nativo; marca `navPend`, muestra un fondo con la barra flotante y un indicador mientras llega la pantalla (máximo 2,5 s; después muestra lo que haya), y mientras tanto ignora estados que no sean de esa pestaña. Además, dentro de la app la barra de la web queda invisible (`body > nav{visibility:hidden}` en el CSS que inyecta el puente; los toques con `__lkTap` siguen funcionando).
- **Animación de entrada**: `Intro` (en `bienvenida.js`) sale cada vez que se abre la app con sesión iniciada: dos piezas forman el ícono, un degradado barre en diagonal y se desvanece (se salta tocando). Luego el Inicio entra escalonado (`listo`, una vez por apertura): encabezado, saldo que cuenta desde $0 (`Monto` con `pausa`), botones que saltan uno por uno, la hoja clara sube y la barra de gastos crece. La bienvenida completa (`Bienvenida`) sigue saliendo cuando no hay sesión.
- **Carrusel como en el video**: ya no es una lista; es un "mazo" propio: todas las tarjetas en el mismo sitio, movidas por un solo valor `pos` (0 = primera) con `PanResponder` (sigue el dedo, resistencia en los bordes, al soltar se acomoda con resorte según la velocidad). Las de los lados quedan detrás (más pequeñas, oscurecidas y giradas), la del centro siempre encima (`zIndex`), el saldo cambia mientras deslizas, las tarjetas entran desde la derecha al abrir, los puntos se pueden tocar y hay una forma diagonal del color de la tarjeta al fondo. ("Agregar cuenta" es opaca.)
- **Pruebas**: `tdiseno.js` ahora 23 controles (revisa que la barra web no se vea). Todas en verde, claro y oscuro.

## 44. Apple Pay y SMS del banco automáticos (31 oct 2026, noche)
Jose pidió opciones como las de la app "AI Money" (capturas: SMS bancario automático, Apple Pay automático, "Cuentas y tarjetas" con cuenta por defecto y reglas por últimos 4 / palabra / nombre de tarjeta en Wallet). Eligió: **en silencio** (sin abrir Lu-Ks), **guardar directo** y **las dos** (Apple Pay y SMS). Archivos: nuevo `pubsrc/auto.js` (se agrega en `build_pub.py` después de `gar.js`), `pub/index.html` (repo lu-ks, raíz), `App.js` (mensaje `copy` → portapapeles del celular), prueba `pruebas/tauto.js`, reglas nuevas de Firebase.
- **Cómo llega el dato sin servidor**: cada usuario tiene una llave secreta de 28 letras (`ajG("atajo")`) y un buzón `buzon/{llave}` = {uid}. El Atajo de iOS hace `POST https://firestore.googleapis.com/v1/projects/lu-ks-ia/databases/(default)/documents/buzon/{llave}/items?key={apiKey}&documentId={dato}` con cuerpo `{}`: el dato va en el NOMBRE del documento: `ap~MONTO~COMERCIO~TARJETA~FECHA` o `sms~TEXTO~FECHA` (fecha ISO 8601; el atajo cambia "/" por "-" y codifica la URL). Las reglas solo dejan CREAR documentos vacíos en un buzón que exista (nadie puede leer buzones ajenos). Si el mismo dato llega dos veces, Firestore lo rechaza (mismo nombre).
- **Cuándo se vuelve movimiento**: Lu-Ks (app o web) escucha su buzón con sesión iniciada; cuando ya cargaron cuentas y ajustes y la página está visible, procesa cada dato: lee monto/comercio/tarjeta (`parseAp`) o el SMS (`parseSms`: monto con $ / COP / separadores, comercio después de "en", nombre después de "a" (envíos) o "de" (ingresos), últimos 4 con "*1234", "terminada en", "T.Cred"; ingresos por "recibiste/abono/consignación…"; ignora códigos/claves), elige la cuenta (`cuentaPara`: reglas — 4 dígitos para SMS, si no palabra; sin tildes ni mayúsculas; gana la más larga — si no la cuenta por defecto, si no la principal), adivina la categoría (`guess`), guarda con `qSend` (fuente `applepay` o `sms`, con cola sin conexión), borra el dato del buzón y muestra un aviso. Si llega sin monto no guarda nada y marca `sinMonto` (aviso rojo como el de AI Money). Sin servidor, si Lu-Ks está cerrada en todos los dispositivos, los datos esperan en el buzón hasta abrirla.
- **Pantallas** (web, y en la app como hojas nativas por el espejo): Ajustes → "Automatización en iOS" (SMS bancario automático / Apple Pay automático, con estado "Activo · último hace…" o aviso). Cada una: estado, "¿Cómo funciona?" (3 pasos), "Configurar cuentas y tarjetas" (cuenta por defecto, "Tus tarjetas de Wallet" vistas con su cuenta, reglas con + y ✕) y "Activar" (crea la llave, "Copiar mi enlace" y pasos para crear la automatización en Atajos: Transacción o Mensaje → Texto → Reemplazar texto → Codificar URL → Obtener contenido de URL (POST)). `LINKS` en `auto.js` queda listo para poner los enlaces de iCloud si Jose arma y comparte los atajos (entonces los pasos se acortan). Ajustes guardados en `ajG("autoCfg")` (JSON).
- **Reglas de Firebase nuevas** (agregar antes de las dos llaves finales; ver `reglas-buzon.txt`):
```
    match /buzon/{llave} {
      allow create: if request.auth != null && request.resource.data.uid == request.auth.uid && llave.size() >= 24;
      allow read: if request.auth != null && resource.data.uid == request.auth.uid;
      match /items/{item} {
        allow create: if item.size() <= 1400 && request.resource.data.size() == 0
          && exists(/databases/$(database)/documents/buzon/$(llave));
        allow read, delete: if request.auth != null
          && get(/databases/$(database)/documents/buzon/$(llave)).data.uid == request.auth.uid;
      }
    }
```
- **Sin probar contra Firebase real** (desde aquí no hay acceso a Google): la llamada REST con `documentId` y cuerpo `{}`. Prueba: en Atajos, tocar ▶ en el atajo: si responde un JSON con `"name": ".../buzon/..."` funciona; si dice PERMISSION_DENIED faltan las reglas.
- **Pruebas**: `tauto.js` (35 controles: SMS de Bancolombia, Nequi, Davivienda, BBVA, envíos, transferencias, formato 25,000.50, códigos ignorados; sección en Ajustes, llave y buzón, enlace, copiar, reglas, cuenta por defecto, Apple Pay guardado con regla / por defecto / sin monto con aviso, SMS de compra con *1234, ingreso por palabra, clave ignorada, estado "Activo"). Todas las demás en verde.

## 45. Automatizaciones en "Pronto" (31 oct 2026, noche)
Probado con Jose en su iPhone (iOS 26): su automatización Wallet → Texto → Reemplazar → Codificar URL → Obtener contenido (POST) **funciona**: Firebase respondió `"name": ".../buzon/<llave>/items/ap~~~~2026-10-31T21:12:48-05:00"` (prueba sin pago, por eso sin monto). Las reglas `buzon` están publicadas. En iOS 26 la automatización se llama **Wallet** ("Cuando se use cualquier tarjeta"; Automatización encendido, Notificar apagado) y los campos de la Entrada de shortcut con **Tipo: Transacción** son **Cantidad**, **Comercio**, **Tarjeta o pase**, **Nombre**; "Mostrar resultado" se llama **Mostrar contenido**. La automatización de Jose se llama "Mi Apple Pay" (o "Lu-Ks Apple Pay").
- **No se puede compartir para Apple Pay**: un atajo normal no puede recibir el tipo "Transacción" (no está en "Recibir"), así que Cantidad/Comercio/Tarjeta solo existen dentro de la automatización de Wallet. Cada usuario tendría que armar el Texto con 4 variables. Para SMS sí se podría compartir (el mensaje llega como texto), pero Jose decidió esperar.
- **Decisión de Jose**: dejar Apple Pay y SMS automáticos como **"Pronto"** hasta tener la cuenta de Apple. En `auto.js`: si la persona no tiene llave (`ajG("atajo")`), Ajustes muestra la etiqueta "Pronto · Disponible en la próxima versión de Lu-Ks" y al tocar sale un aviso; quien ya tiene llave (Jose) sigue viendo y usando todo, y el buzón se sigue procesando. Las instrucciones de "Activar" ya usan los nombres de iOS 26. `LKAUTO.asegurar()` crea la llave (lo usa la prueba). `tauto.js`: 38 controles.
- **Cuando haya cuenta de Apple (compilación propia)**: hacer una acción nativa de Atajos (App Intent, como AI Money) "Lu-Ks: registrar pago" que reciba la transacción de Wallet o el texto del SMS y lo mande al buzón (o lo guarde directo). Así cada usuario solo hace Automatización → Wallet (o Mensaje) → acción de Lu-Ks, sin escribir nada. Luego quitar "Pronto".

## 46. Carrusel en fila y teclado (31 oct 2026, noche)
Jose (video): las tarjetas no se movían como en el video de referencia (en el mazo se cruzaban y se veían dos a la vez), el teclado se quedaba abierto (incluso sobre "¡Cuenta agregada!") y al buscar un banco que no está en la lista no había salida. Archivos: `cuentas.js`, `nuevacuenta.js`.
- **Carrusel**: ahora las tarjetas van en fila, lado a lado (paso = ancho + 18 px), la del centro a tamaño completo y las vecinas al 90 % asomándose en los bordes, con un giro leve (3°) y una sombra suave; el dedo arrastra la fila y al soltar se acomoda con resorte.
- **Teclado**: `Keyboard.dismiss()` al guardar, al elegir banco, al volver de la lista de bancos y al cerrar el formulario; las listas cierran el teclado al deslizar (`keyboardDismissMode="on-drag"`).
- **Banco que no está en la lista**: aparece arriba "Usar «Binance»" con un logo de iniciales (`tilePropio`, mismo cálculo de color que `bkTile` de la web); se guarda con ese nombre de banco.

## 47. Carrusel 3D como el video y portadas de tarjetas (1 nov 2026, madrugada)
Jose: "lo de las tarjetas sigue sin ser lo que pedí" y pidió poner portadas (fotos de sus tarjetas de Wallet: Binance, Apple Cash, Nu, BBVA, Rappi) a sus cuentas. Archivos: `cuentas.js`, `nuevacuenta.js`, `puente.js`, `package.json` y `package-lock.json` (nueva: `expo-image-manipulator`, viene en Expo Go), nuevo `pubsrc/port.js` (en `build_pub.py` después de `auto.js`), `pub/index.html`.
- **Carrusel** (estudiado cuadro por cuadro el video de referencia): tarjetas de pie; en reposo solo se asoma el borde de las vecinas (paso `P = W/2 + 0.42·ancho − 10`); al deslizar es un cilindro 3D: la que sale gira hasta 30–42° (`rotateY`) y la que entra llega girada al revés, con leve reducción y sombra. Entrada al abrir: la tarjeta llega volando desde abajo, inclinada, y las vecinas aparecen después (resorte). Al soltar se acomoda según lo arrastrado y la velocidad. **Error encontrado**: al soltar, el toque se contaba como "tocar la tarjeta anterior" y volvía atrás; ahora se ignoran toques 400 ms después de un deslizamiento. Debajo de la tarjeta "Agregar" sale "Nueva cuenta" grande como en el video. En las tarjetas de pie la red (VISA) va abajo a la izquierda y el tipo a la derecha.
- **Portadas**: cada usuario elige una foto para cada cuenta (Editar cuenta → "Portada de la tarjeta" → Elegir foto → recortar 8:5 → se reduce a 900 px JPEG 72 % con `expo-image-manipulator`). Se guarda en `users/{uid}/portadas/{id}` = {n, img} (`LKPORT.get/set/mover` en `port.js`; reglas ya permiten todo bajo `users/{uid}`). `__lkSaveAcc` guarda la portada solo si la cuenta se guardó bien (`ok = !ultimoAviso`), la mueve si cambia el nombre y la borra al eliminar la cuenta. `cuentaData` y `accform` mandan `portada`. `Plastico` con `portada` dibuja la foto (`Portada`): de pie se gira 90°; en el formulario y la animación de guardado va horizontal con el monto encima de un degradado. No se incluyen logos de bancos en la app: cada quien pone sus fotos.
- A Jose se le entregaron sus 5 portadas recortadas (`portada-*.jpg`) para guardar en Fotos.
- **Pruebas**: `tdiseno.js` 30 controles (portada: guardar, carrusel, formulario, Firebase, mover al renombrar, no guardar si falta el cupo, quitar). Todas en verde.

## 48. Portadas sin recortes y fondo del color de la portada (1 nov 2026)
Jose: "se ve cortado, no se ve natural los fondos para las tarjetas". Las tarjetas de pie tenían proporción 1,36 y la portada (8:5) se recortaba (logo de BBVA, últimos 4 y logo de Nu cortados); además la franja diagonal de color terminaba en un corte duro. Archivos: `cuentas.js`, `puente.js`, `pubsrc/port.js`, `pruebas/tdiseno.js`, `pruebas/tmov.js`.
- Las tarjetas del carrusel ahora tienen la proporción real de una tarjeta (1,586: alto = 46 % de la pantalla, máx. 430 px): la portada girada cabe completa.
- Fondo: degradado vertical suave del color de la tarjeta (sin la franja diagonal). Con portada, el color sale de la propia foto: `port.js` calcula el color promedio (lienzo 12×8, dando más peso a los colores vivos) y `cuentaData` lo manda en `portadaC`.
- Con portada ya no se pinta el monto encima (tapaba el número impreso).
- **Prueba de fecha**: `tmov.js` creaba movimientos de los últimos 20 días y el 1 de noviembre solo los de hoy entran en "Este mes"; ahora las fechas quedan dentro del mes actual. `tdiseno.js` 31 controles (color de la portada) con una imagen morada inventada (no se suben imágenes reales de tarjetas al repo).

## 49. Entrada de "Mis cuentas" como el video (1 nov 2026)
Estudiada a 20 cuadros por segundo: en el video la pantalla no sube; la tarjeta sale pequeña y acostada desde el Inicio, se endereza mientras crece con un rebote, el resto aparece suave encima del Inicio, el saldo cuenta y las tarjetas vecinas llegan al final. En `cuentas.js`: `Modal` transparente sin animación (`animationType="none"`), toda la pantalla con `opacity: fondo` (0→1 en 260 ms, así se ve el Inicio desvanecerse debajo); la tarjeta del centro va de escala 0,5, giro −72°, 16 % a la izquierda y 10 % arriba a su lugar con resorte (rigidez 170, amortiguación 16); las vecinas entran desde el lado 380 ms después; el saldo de la cuenta abierta cuenta desde $0 cada vez que se abre (`Monto` con `desde0` = 'cu' + número de apertura).

## 50. Animación al abrir más fluida (1 nov 2026)
Jose: "la animación de entrada no se ve fluida, se nota poco profesional". Causas: muchas piezas animadas a la vez (piezas volando, barrido diagonal de una vista gigante girada, botones con rebote, hoja subiendo 45 % de la pantalla) y el saldo que cuenta cambiando el texto en cada cuadro mientras la web todavía carga (hilo de JavaScript ocupado). Archivos: `bienvenida.js`, `inicio.js`.
- **`Intro`** nueva: empieza idéntica a la pantalla de carga (`expo-splash-screen`: ícono de 120 px centrado, fondo #F2F2F7 o negro según el iPhone, `useColorScheme`), el ícono se encoge a 0,88 (170 ms), se acerca a 2,6× y todo se desvanece (≈ 0,7 s). Solo `transform`/`opacity` con `useNativeDriver`.
- **Entrada del Inicio**: un solo `Animated.timing` de 850 ms con `Easing.out(Easing.exp)`; encabezado y saldo aparecen subiendo 10–14 px, botones de 0,86 a 1 escalonados 50 ms (sin rebote), la hoja sube 70 px.
- **`Monto`**: a lo más ~30 cambios por segundo y solo si cambia el texto; el saldo del Inicio cuenta en 800 ms.

## 51. Portadas que se borraban y destello al cambiar de tarjeta (1 nov 2026, noche)
Jose: "las portadas se eliminan y cuando intento pasar a otra tarjeta se superpone la anterior por milisegundos". Archivos: `App.js`, `cuentas.js`, `nuevacuenta.js`, `puente.js`, `pubsrc/port.js`, `pruebas/tdiseno.js`.
- **Portadas borradas**: si se abría "Editar cuenta" antes de que cargaran las portadas de Firebase, el formulario creía que no había portada y al guardar cualquier cambio mandaba `portada: ''` (borrar). Ahora el formulario solo manda `portada` si el usuario tocó "Elegir foto" o "Quitar" (`portadaCambio`); el puente nunca borra si las portadas no han cargado (`LKPORT.listo()`); si cambia el nombre sin tocar la portada, la mueve (`LKPORT.mover`). Además `port.js` avisa cada vez que cargan o cambian (`__lkPortadas` → mensaje `portadas`) y la app las guarda en el celular (`portadas.json`) para mostrarlas al instante en el carrusel y el formulario aunque Firebase aún no responda.
- **Destello al deslizar**: al pasar la mitad del gesto cambiaba `idx` y se volvían a dibujar todas las tarjetas (para cambiar `zIndex`); en el iPhone eso reconecta las animaciones nativas y por un cuadro la tarjeta anterior aparecía encima. Ahora las tarjetas se memorizan (`useMemo`) y no se redibujan durante el gesto (comprobado: 0 redibujos); el orden es fijo (no hace falta: con el giro hacia afuera no se tocan, medido a mitad del gesto: −17…213 px y 276…480 px).
- **Entrada cortada** al abrir desde otra tarjeta que no fuera la primera: el papel "tarjeta del centro" de la entrada cambiaba al fijar `idx` y la animación se detenía (`finished:false`). Ahora se usa la tarjeta con la que se abrió (`data.at`).
- **Pruebas**: `tdiseno.js` 34 controles (editar sin tocar la portada la conserva, renombrar la mueve, la app recibe las portadas). Todas en verde.

## 52. Revisión completa del diseño nuevo (1 nov 2026, noche)
Jose pidió una revisión detallada de todo. Se leyó línea por línea el código nativo nuevo y el puente, y se agregaron pruebas de casos borde. Fallas encontradas y corregidas:
1. **El formulario de cuenta se reiniciaba solo** (`App.js` creaba un objeto nuevo de datos en cada redibujo cuando la portada venía del caché del celular; `nuevacuenta.js` se reiniciaba con cada objeto nuevo): podía borrar lo escrito o cortar la animación "Agregando tu cuenta". Ahora se reinicia solo al abrirlo (`ses`, contador de aperturas) y la portada que llega tarde se pone sin tocar lo demás (`portadaGuardada`).
2. **Guardar sin respuesta dejaba la animación colgada al 85 %**: si la web no confirma en 12 s, vuelve al formulario con un mensaje.
3. **Privacidad al cerrar sesión**: las portadas guardadas en el celular quedaban y otra persona con una cuenta del mismo nombre las vería. Ahora al cerrar sesión se borran (`portadas.json`, carrusel, formulario, ventanas); en la web `port.js` limpia todo al cambiar de usuario y `listo()` vuelve a falso.
4. **Errores silenciosos**: si "Cuentas" o "Agregar cuenta" fallaban en la web, no pasaba nada al tocar; ahora sale un aviso.
5. **Apple Pay/SMS duplicados con dos dispositivos abiertos**: ambos procesaban el mismo dato del buzón. Ahora cada dato se "reclama" con una transacción de Firestore (leer y borrar); solo el que lo logra lo guarda (`reclamar` en `auto.js`). El falso Firebase de pruebas ahora tiene `runTransaction` (no puede reproducir el duplicado porque cada pestaña sobrescribe la hoja, pero `tdoble.js` confirma que se guarda una vez).
6. **"Mis cuentas" se cerraba de golpe**: ahora se desvanece (180 ms) igual que abre. Se quitó un estilo sin uso.
- **Pruebas nuevas**: `tbordes.js` (13: sin cuentas, montos ocultos, portadas que no pasan a otro usuario, inglés), `tdoble.js` (1). Todas: `tbordes` 13, `tdiseno` 34, `tauto` 38, `tventanas` 16, `tinicio` 49, `tmov` 41, `trep` 40, `tmas` 37, `tcopia` 13, `trecordatorios` 14, `tparpadeo` OK (claro y oscuro).

## 53. Portadas que no se pierden nunca (2 nov 2026)
Jose: "se siguen borrando las portadas de las tarjetas, corrige definitivamente ese error". Archivos: `App.js`, `puente.js`, `portadas.js` (nuevo, en lu-ks-app), `pubsrc/port.js` (va dentro de `index.html`), `pruebas/mockfb.js`, `pruebas/tdiseno.js`, `pruebas/tportadas.js` (nueva). Versión de la app 1.6.1.
Causas encontradas (había varias formas de perderlas, todas cerradas):
1. **Firebase "olvidaba" escrituras sin avisar**: `poner` hacía `commit()` sin revisar si funcionó. Si la subida fallaba (conexión del iPhone, app cerrada antes de terminar), la portada solo existía en el celular y al reabrir desaparecía. Ahora cada escritura queda en una **cola** (`localStorage`, `lkPortCola:{uid}`) hasta que Firebase confirma; se reintenta cada 20 s, al volver la conexión y al abrir. Si falla, sale un aviso ("La portada aún no se guardó en la nube; se reintentará sola").
2. **La primera respuesta del caché venía vacía**: al abrir, Firestore puede responder primero desde el caché del celular sin documentos; eso se tomaba como "no hay portadas" y la app **reemplazaba** su copia por una lista vacía. Ahora `port.js` ignora esa respuesta (escucha con `includeMetadataChanges` y espera la de Firebase) y la web nunca manda una lista vacía antes de saber.
3. **La copia del celular se reemplazaba entera**: ahora se **mezcla** (`mezclarPortadas` en `portadas.js`): una portada solo sale de la copia si la persona la quitó, borró o renombró la cuenta.
4. **Quitar = borrar el documento**: no se podía distinguir "la quitó" de "se perdió". Ahora quitar deja una marca `{n, img:'', q:1}`.
5. **Reparación automática**: si Firebase ya respondió y le falta una portada que el celular sí tiene (y no está marcada como quitada y la cuenta existe), la app la vuelve a subir (`__lkPortRep` → `LKPORT.reparar`), una vez por apertura.
6. **Renombrar solo con mayúsculas** (ej. "Nequi" → "NEQUI") usaba el mismo documento: se guardaba y luego se borraba. Ahora solo se actualiza el nombre.
- Se sigue borrando la copia del celular al **cerrar sesión** (privacidad, sección 52).
- La web solo manda las portadas a la app cuando algo cambió (firma de nombres/tamaños), no en cada actualización.
- **Pruebas**: `tportadas.js` 22 controles (quitar deja marca, caché vacío al abrir, falla al guardar → aviso, cola, sigue tras reabrir, se sube al volver la conexión, reparar, no revive quitadas ni cuentas inexistentes, renombres, mezcla de la copia). El Firebase falso ahora tiene `doc().set/delete`, `mockFalla` (falla escrituras) y `mockCacheVacio` (primera respuesta vacía del caché). Todas en verde: `tportadas` 22, `tdiseno` 34, `tbordes` 13, `tdoble` 1, `tauto` 38, `tventanas` 16, `tinicio` 49, `tmov` 41, `trep` 40, `tmas` 37, `tcopia` 13, `trecordatorios` 14.
- Las portadas que se perdieron antes de esta versión no se pueden recuperar (no quedaron en ninguna parte): Jose debe ponerlas una vez más.

## 54. El diseño nuevo de la app, también en la web (2 nov 2026)
Jose: "¿podrías implementar todos los cambios realizados en la app en la app de la web?". Archivos: `pubsrc/nuevo.js` y `pubsrc/nuevo.css` (nuevos; `build_pub.py` los agrega después de `ios.css` y de `li.js`), `pruebas/tnuevo.js` (nueva), `pruebas/foto-prueba.png` (imagen inventada para la prueba de portada), `pruebas/tdoble.js` y `pruebas/tcopia.js` (usan `window.LKNV_OFF=1` para seguir probando el diseño clásico). Solo se sube `index.html` (+ `codigo-fuente.zip` y esta guía); la app del celular no cambia.
- **Solo en el navegador**: `nuevo.js` no hace nada dentro de la app (`window.ReactNativeWebView` o `LuKsApp` en el agente), porque ahí esas pantallas las dibuja la app. Activo pone la clase `nv` en `<html>`.
- **Inicio** (igual al nativo): después de cada `render()` el Inicio se reacomoda en `.nv-top` (encabezado azul oscuro con degradado del color de acento: saludo, frase, "Tu semana", Finanzas/Ahorros, período y el saldo grande que cuenta) + botones rápidos **Anotar / Transferir / Cuentas / Más** + `.nv-hoja` (hoja clara redondeada que sube) con la tarjeta **Gastos** (barra de colores por categoría que crece y leyenda; tocar una categoría abre Movimientos filtrados). Animación de entrada solo la primera vez. Tocar una cuenta del Inicio abre el carrusel.
- **Barra de abajo flotante**: píldora con resaltado que se desliza a la pestaña activa (`.nv-on`) y botón + aparte a la derecha.
- **Mis cuentas** (`LKNV.abrirCuentas(i)`): carrusel 3D tipo cilindro igual al de la app (mismos números: alto 46 % de la pantalla, proporción 1,586, giro 30°/42°, entrada con resorte desde −72° y escala 0,5, vecinas a los 380 ms), arrastre con el dedo o ratón, flechas del teclado, portadas giradas, fondo del color de la tarjeta, saldo que cuenta, Entró/Salió del mes, Información, "Ver en Movimientos", "Editar cuenta" y movimientos (tocar uno abre la ventana de la web). La tarjeta se redibuja si cambia la cuenta o su portada.
- **Formulario de cuenta** (reemplaza `openAcc`): tarjeta que se llena en vivo y se voltea al escribir corte/pago, Débito/Crédito/Efectivo, **portada** (elegir foto → recortar 8:5 moviendo y con zoom → JPEG 900 px al 72 %), banco con buscador y "Usar «…»" para bancos que no están, apodo, red, colores, cupo con puntos, corte, pago, recordatorio, saldo de hoy, cuenta principal, opciones avanzadas, eliminar con dos toques. Guarda con la misma lógica de la web (`saveAcc2` + portadas con `LKPORT`, igual que `__lkSaveAcc` del puente) y muestra "Agregando tu cuenta" (Revisar → Guardar → Lista) y "¡Cuenta agregada!" con "Ver cuenta".
- **Bienvenida** (sin sesión, sobre la página de presentación): las dos piezas se juntan, aparece el ícono, el degradado barre en diagonal y suben el mensaje y los botones (empieza cuando se va la pantalla de carga). Botones: Crear cuenta, Ya tengo cuenta, idioma y "Conoce Lu-Ks ↓" (muestra la página de presentación completa).
- **Pantalla de carga**: al irse, el ícono se acerca (×2,6) y se desvanece, como en la app.
- **Pruebas**: `tnuevo.js` 26 controles (bienvenida, Inicio, barra, agregar tarjeta con banco propio, validaciones, vista previa, volteo, animación, portada con recorte, carrusel, deslizar, botones rápidos, eliminar, nada cambia dentro de la app). Todas en verde: `tnuevo` 26, `tportadas` 22, `tdiseno` 34, `tbordes` 13, `tdoble` 1, `tauto` 38, `tventanas` 16, `tinicio` 49, `tmov` 41, `trep` 40, `tmas` 37, `tcopia` 13, `trecordatorios` 14.

## 55. La versión nueva de la web se ve al instante (2 nov 2026)
Jose subió el `index.html` nuevo y en Safari seguía viendo el diseño viejo: GitHub Pages manda la página con "guárdala 10 minutos" (`max-age=600`) y el service worker la pedía usando esa memoria del navegador. Archivo: `pubsrc/sw.js` (caché `luks-v3`): la página (navegación, `/` o `.html`) se pide siempre con `cache:"no-store"`; lo demás igual (red primero y, sin internet, lo guardado). Prueba `pruebas/tsw.js` (servidor con `max-age=600`, la versión nueva se ve al abrir). Mientras el sw.js nuevo no esté instalado, el truco es abrir `jimenez19pro.github.io/lu-ks/?v=2`.

## 56. Página de presentación, video e imagen para compartir con el diseño nuevo (2 nov 2026)
Jose: "actualiza la página e igualmente el video". Archivos: `pubsrc/land.js` (capturas nuevas dentro del archivo), `lu-ks-video.mp4`, `lu-ks-poster.jpg`, `og-luks.jpg` (raíz del repositorio), `promo/gen.py`, `pruebas/tshotnuevo.js` (nueva), `work/landshots/` y `work/og/`.
- **Capturas** con `pruebas/tshotnuevo.js <es|en>` (modo oscuro, datos de ejemplo de `tshotland.js`, pasa por la bienvenida nueva): `l2-home` (Inicio nuevo), `l2-cuentas` (carrusel en la tarjeta Nu), `l2-form` (Nueva cuenta con Visa Oro de BBVA), `l2-pres`, `l2-mov`, `l2-rep`, `l2-aho`. Variables: `VH` (alto: 780 página, 844 video), `OUT` (carpeta), `PRE` (prefijo).
- **Página de presentación**: las 3 capturas (ES e inglés) ahora son Inicio, Mis cuentas y Nueva cuenta (480×960 JPEG 82 %).
- **Video** (25 s, 1080×1920): mismas escenas, tiempos y voz/música del video anterior (el audio se copió del `lu-ks-video.mp4` publicado), con las capturas nuevas; en la primera escena, cuando sale "Todas tus cuentas juntas" (5,15 s), el celular pasa al carrusel. Proceso: copiar `l2-*.png` (VH=844) a `promo/`, `python3 gen.py`, `node render2.js video-luks.html 25 frl`, `ffmpeg -framerate 30 -i frl/%04d.jpg -i <video anterior> -map 0:v -map 1:a -c:v libx264 -crf 22 -pix_fmt yuv420p -c:a copy -t 25 -movflags +faststart lu-ks-video.mp4`. Portada del video: segundo 5 (540×960).
- **Imagen para compartir** (`og-luks.jpg`, WhatsApp): Inicio nuevo + carrusel.

## 57. Portadas: solo el botón "Quitar" puede quitarlas (2 nov 2026, tarde)
Jose: las portadas se volvieron a borrar, en la app y en la web, sin ningún aviso (o sea, sí se habían guardado en Firebase; algo las quitó). Pidió una solución definitiva. Archivos: `pubsrc/port.js`, `pubsrc/nuevo.js` (van en `index.html`), `App.js`, `puente.js`, `portadas.js` (app 1.6.2), pruebas `tportadas.js` (30), `tdiseno.js`, `tnuevo.js`.
Se quitaron todas las formas automáticas de quitar una portada:
1. `LKPORT.set(nombre, '')` ya no hace nada. Quitar solo existe como `LKPORT.quitar(nombre)`, que se llama únicamente cuando la persona toca "Quitar" en el formulario (`portada: ''`).
2. **Cambiar el nombre** de una cuenta copia la foto al nombre nuevo y **deja** la del nombre viejo (antes la marcaba como quitada).
3. **Eliminar una cuenta** ya no borra su portada.
4. La marca de quitada ahora dice `por: "quitar"`, guarda la foto anterior (`ant`) y desde dónde se quitó (`de`). Las marcas viejas sin `por: "quitar"` (las que pudieron hacer los cambios de nombre o borrados) **no cuentan**: la copia del celular puede volver a subir esa portada. La cola local también descarta marcas viejas.
5. **Cerrar sesión ya no borra la copia del celular**: `portadas.json` ahora es `{v:2, ultimo, por:{uid:{…}}}` (separada por usuario, así otra persona no ve las de Jose); al cerrar sesión solo se deja de mostrar. El puente manda el `uid` en el mensaje `portadas` (`u`). El archivo viejo (sin usuario) se adopta para el primer usuario que entre.
Las portadas que se perdieron hoy no se pueden recuperar (no quedaron en Firebase ni en el celular): hay que ponerlas una vez más. Desde esta versión, si vuelve a pasar algo, la marca guardada dirá exactamente desde dónde se quitó.
Todas las pruebas en verde: `tportadas` 30, `tdiseno` 34, `tnuevo` 26, `tbordes` 13, `tauto` 38, `tventanas` 16, `tinicio` 49, `tmov` 41, `trep` 40, `tmas` 37, `tcopia` 13, `trecordatorios` 14, `tdoble` 1.
