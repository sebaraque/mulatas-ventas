# Las Mulatas - Sistema de Ventas

PWA (app instalable) para registrar el cierre de ventas diario y ver reportes desde el celular.

## Roles

- **Vendedora (PIN 1234)**: ingresa el total de ventas del día
- **Dueños (PIN 5678)**: dashboard con ventas diarias, semanales, mensuales, gráfico de 14 días y comparativa

> ⚠️ **Cambia los PINs antes de usar la app en producción.** Edita `index.html` líneas:
> ```js
> const PIN_VENDEDORA = "1234";
> const PIN_DUENOS = "5678";
> ```

---

## Pasos de instalación

### 1. Crear proyecto en Firebase (5 minutos)

Firebase es la base de datos en la nube. Es gratis para este uso (muy por debajo del límite gratuito de 50,000 lecturas/día).

1. Andá a https://console.firebase.google.com/
2. Click en **"Agregar proyecto"** → ponle nombre `mulatas-ventas` (o como quieras)
3. Desactivá Google Analytics (no lo necesitás)
4. Una vez creado, click en el icono **`</>`** (Web) para registrar una app web
5. Ponele apodo `mulatas-web`, **NO** marques Firebase Hosting, click en **Registrar app**
6. Te va a mostrar un bloque `firebaseConfig = { ... }` — **copiá ese bloque entero**

### 2. Activar Firestore

1. En el menú lateral de Firebase: **Firestore Database** → **Crear base de datos**
2. Modo: **modo de producción** → siguiente
3. Ubicación: **southamerica-east1** (São Paulo, lo más cercano a Ecuador)
4. Click en **Crear**

### 3. Configurar las reglas de seguridad

En Firestore → pestaña **Reglas** → reemplazá todo por esto y publicá:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /ventas/{venta} {
      allow read, write: if true;
    }
  }
}
```

> Nota: estas reglas son abiertas porque usamos PIN para proteger la app. Si querés más seguridad después, podemos agregar Firebase Authentication.

### 4. Pegar la configuración en el código

Abrí `index.html` y buscá esta sección (cerca de la línea 230):

```js
const firebaseConfig = {
  apiKey: "TU_API_KEY",
  authDomain: "TU_PROJECT.firebaseapp.com",
  ...
};
```

Reemplazá ese bloque entero con el que copiaste de Firebase en el paso 1.

### 5. Subir a GitHub

1. En GitHub, creá un repo nuevo: **mulatas-ventas** (público)
2. En tu compu, abrí terminal en la carpeta del proyecto:

```bash
git init
git add .
git commit -m "Primera versión"
git branch -M main
git remote add origin https://github.com/sebaraque/mulatas-ventas.git
git push -u origin main
```

### 6. Activar GitHub Pages

1. En el repo en GitHub: **Settings** → **Pages**
2. Source: **Deploy from a branch**
3. Branch: **main** → Folder: **/ (root)** → **Save**
4. Esperá ~1 minuto. Te aparecerá la URL: `https://sebaraque.github.io/mulatas-ventas/`

### 7. Instalar en el celular

Abrí la URL en el celular de cada persona (vendedora, vos, esposa):

- **Android (Chrome)**: menú ⋮ → "Instalar aplicación" o "Agregar a pantalla de inicio"
- **iPhone (Safari)**: botón compartir → "Agregar a pantalla de inicio"

Listo, ahora aparece como app normal con el icono de Las Mulatas.

---

## Cómo se usa

**Vendedora:**
1. Abre la app, pone PIN 1234
2. Escribe el total de ventas del día en el campo grande
3. Click en "Enviar cierre"
4. Si se equivoca, puede volver a entrar y actualizar el monto

**Dueños:**
1. Abre la app, pone PIN 5678
2. Ve directo el dashboard con todos los reportes en tiempo real

---

## Estructura

```
mulatas-ventas/
├── index.html          ← App completa (HTML + CSS + JS)
├── manifest.json       ← Configuración PWA
├── service-worker.js   ← Cache offline
├── icon-192.png        ← Icono pequeño
├── icon-512.png        ← Icono grande
└── README.md           ← Este archivo
```

---

## Costos

**Todo gratis** mientras Las Mulatas no haga más de 50,000 cierres por día (que sería imposible 😄). El plan gratuito de Firebase aguanta sin problema un restaurante.
