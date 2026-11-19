# gml-postgis-importer

Servicio HTTP que **importa cartografía (GML, ZIP y otros formatos de GDAL) a PostgreSQL/PostGIS**. Lo uso desde **n8n** para cargar parcelas catastrales en la base de datos geoespacial de mis automatizaciones.

## Cómo funciona

`POST /import` acepta:

- un archivo binario (campo `data` o `file`, tal como lo envía n8n), o
- una URL de descarga (`sourceUrl`).

El servicio detecta si la capa trae sistema de referencia, la reproyecta al SRID indicado (por defecto `25830`) y la carga con `ogr2ogr` en la tabla elegida.

| Parámetro | Por defecto | Descripción |
|---|---|---|
| `table` | `parcelas_muros` | Tabla destino |
| `srid` | `25830` | Sistema de referencia de destino |
| `promoteToMulti` | `true` | Convierte geometrías a multi |
| `layerName` | — | Capa concreta del archivo |

Conexión configurada por variables de entorno: `PGHOST`, `PGPORT`, `PGDATABASE`, `PGUSER`, `PGPASSWORD`, `PGSSLMODE`.

## Ejecución

```bash
docker build -t gml-postgis-importer .
docker run -p 8080:8080 -e PGHOST=... -e PGDATABASE=... -e PGUSER=... -e PGPASSWORD=... gml-postgis-importer
```

## Stack

Node.js · Express · Multer · GDAL (`ogr2ogr`) · PostGIS · Docker
