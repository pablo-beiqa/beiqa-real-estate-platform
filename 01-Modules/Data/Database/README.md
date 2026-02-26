# Database (Supabase)

**Estado**: 🔴 Por iniciar
**Owner**: Por definir

---

## Descripcion

Sub-modulo que documenta la base de datos PostgreSQL + PostGIS alojada en Supabase.
Cubre la implementacion concreta: tablas, storage, extensiones y configuracion del
entorno Supabase que soporta toda la plataforma BEIQA.

---

## Componentes en Supabase

### PostgreSQL + PostGIS
- Base de datos relacional con extension PostGIS para queries geoespaciales
- Tablas core: PROPERTY, PROPERTY_SOURCE, PROPERTY_IMAGE, PROPERTY_TYPE
- Tablas de clientes: CLIENT, SEARCH_REQUEST, SEARCH_PROPERTY, COMMUNICATION
- Tablas de zona: ZONE, ZONE_DATA, MARKET_PRICE, VACANCY_RATE
- Indices GIST en columnas Geography para busquedas espaciales

### Supabase Storage
- Bucket para imagenes de propiedades (descargadas por el Scraper)
- URLs de storage asociadas a cada propiedad via PROPERTY_IMAGE

### Extensiones Habilitadas
- PostGIS — Tipos Geography(Point, 4326) y Geography(Polygon, 4326)
- H3 (por evaluar) — Asignacion de hexagonos a propiedades

---

## Relacion con Arquitectura

El **diseno** del esquema (entidades, relaciones, tipos de datos) esta documentado en:
- [Data-Model.md](../../../02-Architecture/Database/Data-Model.md) — Esquema completo
- [Database-Options.md](../../../02-Architecture/Database/Database-Options.md) — Evaluacion de hosting
- [Product-Questions.md](../../../02-Architecture/Database/Product-Questions.md) — Preguntas de discovery

Este sub-modulo documenta la **implementacion** en Supabase de ese diseno.

---

## Dependencias

### Necesita (upstream)
- **02-Architecture/Database/** → Diseno del esquema y decisiones arquitectonicas

### Depende de este (downstream)
- **Scraper** → Escribe propiedades e imagenes a Supabase
- **Data / Normalization** → Opera sobre datos en Supabase (dedup, H3, AGEB)
- **Internal App** → Lee datos de Supabase para la interfaz del equipo
- **Todos los modulos** → Supabase es la fuente de verdad central

---

## Documentos Relacionados

- [Data-Model.md](../../../02-Architecture/Database/Data-Model.md) — Esquema de base de datos
- [ADR-001-Database-Choice.md](../../../02-Architecture/ADRs/ADR-001-Database-Choice.md) — Decision PostgreSQL + PostGIS
- [Normalization README](../Normalization/README.md) — Pipeline que opera sobre estos datos
