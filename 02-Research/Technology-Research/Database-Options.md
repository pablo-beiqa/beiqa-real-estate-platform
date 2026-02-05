# Investigación: Opciones de Base de Datos

## Objetivo

Evaluar opciones de base de datos para el almacenamiento de datos de BEIQA, considerando requerimientos relacionales y geoespaciales.

**Estado**: 🔴 Por investigar

---

## Requerimientos de Almacenamiento

### Tipos de Datos a Almacenar

| Tipo | Volumen Estimado | Características |
|------|------------------|-----------------|
| Propiedades | 50K+ registros | Relacional + coordenadas |
| Clientes | 20+ registros | Relacional |
| Búsquedas | Variable | Relacional |
| Comunicaciones | Alto volumen | Texto + metadata |
| Documentos | Variable | Archivos (separado) |
| Geometrías | Muchas capas | Polígonos, puntos, líneas |
| Series temporales | Histórico | Precios, vacancy por tiempo |

### Requerimientos Funcionales

- [ ] Queries relacionales estándar (JOIN, WHERE, etc.)
- [ ] Queries geoespaciales (within, distance, intersects)
- [ ] Búsqueda de texto completo
- [ ] Índices eficientes para consultas frecuentes
- [ ] ACID compliance
- [ ] Backups automatizados
- [ ] Escalabilidad futura

---

## Opciones a Evaluar

### Opción 1: PostgreSQL + PostGIS

| Criterio | Evaluación |
|----------|------------|
| **Descripción** | DB relacional con extensión geoespacial madura |
| **Pros** | - Soporte geoespacial completo - Maduro y estable - Gran comunidad - Open source - ACID compliant |
| **Cons** | - Requiere conocimiento específico PostGIS - Setup más complejo |
| **Costo** | Open source (gratis), hosting varía |
| **Experiencia del equipo** | 🔴 Por verificar |
| **Hosting options** | AWS RDS, GCP Cloud SQL, Azure, self-hosted |

**Preguntas de investigación**:
- [ ] ¿El equipo tiene experiencia con PostgreSQL?
- [ ] ¿Qué hosting option es más conveniente?
- [ ] ¿Cuál es el costo estimado de hosting?

---

### Opción 2: MySQL + MySQL Spatial

| Criterio | Evaluación |
|----------|------------|
| **Descripción** | DB relacional popular con funciones espaciales |
| **Pros** | - Muy conocido - Fácil de usar - Buen hosting disponible |
| **Cons** | - Funciones geoespaciales menos completas que PostGIS - Limitaciones en tipos de geometría |
| **Costo** | Open source, hosting varía |
| **Experiencia del equipo** | 🔴 Por verificar |

---

### Opción 3: MongoDB + Geospatial Indexes

| Criterio | Evaluación |
|----------|------------|
| **Descripción** | DB de documentos con soporte geoespacial |
| **Pros** | - Schema flexible - Bueno para datos heterogéneos - Fácil de empezar |
| **Cons** | - No ACID completo - Geospatial limitado vs PostGIS - Más costoso a escala |
| **Costo** | Atlas free tier, luego pay per use |
| **Experiencia del equipo** | 🔴 Por verificar |

---

### Opción 4: Supabase (PostgreSQL managed)

| Criterio | Evaluación |
|----------|------------|
| **Descripción** | PostgreSQL hosted con APIs automáticas |
| **Pros** | - Setup muy rápido - APIs REST automáticas - Auth incluido - PostGIS disponible |
| **Cons** | - Menos control - Vendor lock-in - Límites en free tier |
| **Costo** | Free tier, luego $25/mes+ |

---

### Opción 5: PlanetScale (MySQL managed)

| Criterio | Evaluación |
|----------|------------|
| **Descripción** | MySQL serverless con branching |
| **Pros** | - Serverless - Branching para desarrollo - Escalable |
| **Cons** | - Sin soporte geoespacial robusto - Relativamente nuevo |
| **Costo** | Free tier limitado |

---

## Matriz de Comparación

| Criterio | PostgreSQL+PostGIS | MySQL Spatial | MongoDB | Supabase |
|----------|-------------------|---------------|---------|----------|
| Relacional | ✅ Excelente | ✅ Excelente | ⚠️ Limitado | ✅ Excelente |
| Geoespacial | ✅ Excelente | ⚠️ Básico | ⚠️ Básico | ✅ Excelente |
| Full-text search | ✅ Bueno | ✅ Bueno | ✅ Bueno | ✅ Bueno |
| Facilidad setup | ⚠️ Media | ✅ Fácil | ✅ Fácil | ✅ Muy fácil |
| Costo inicial | ✅ Bajo | ✅ Bajo | ✅ Bajo | ✅ Gratis |
| Escalabilidad | ✅ Alta | ✅ Alta | ✅ Alta | ⚠️ Media |
| Vendor lock-in | ✅ Ninguno | ✅ Ninguno | ⚠️ Medio | ⚠️ Medio |

---

## Almacenamiento de Archivos (Separado)

| Opción | Descripción | Costo |
|--------|-------------|-------|
| AWS S3 | Object storage de AWS | Pay per use |
| Google Cloud Storage | Object storage de GCP | Pay per use |
| Azure Blob | Object storage de Azure | Pay per use |
| MinIO | Self-hosted S3-compatible | Self-hosted |
| Cloudflare R2 | S3-compatible, sin egress fees | Pay per use |

**Decisión**: Dependerá del cloud provider elegido.

---

## Recomendación Preliminar

**Para BEIQA, la recomendación preliminar es PostgreSQL + PostGIS** porque:

1. Mejor soporte geoespacial (crítico para el módulo Geospatial)
2. ACID compliance (importante para datos de negocio)
3. Maduro y estable
4. Buenas opciones de hosting managed
5. Open source (sin vendor lock-in)

**Sin embargo**, esta decisión debe validarse con:
- [ ] Experiencia del equipo de desarrollo
- [ ] Costos de hosting en el provider elegido
- [ ] Prueba de concepto con datos reales

---

## Próximos Pasos

1. [ ] Verificar experiencia del equipo con PostgreSQL
2. [ ] Estimar costos de hosting (AWS RDS vs Supabase vs otros)
3. [ ] Hacer PoC con datos de muestra
4. [ ] Documentar decisión en ADR

---

## Hallazgos

*(Documentar aquí los resultados de la investigación)*
