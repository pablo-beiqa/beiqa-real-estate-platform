# ADR-001: Elección de Base de Datos

**Fecha**: 2026-02-04  
**Estado**: Propuesto  
**Decisores**: [Por definir]

---

## Contexto

BEIQA necesita una base de datos que soporte:
- Datos relacionales tradicionales (propiedades, clientes, búsquedas)
- Datos geoespaciales (coordenadas, polígonos, búsquedas por área)
- Volumen estimado de 50K+ propiedades
- Consultas complejas con JOINs y filtros múltiples
- Historial de datos (5+ años)

---

## Opciones Consideradas

### 1. PostgreSQL + PostGIS

**Descripción**: Base de datos relacional open-source con extensión geoespacial madura.

✅ **Pros**:
- Soporte geoespacial más completo del mercado (PostGIS)
- ACID compliant
- Gran comunidad y documentación
- Open source (sin vendor lock-in)
- Múltiples opciones de hosting (AWS RDS, GCP CloudSQL, Supabase, etc.)
- Maduro y estable

❌ **Cons**:
- Requiere conocimiento específico de PostGIS
- Setup más complejo que alternativas managed
- Tuning de performance requiere expertise

### 2. MySQL + MySQL Spatial

**Descripción**: Base de datos relacional popular con funciones espaciales.

✅ **Pros**:
- Muy conocido, fácil encontrar desarrolladores
- Buen ecosistema de herramientas
- Múltiples opciones de hosting

❌ **Cons**:
- Funciones geoespaciales menos completas que PostGIS
- No soporta algunos tipos de geometría avanzados
- Índices espaciales menos eficientes

### 3. MongoDB + Geospatial Indexes

**Descripción**: Base de datos de documentos con índices geoespaciales.

✅ **Pros**:
- Schema flexible
- Fácil de empezar (Atlas free tier)
- Bueno para datos heterogéneos

❌ **Cons**:
- No es ACID completo (important para datos de negocio)
- Geospatial limitado comparado con PostGIS
- Más costoso a escala
- Queries relacionales complejos son difíciles

### 4. Supabase (PostgreSQL managed)

**Descripción**: PostgreSQL hosted con APIs automáticas y dashboard.

✅ **Pros**:
- Setup muy rápido
- APIs REST/GraphQL automáticas
- Auth incluido
- PostGIS disponible
- Free tier generoso

❌ **Cons**:
- Menos control sobre configuración
- Vendor lock-in moderado
- Límites en operaciones avanzadas

---

## Análisis de Requerimientos vs Opciones

| Requerimiento | PostgreSQL+PostGIS | MySQL Spatial | MongoDB | Supabase |
|---------------|-------------------|---------------|---------|----------|
| Datos relacionales | ✅✅ | ✅✅ | ⚠️ | ✅✅ |
| Datos geoespaciales | ✅✅ | ⚠️ | ⚠️ | ✅✅ |
| Queries complejos | ✅✅ | ✅✅ | ⚠️ | ✅✅ |
| ACID | ✅✅ | ✅✅ | ⚠️ | ✅✅ |
| Facilidad setup | ⚠️ | ✅ | ✅✅ | ✅✅ |
| Escalabilidad | ✅✅ | ✅✅ | ✅✅ | ⚠️ |
| Costo inicial | ✅ | ✅ | ✅✅ | ✅✅ |
| Vendor lock-in | ✅✅ | ✅✅ | ⚠️ | ⚠️ |

---

## Decisión

**Decisión pendiente** - A validar en Fase 0.

### Recomendación Preliminar

**PostgreSQL + PostGIS** se recomienda preliminarmente porque:

1. **Geoespacial crítico**: El análisis geoespacial es core para BEIQA, y PostGIS es el estándar de la industria
2. **ACID necesario**: Los datos de negocio (clientes, búsquedas, deals) requieren transacciones confiables
3. **Sin vendor lock-in**: Open source permite flexibilidad de hosting
4. **Ecosistema maduro**: Herramientas, documentación, y comunidad robustas

### Opción de implementación

| Opción | Cuándo elegir |
|--------|---------------|
| Self-managed (EC2/VM) | Si tenemos DevOps expertise y queremos control total |
| AWS RDS | Si usamos AWS como cloud provider |
| GCP Cloud SQL | Si usamos GCP como cloud provider |
| Supabase | Para MVP rápido, migrar después si necesario |

---

## Validación Necesaria (Fase 0)

Antes de confirmar esta decisión:

1. [ ] Verificar experiencia del equipo con PostgreSQL
2. [ ] Hacer PoC con queries geoespaciales reales
3. [ ] Estimar costos de hosting en diferentes providers
4. [ ] Probar carga de datos geográficos (shapefiles, KML)
5. [ ] Validar performance con volumen esperado

---

## Consecuencias (si se confirma PostgreSQL)

### Positivas

- Capacidad geoespacial completa desde día 1
- Flexibilidad para queries complejos
- Herramientas de migración maduras (Alembic, Flyway)
- Integración con la mayoría de ORMs (SQLAlchemy, Prisma, TypeORM)

### Negativas

- Curva de aprendizaje para PostGIS
- Necesidad de tuning para queries geoespaciales complejos
- Setup inicial más trabajo que alternativas managed

### Riesgos

- Performance de queries espaciales con muchos polígonos - mitigar con índices apropiados
- Complejidad de migraciones geoespaciales - mitigar con buena práctica

---

## Notas Adicionales

- Considerar usar ORM que soporte bien geoespacial (GeoAlchemy2 para Python, PostGIS extensions para Prisma)
- Planificar estrategia de backups incluyendo datos geoespaciales
- Documentar convenciones de CRS (coordinate reference system) desde el inicio

---

*Relacionado con*: ADR-002 (Cloud Provider), ADR-003 (Backend Framework)
