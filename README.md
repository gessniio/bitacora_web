# bitacora_web
bitacora web + api de google sheet

Descripción del proyecto y tabla de características
Los 13 campos exactos de tu hoja con los valores permitidos (Alta/Media/Baja, etc.)
Guía paso a paso de Google Cloud: API Key, OAuth Client ID, pantalla de consentimiento
Cómo correrlo local con Python o desplegarlo en GitHub Pages
Las 4 constantes que hay que editar (CLIENT_ID, API_KEY, SPREADSHEET_ID, ROWS_PER_PAGE)
Diagrama de la estructura del archivo HTML y el flujo de datos
Tabla de consideraciones de seguridad (importante — tu API_KEY y SPREADSHEET_ID están visibles en el código)
Mejoras futuras y referencias oficiales de Google

# 📋 Bitácora de Actividades — UNRC Tampico

Aplicación web de una sola página (**sin backend**) que conecta directamente con **Google Sheets** para visualizar, filtrar y registrar actividades de ingeniería. Todo corre en el navegador; no requiere servidor ni base de datos.

---

## ¿Qué hace?

| Función | Descripción |
|---|---|
| 🔐 Autenticación OAuth 2.0 | Inicia sesión con tu cuenta Google directamente desde el navegador |
| 📊 Lectura de Sheets | Lee la hoja de cálculo y detecta automáticamente las columnas por nombre |
| 🔍 Filtros combinados | Filtra por prioridad, estado, dependencia y búsqueda de texto libre |
| ➕ Alta de registros | Formulario completo que agrega una nueva fila al final de la hoja |
| 📄 Paginación | Muestra 30 registros por página con navegación |
| ↕️ Ordenamiento | Clic en cualquier columna para ordenar ascendente / descendente |
| 📈 Estadísticas | Contadores en tiempo real de total, pendientes, en proceso y completados |

---

## Requisitos previos

Antes de usar o desplegar el proyecto necesitas tener configurado lo siguiente en **Google Cloud Console**:

1. **Proyecto activo** en [console.cloud.google.com](https://console.cloud.google.com)
2. **Google Sheets API** habilitada
3. **Clave de API** (para lectura pública) → `API_KEY`
4. **OAuth 2.0 Client ID** (tipo "Aplicación Web") → `CLIENT_ID`
   - Agrega el origen de tu dominio en *Orígenes autorizados de JavaScript*
5. Una **Google Sheet** con fila de encabezado que incluya estas columnas (el orden no importa, el sistema las detecta por nombre):

```
Elaboro | Fecha de solicitud | Numero Oficio | Nombre de actividad |
Descripción de la tarea | Dependencia | Prioridad | Estado |
Fecha de inicio | Fecha de finalización | Duración (horas) | Comentarios | Enlace
```

---

## Configuración rápida

Abre el archivo `index.html` y localiza el bloque de configuración al inicio del script:

```javascript
const CLIENT_ID      = 'TU_CLIENT_ID.apps.googleusercontent.com';
const API_KEY        = 'TU_API_KEY';
const SPREADSHEET_ID = 'ID_DE_TU_HOJA'; // está en la URL de Sheets
```

Reemplaza esos tres valores y listo.

---

## Cómo usarlo

```
1. Abre index.html en un navegador (o súbelo a GitHub Pages / cualquier hosting estático)
2. Clic en "Autorizar con Google"
3. Acepta los permisos de Google Sheets
4. Los datos de tu hoja cargan automáticamente
5. Usa los filtros, busca, ordena columnas
6. "Nueva actividad" → llena el formulario → Guardar
   └─ La nueva fila aparece al final de tu Google Sheet en tiempo real
```

---

## Estructura del proyecto

```
📁 bitacora/
 ├── index.html   ← Todo el proyecto (HTML + CSS + JS en un solo archivo)
 └── README.md
```

No hay dependencias npm, no hay build, no hay servidor. Es HTML puro que se sirve desde cualquier lugar.

---

## Cosas importantes a saber

- **Sin backend propio.** Toda la lógica corre en el navegador usando la Google Sheets API directamente. Esto significa que las credenciales (`CLIENT_ID`, `API_KEY`) quedan visibles en el código fuente — es el comportamiento normal para apps de tipo "cliente OAuth público".
- **Permisos OAuth.** El scope usado es `spreadsheets` (lectura y escritura). El usuario que se autentica necesita tener acceso a la hoja.
- **Detección automática de columnas.** El sistema mapea los encabezados de tu hoja a los campos del formulario sin importar en qué columna estén. Si una columna no existe en la hoja, ese campo se ignora al guardar.
- **`valueInputOption: 'USER_ENTERED'`** — Las fechas y números se guardan respetando el formato de la hoja, igual que si los escribieras a mano.
- **El token de sesión no persiste.** Al cerrar o recargar la página, el usuario debe volver a autorizar (comportamiento estándar de OAuth en apps de cliente puro).
- **La hoja debe tener al menos una fila de encabezados.** Si la hoja está completamente vacía, la app mostrará un aviso.

---

## Despliegue sugerido

Al ser un archivo estático, puedes desplegarlo en:

- **GitHub Pages** → gratis, solo activa Pages en el repositorio
- **Netlify / Vercel** → arrastra y suelta el archivo
- **Cualquier servidor web** → sube el `index.html` y ya

> ⚠️ Recuerda agregar el dominio donde lo despliegues a los **Orígenes autorizados** en tu OAuth Client ID de Google Cloud, de lo contrario la autenticación fallará.

---

## Tecnologías

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)
![Google Sheets API](https://img.shields.io/badge/Google_Sheets_API-v4-34A853?style=flat&logo=google-sheets&logoColor=white)
![OAuth 2.0](https://img.shields.io/badge/OAuth_2.0-4285F4?style=flat&logo=google&logoColor=white)
