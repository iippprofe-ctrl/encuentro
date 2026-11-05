# Configuración de Google Sheets para el Conversatorio

## Paso 1: Crear la hoja de cálculo

1. Ve a [Google Sheets](https://sheets.google.com) e inicia sesión con tu cuenta de Google.
2. Crea una nueva hoja de cálculo.
3. Nómbrala, por ejemplo: **Preguntas Conversatorio 2026**
4. En la primera fila, escribe los encabezados:
   - **A1**: Fecha
   - **B1**: Nombre
   - **C1**: Pregunta

---

## Paso 2: Crear el Google Apps Script

1. En tu hoja de cálculo, ve al menú **Extensiones → Apps Script**.
2. Borra todo el código que aparece por defecto.
3. Pega exactamente el siguiente código:

```javascript
function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var fecha    = e.parameter.fecha    || new Date().toLocaleString("es-BO");
  var nombre   = e.parameter.nombre   || "";
  var pregunta = e.parameter.pregunta || "";
  sheet.appendRow([fecha, nombre, pregunta]);
  return ContentService
    .createTextOutput(JSON.stringify({ status: "ok" }))
    .setMimeType(ContentService.MimeType.JSON);
}
```

4. Haz clic en el ícono de **guardar** (💾) o presiona `Ctrl+S`.
5. Dale un nombre al proyecto, por ejemplo: **ConversatorioScript**.

---

## Paso 3: Publicar como Web App

1. Haz clic en **Implementar → Nueva implementación**.
2. En "Tipo", selecciona **Aplicación web**.
3. Configura:
   - **Descripción**: Conversatorio 2026
   - **Ejecutar como**: Yo (tu cuenta de Google)
   - **Quién tiene acceso**: **Cualquier usuario** ← IMPORTANTE
4. Haz clic en **Implementar**.
5. Google te pedirá que autorices los permisos → Acepta todo.
6. Copia la **URL de la aplicación web** que aparece al final. Tiene este formato:
   ```
   https://script.google.com/macros/s/XXXXXXXXXX/exec
   ```

---

## Paso 4: Configurar la página web

1. Abre el archivo `index.html` con un editor de texto (Notepad, VS Code, etc.).
2. Busca esta línea:
   ```javascript
   const GOOGLE_SCRIPT_URL = 'PEGA_AQUI_TU_WEB_APP_URL';
   ```
3. Reemplaza `PEGA_AQUI_TU_WEB_APP_URL` con la URL copiada en el paso anterior. Ejemplo:
   ```javascript
   const GOOGLE_SCRIPT_URL = 'https://script.google.com/macros/s/AKfycbxXXXXXX/exec';
   ```
4. Guarda el archivo.

---

## Paso 5: Probar

1. Abre `index.html` en tu navegador.
2. Llena el formulario con un nombre y una pregunta de prueba.
3. Haz clic en **Enviar pregunta**.
4. Verifica en tu hoja de Google Sheets que los datos aparecieron en una nueva fila.

---

## Notas importantes

- Si haces cambios al Apps Script, debes crear una **nueva implementación** (no actualizar la existente) para que los cambios surtan efecto.
- El modo `no-cors` del formulario significa que no veremos errores del servidor, pero los datos se enviarán correctamente si la URL es correcta.
- Si la hoja no recibe datos, verifica que el acceso sea **"Cualquier usuario"** al implementar.

---

**Soporte**: Ministerio de Educación · Estado Plurinacional de Bolivia · 2026
