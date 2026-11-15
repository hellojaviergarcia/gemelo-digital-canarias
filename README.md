# Gemelo digital de Canarias

Visor 3D gratuito y de código abierto de las siete islas Canarias, con una línea temporal (1400–2026), fichas de patrimonio natural, histórico, arqueológico y científico, y un recorrido guiado.

Funciona en un único archivo `index.html`, sin instalar nada: no hay `npm`, ni compilación, ni servidor propio.

> **Estado: prototipo.** Es una primera versión con 116 puntos. No es un inventario completo de Canarias. Consulta la sección [Limitaciones](#limitaciones) antes de usarlo como fuente.

## Qué incluye

- Globo 3D con terreno real de las siete islas (CesiumJS).
- **116 puntos de interés** con ficha: descripción, dato clave, fuente y origen de la coordenada.
- **Línea temporal (1400–2026):** los puntos aparecen según la época y el mapa cambia de tono en épocas antiguas.
- **Filtros** por tipo: volcán, yacimiento, naturaleza, patrimonio, ciencia.
- **Buscador** de lugares y botones de acceso a cada isla, con ficha de resumen.
- **Recorrido guiado** por todos los puntos, isla por isla, con subtítulos.
- **Reconstrucción 3D esquemática** de un tagoror y viviendas aborígenes en Gáldar (Gran Canaria).
- Soporte para superponer mapas históricos georreferenciados (ver más abajo).

### Puntos por isla

| Isla | Puntos |
|---|---|
| La Palma | 18 |
| El Hierro | 5 |
| La Gomera | 7 |
| Tenerife | 34 |
| Gran Canaria | 26 |
| Lanzarote | 14 |
| Fuerteventura | 12 |
| **Total** | **116** |

## Cómo ejecutarlo

Cesium no puede cargar sus recursos si abres el archivo con doble clic (`file://`), así que hay que servirlo por `http`.

1. Descarga o clona el repositorio.
2. Consigue un token gratuito en [ion.cesium.com](https://ion.cesium.com) → *Access Tokens*.
3. Abre `index.html` y pega tu token en la constante `TOKEN`:
   ```js
   const TOKEN = "TU_TOKEN_GRATIS_DE_CESIUM_ION";
   ```
4. Sirve la carpeta en local:
   ```bash
   python -m http.server 8000
   ```
5. Entra en `http://localhost:8000/index.html`.

Alternativas a Python: la extensión *Live Server* de VS Code, o publicarlo en GitHub Pages.

**Sin token** el visor funciona con un mapa plano de OpenStreetMap y sin terreno 3D.

> No subas tu token a un repositorio público sin restringirlo a tu dominio en el panel de Cesium ion.

## Estructura del repositorio

| Archivo | Contenido |
|---|---|
| `index.html` | El visor completo. Los puntos están incluidos dentro del código (`POIS`). |
| `pois.geojson` | Exportación de los 116 puntos en GeoJSON, con la fuente de cada coordenada. |
| `FUENTES.csv` | Tabla con cada punto, su coordenada, la fuente de la coordenada, la fuente del dato y si el dato está verificado. |
| `LICENSE` | Licencia MIT del código. |
| `LICENSE-DATOS.md` | Licencia CC BY 4.0 de los datos. |

`pois.geojson` y `FUENTES.csv` son exportaciones del contenido de `index.html`: editar estos dos archivos **no** cambia el visor.

## Datos y fuentes

### Coordenadas

Cada ficha muestra de dónde sale su coordenada. Estado actual:

- **112 de 116 contrastadas con una fuente.** Las fuentes son: GeoNames (centros urbanos), Wikipedia, UNESCO, el IAC, INVOLCAN (Guía Geoturística de Tenerife), el Cabildo de Gran Canaria, el Ayuntamiento de Tinajo y Sygic.
- **4 aproximadas**, marcadas como tales en `FUENTES.csv`: El Julan, Montaña de Tindaya, Los Tilos y Parque Rural de Anaga.

### Descripciones

**Las descripciones y los datos de las fichas no están verificados.** La columna `dato_verificado` de `FUENTES.csv` está en `NO` para todos los puntos. Contrasta cualquier dato con una fuente oficial antes de citarlo.

### Mapas base

- Terreno e imágenes: Cesium ion (requiere token).
- Mapa plano sin token: © colaboradores de OpenStreetMap.

## Cómo añadir o corregir un punto

Los puntos se definen en el array `POIS` de `index.html`:

```js
{n:"Nombre",is:"Isla",tipo:"Patrimonio",lat:28.0000,lon:-16.0000,d:1500,h:2026,
 t:"Descripción.",dato:"Dato clave.",f:"Fuente del dato"}
```

| Campo | Significado |
|---|---|
| `n` | Nombre |
| `is` | Isla (`La Palma`, `El Hierro`, `La Gomera`, `Tenerife`, `Gran Canaria`, `Lanzarote`, `Fuerteventura`) |
| `tipo` | `Volcán`, `Yacimiento`, `Naturaleza`, `Patrimonio` o `Ciencia` |
| `lat`, `lon` | Coordenadas en grados decimales (WGS84) |
| `d`, `h` | Años entre los que el punto es visible en la línea temporal |
| `t`, `dato`, `f` | Descripción, dato clave y fuente del dato |

Después, añade en el objeto `COORD` la fuente de su coordenada. Si no existe, el visor la muestra como "Aproximada, sin verificar".

## Mapas históricos

El visor admite capas de mapas históricos como teselas XYZ georreferenciadas (por ejemplo, con QGIS o [Allmaps](https://allmaps.org)). Edita la constante `MAPAS` e indica el rango de años y la URL de las teselas:

```js
const MAPAS = [
  {desde:1400,hasta:1799,url:"https://tu-dominio/tiles/{z}/{x}/{y}.png",nombre:"Cartografía siglo XVI"}
];
```

**Actualmente no se incluye ningún mapa histórico:** `MAPAS` tiene las URLs vacías.

## Contribuir

Las correcciones son bienvenidas, sobre todo las de datos. Si encuentras una coordenada mal ubicada o un dato erróneo, abre un *issue* indicando el punto y la fuente que lo contradice.

## Licencia

- **Código** (`index.html`): [MIT](LICENSE).
- **Datos** (`pois.geojson`, `FUENTES.csv`): [CC BY 4.0](LICENSE-DATOS.md).

Las coordenadas proceden de las fuentes indicadas, cada una con sus propias condiciones. Las descripciones son texto propio.

## Créditos

- [CesiumJS](https://cesium.com/platform/cesiumjs/) y Cesium ion para el globo 3D y el terreno.
- [GeoNames](https://www.geonames.org) y [OpenStreetMap](https://www.openstreetmap.org/copyright) para datos geográficos.
- [jsDelivr](https://www.jsdelivr.com) como CDN de CesiumJS.

## Autor

**Javier García**

- GitHub: [@hellojaviergarcia](https://github.com/hellojaviergarcia)
- LinkedIn: [hellojaviergarcia](https://www.linkedin.com/in/hellojaviergarcia/)
