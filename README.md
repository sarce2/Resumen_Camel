# Calificación CAMEL de bancos

Página de una sola hoja (`index.html`) que muestra el scoring CAMEL de los bancos: puntaje global, posición, nota, variación, componentes C-A-M-E-L, patrimonio y participación.

- **Lista de mes:** todos los meses cargados (hoy, enero a septiembre de 2026).
- **Lista de banco:** muestra un solo banco con su posición, su puntaje y su evolución mes a mes.
- **Comparar contra:** por defecto el mes anterior; se puede elegir cualquier otro.

## Publicarla gratis con GitHub Pages

1. Entrá a [github.com](https://github.com) y creá un repositorio nuevo. Tiene que ser **público** (en la cuenta gratis, Pages solo funciona con repositorios públicos).
2. En el repositorio: **Add file → Upload files**, arrastrá `index.html` y `README.md`, y confirmá con **Commit changes**.
3. Andá a **Settings → Pages**. En "Build and deployment" elegí **Deploy from a branch**, rama **main**, carpeta **/ (root)**, y **Save**.
4. Esperá uno o dos minutos. La dirección queda así: `https://TU-USUARIO.github.io/NOMBRE-DEL-REPOSITORIO/`

Para actualizar la página más adelante, subí de nuevo `index.html` con el mismo nombre; GitHub reemplaza el anterior.

## De dónde salen los datos

La página combina dos cosas:

| Origen | Qué aporta |
|---|---|
| **Copia guardada dentro de `index.html`** | Los 9 meses de la pestaña **Puntajes** y el patrimonio de la pestaña **Base**, sacados del Excel el 16/11/2026. Con esto la lista de mes siempre tiene todos los meses, aunque Google no responda. |
| **Google Sheet publicado, en vivo** | Cada vez que se abre la página lee el link publicado. La pestaña **Resumen** manda sobre su mes: si en el Sheet aparece un mes nuevo, la página lo suma sola. |

El link publicado está al principio del bloque `<script>`, en `CONFIG.DOCUMENTO_URL`:

```
https://docs.google.com/spreadsheets/d/e/2PACX-1vTglShja49GBgQX7qzsKUqE6sgwxhMMVaTyrw7i7WCfmCxtbFBzDdTwo2mqyCSKFyTRenh2tbqXKn5Z/pub?output=csv
```

Ese link, tal cual, devuelve solo la **primera** pestaña del documento (Resumen). Las otras pestañas se piden con su número (`gid`).

## Que todo el historial se lea en vivo (recomendado)

Sin este paso, los meses anteriores salen de la copia guardada: son correctos al 16/11/2026, pero no se enteran si después cambiás la Base o los Parámetros.

1. Abrí el Google Sheet y hacé clic en la pestaña **Puntajes**.
2. Mirá la dirección del navegador: termina en `#gid=` seguido de un número. Copiá ese número.
3. En `index.html`, buscá `GID_PUNTAJES: ""` y escribí el número entre las comillas. Por ejemplo: `GID_PUNTAJES: "123456789"`.
4. Opcional: hacé lo mismo con la pestaña **Base** en `GID_BASE`, para que el patrimonio de los meses nuevos también llegue en vivo.
5. Subí de nuevo `index.html` a GitHub.

El documento tiene que estar publicado completo: **Archivo → Compartir → Publicar en la web → Todo el documento**, con **Volver a publicar automáticamente cuando se realicen cambios** marcado.

La página también intenta encontrar esas pestañas sola. Si lo logra, el pie dice "Pestaña Puntajes (encontrada sola…)" y no hace falta tocar nada.

## Cómo leer el pie de la página

Hay una línea por cada cosa que la página leyó:

- **"Pestaña Resumen: mes publicado en la hoja, …"**: se leyó el Resumen en vivo.
- **"Documento completo: devuelve lo mismo que «Pestaña Resumen»"**: es lo normal; el link sin `gid` trae la primera pestaña.
- **"Pestaña Puntajes: 9 meses (…) en vivo"**: el historial completo viene del Sheet.
- **"Historial: 9 meses (…) de la copia guardada el 16/11/2026"**: el historial sale de la copia; falta `GID_PUNTAJES`.
- **"Ojo: el Resumen de … ya no coincide con la copia guardada"**: cambiaron datos o parámetros en el Sheet después de la copia. Ese mes usa el Resumen; para los demás hace falta `GID_PUNTAJES` o una copia nueva.
- **"El Sheet tiene N meses cargados y acá hay M completos"**: hay meses nuevos que todavía no llegan a la página.
- **"no respondió"**: el link no está publicado o no hay conexión.

## Cómo se calcula cada dato

- **Puntaje global y componentes:** tal cual están en la pestaña Puntajes (filas C, A, M, E, L y G).
- **Nota CAMEL y categoría:** con la escala de la hoja Parámetros: desde 1.0 Sólido, desde 1.5 Satisfactorio, desde 2.5 Aceptable, desde 3.5 Débil, desde 4.5 Crítico.
- **Posición:** orden por puntaje global dentro del mes (1 = mejor).
- **Variación:** puntaje del mes menos puntaje del mes de comparación. Negativa = mejora.
- **Patrimonio efectivo:** ratio 1.04 de la Base. **Participación:** patrimonio de la entidad sobre el del sistema.

Septiembre de 2026, calculado así, coincide con la pestaña Resumen en las 17 filas y en todas las columnas.

Si cambiás la escala en Parámetros, hay que cambiarla también en `ESCALA`, al principio del `<script>`.

## Cosas a saber

- Google tarda unos minutos en republicar después de un cambio en el Sheet. El botón **Volver a leer** recarga los datos sin recargar la página.
- La página usa la tipografía Figtree de Google Fonts; sin conexión usa la del sistema.
