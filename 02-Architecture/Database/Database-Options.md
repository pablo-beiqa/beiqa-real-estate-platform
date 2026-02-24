# Investigación: Hosting de Base de Datos

**Fase**: R-2 (Tecnología y Plataforma)
**Estado**: 🔴 Por investigar
**Depende de**: Tech Stack Decision (ADR-005)

---

## Decisión Tomada: Motor de Base de Datos

> **PostgreSQL + PostGIS es la decisión clara** para BEIQA. No hay alternativa que ofrezca el mismo nivel de soporte geoespacial + relacional + text search en un solo motor.
>
> La pregunta abierta es: **¿DÓNDE hostear PostgreSQL + PostGIS?**

### Justificación de PostgreSQL + PostGIS

| Requerimiento | PostgreSQL + PostGIS | Alternativas |
|---------------|---------------------|-------------|
| Queries relacionales (JOIN, WHERE) | ✅ Excelente | MySQL: ✅, MongoDB: ⚠️ |
| Queries geoespaciales (within, distance, intersects) | ✅ Excelente (PostGIS) | MySQL Spatial: ⚠️ Básico, MongoDB: ⚠️ |
| Búsqueda full-text en español | ✅ Bueno (tsvector, unaccent) | Bueno en todas |
| Índices eficientes (GiST, GIN, BRIN) | ✅ Excelente | Limitado en otras |
| ACID compliance | ✅ | ✅ MySQL, ⚠️ MongoDB |
| Open source, sin vendor lock-in | ✅ | ✅ |

---

## Opciones de Hosting a Evaluar

### Opción 1: Supabase

| Criterio | Evaluación |
|----------|------------|
| PostGIS disponible | ✅ Sí (extensión habilitada) |
| Free tier | 500 MB, 50K rows |
| Pro tier | $25/mes -- 8 GB, 500K rows |
| Backups | Automáticos (diarios en Pro) |
| APIs automáticas | ✅ REST + GraphQL generados |
| Auth incluido | ✅ Supabase Auth |
| Storage incluido | ✅ Supabase Storage |
| Dashboard | ✅ SQL editor, table view |
| Región | 🔴 ¿Cuál es la más cercana a CDMX? |
| Latencia estimada | 🔴 Por medir |

**Preguntas:**
- [ ] ¿PostGIS en Supabase tiene todas las funciones necesarias (ST_DWithin, ST_Intersects, etc.)?
- [ ] ¿El límite de 8 GB en Pro es suficiente para el volumen estimado?
- [ ] ¿Se puede acceder directamente a PostgreSQL (no solo via Supabase APIs)?
- [ ] ¿Qué pasa si necesitamos migrar de Supabase?

### Opción 2: AWS RDS for PostgreSQL

| Criterio | Evaluación |
|----------|------------|
| PostGIS disponible | ✅ Sí |
| Free tier | 12 meses, db.t3.micro |
| Costo estimado (t3.small + 20GB) | ~$30-50/mes |
| Backups | Automáticos, snapshots |
| Multi-AZ | Disponible (costo extra) |
| Región cercana a CDMX | us-east-1 (Virginia) |
| Latencia estimada | 🔴 Por medir |

**Preguntas:**
- [ ] ¿Cuál es la instancia mínima para nuestro volumen?
- [ ] ¿Cuánto cuesta el storage adicional?
- [ ] ¿Se necesita VPC/Security Groups especiales?

### Opción 3: GCP Cloud SQL for PostgreSQL

| Criterio | Evaluación |
|----------|------------|
| PostGIS disponible | ✅ Sí |
| Free tier | $300 crédito por 90 días |
| Costo estimado | ~$30-60/mes |
| Backups | Automáticos |
| Región cercana a CDMX | us-central1 (Iowa) |

### Opción 4: Neon (Serverless PostgreSQL)

| Criterio | Evaluación |
|----------|------------|
| PostGIS disponible | ✅ Sí |
| Free tier | 0.5 GiB storage, 100 hours/mes |
| Launch tier | $19/mes -- 10 GiB, 300 hours |
| Serverless | ✅ Escala a cero cuando no se usa |
| Branching | ✅ Base de datos de prueba instantánea |
| Latencia estimada | 🔴 Por medir (cold start?) |

**Preguntas:**
- [ ] ¿El cold start afecta queries después de inactividad?
- [ ] ¿PostGIS funciona sin limitaciones en Neon?
- [ ] ¿300 compute hours/mes son suficientes?

### Opción 5: Railway

| Criterio | Evaluación |
|----------|------------|
| PostGIS disponible | 🔴 Por verificar |
| Costo | $5/mes base + uso |
| Simplicidad | ✅ Muy fácil |
| Backups | 🔴 Por verificar |

### Opción 6: Self-Hosted (VPS)

| Criterio | Evaluación |
|----------|------------|
| PostGIS disponible | ✅ Sí (instalar manualmente) |
| Costo | $10-40/mes (DigitalOcean, Hetzner) |
| Control | ✅ Total |
| Mantenimiento | ❌ Manual (updates, backups, security) |
| Justificación | Solo si necesitamos control absoluto |

---

## Matriz de Comparación Actualizada

| Criterio | Supabase | AWS RDS | GCP Cloud SQL | Neon | Railway |
|----------|----------|---------|---------------|------|---------|
| PostGIS | ✅ | ✅ | ✅ | ✅ | 🔴 |
| Costo MVP | ✅ $0-25 | ⚠️ $30-50 | ⚠️ $30-60 | ✅ $0-19 | ✅ $5-20 |
| Facilidad setup | ✅ Muy fácil | ⚠️ Medio | ⚠️ Medio | ✅ Fácil | ✅ Fácil |
| Backups auto | ✅ | ✅ | ✅ | ✅ | 🔴 |
| Extras incluidos | ✅ Auth, Storage, APIs | ❌ | ❌ | ❌ | ❌ |
| Lock-in | ⚠️ Medio | ⚠️ Alto | ⚠️ Alto | ⚠️ Bajo | ⚠️ Bajo |
| Escalabilidad | ⚠️ Media | ✅ Alta | ✅ Alta | ✅ Alta | ⚠️ Media |

---

## Almacenamiento de Archivos (Complementario)

La base de datos no almacena archivos pesados. Se necesita object storage separado:

| Opción | Costo | Sin egress fees | Compatible S3 |
|--------|-------|-----------------|---------------|
| Cloudflare R2 | Bajo | ✅ | ✅ |
| AWS S3 | Bajo | ❌ | ✅ |
| Supabase Storage | Incluido | Varía | No |
| GCP Cloud Storage | Bajo | ❌ | Parcial |

> **Recomendación**: Si se elige Supabase, usar Supabase Storage. Si no, Cloudflare R2 por zero egress.

---

## Decisión (Por Tomar)

🔴 _Depende de ADR-004 (Cloud Provider) y evaluación de costos reales_

---

## Acciones de Investigación

1. [ ] Verificar PostGIS completo en Supabase (funciones, extensiones)
2. [ ] Verificar PostGIS en Neon
3. [ ] Verificar PostGIS en Railway
4. [ ] Medir latencia desde CDMX para cada opción (ping test)
5. [ ] Estimar storage necesario (# propiedades × tamaño promedio)
6. [ ] Calcular costo mensual real para cada opción
7. [ ] Hacer PoC con la opción más prometedora
8. [ ] Documentar decisión final
