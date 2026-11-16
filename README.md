# Calificación CAMEL de bancos

Página de una sola hoja (`index.html`) que muestra el scoring CAMEL de los bancos: puntaje global, posición, nota, variación y componentes C-A-M-E-L. Tiene una lista para elegir el mes y otra para ver un solo banco.

Los datos no están fijos: cada vez que se abre, la página los lee del Google Sheet publicado. Si el Sheet no responde, muestra la copia de septiembre de 2026 guardada dentro del archivo.

## Publicarla gratis con GitHub Pages

1. Entrá a [github.com](https://github.com) y creá un repositorio nuevo. Tiene que ser **público** (en la cuenta gratis, Pages solo funciona con repositorios públicos).
2. En el repositorio: **Add file → Upload files**, arrastrá `index.html` y `README.md`, y confirmá con **Commit changes**.
3. Andá a **Settings → Pages**. En "Build and deployment" elegí **Deploy from a branch**, rama **main**, carpeta **/ (root)**, y **Save**.
4. Esperá uno o dos minutos. La dirección queda así: `https://TU-USUARIO.github.io/NOMBRE-DEL-REPOSITORIO/`

Para actualizar la página más adelante, subí de nuevo `index.html` con el mismo nombre; GitHub reemplaza el anterior.

## De dónde salen los datos

Los links están al principio del bloque `<script>` de `index.html`, en `CONFIG.FUENTES`:

| Fuente | Link |
|---|---|
| Pestaña Resumen | `…/pub?gid=611378809&single=true&output=csv` |
| Documento completo | `…/pub?output=csv` |

Cada link puede traer:

- una hoja tipo **Resumen**: un solo mes, el que esté en "Período a analizar";
- una **tabla con todos los meses**: una fila por entidad y período, con columnas `Período`, `Entidad` y `Puntaje global` (y, si están, `C`, `A`, `M`, `E`, `L`, `Nota CAMEL`, `Categoría`, `Patrimonio efectivo`, `Participación`).

La página junta los meses de todos los links y los muestra en la lista de mes.

### Agregar otra pestaña

Publicá la pestaña como CSV en Google Sheets (**Archivo → Compartir → Publicar en la web**) y agregá una línea en `CONFIG.FUENTES`:

```js
{ nombre: "Histórico", url: "https://docs.google.com/spreadsheets/d/e/…/pub?gid=NÚMERO&single=true&output=csv" }
```

## Si faltan meses

Mirá el pie de la página: hay una línea por link que dice qué encontró.

- **"hoja Resumen de …"**: ese link trae un solo mes.
- **"trae la misma hoja Resumen …, no agrega meses"**: los dos links devuelven la misma pestaña. Un link `pub?output=csv` sin `gid` devuelve solo la primera pestaña del documento.
- **"su primera pestaña no tiene columnas Entidad y Puntaje global"**: esa pestaña no tiene puntajes calculados; el pie muestra con qué columnas empieza.
- **"no respondió"**: el link no está publicado o no hay conexión.

Un mes marcado como "solo puntaje global" es el mes de comparación del Resumen: su puntaje se reconstruye como puntaje del mes analizado menos la variación, y no trae nota ni componentes.

## Cosas a saber

- Google tarda unos minutos en republicar después de un cambio en el Sheet. El botón **Volver a leer** recarga los datos sin recargar la página.
- Menor puntaje = mejor. Escala: 1 Sólido, 2 Satisfactorio, 3 Aceptable, 4 Débil, 5 Crítico. Variación negativa = mejora.
- La página usa la tipografía Figtree de Google Fonts; sin conexión usa la del sistema.
