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

En `pruebas/`: `mockfb.js` (Firebase falso: auth, firestore con batch, onSnapshot de colecciones y documentos, messaging) y scripts de Playwright (`ttienda.js`, `tt3.js` = 8 mejoras + empleados, `tlic.js` = licencias, `tadm.js` = panel admin, `tpk.js` = precios por cantidad, `tfx.js` = factura con IA (simula la respuesta de Gemini), `tld.js` = lectora y escanear inventario (simula la lectora escribiendo rápido), `tcam.js` = escáner de cámara con cámara falsa y lector simulado, `tgar.js`, `tdev.js`, `tcol.js`, etc.).
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
- **No le interesaron "por el momento"** (no insistir, quizá más adelante): control de tarjeta de crédito, detector de movimientos repetidos, cierre del mes, exportar a Excel, accesos rápidos, aviso de actualización exacto, foto del recibo en movimientos, escanear factura electrónica, alcancía de redondeo, sobres de efectivo, calculadora de ahorro.
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
