# Estimación de Presupuesto Total

---

## ✅ Costos Reales (Febrero 2026)

> El proyecto está en desarrollo activo con un equipo interno. Los costos reales son una fracción de las estimaciones originales de outsource.

| Herramienta | Costo mensual | Notas |
|-----------|--------------|-------|
| Apify (scraping) | ~$20/mes | Actor contratado para Inmuebles24 |
| Supabase (DB + Auth + Storage) | $0-25/mes | Free tier → Pro plan |
| n8n Cloud (orquestación) | Incluido en plan | Workflows, cron, integraciones |
| Trigger.dev (jobs pesados) | Free tier | Batch AI extraction |
| Claude API vía Rube | Por tokens (~mínimo) | Procesamiento de descripciones |
| HubSpot CRM | Plan existente | Sync vía n8n |
| **Total herramientas** | **~$30-50/mes** | |

**Desarrollo**: Interno (Fabrizio + Pamela). Sin costo de outsource.

---

## Estimación Original (Discovery Phase — Referencia Histórica)

*Las estimaciones originales asumían outsource y un equipo por contratar. Se mantienen como referencia histórica del proceso de descubrimiento.*

**Fase**: R-3 (Validación y Factibilidad)
**Estado original**: Análisis de viabilidad (completado)
**Presupuesto disponible**: $50,000 - $100,000 USD
**Equipo original estimado**: Modelo híbrido (outsource + in-house)

---

## 1. Costos de APIs y Servicios (Mensual)

| Servicio | Uso Estimado | Precio | Costo/Mes | Notas |
|----------|-------------|--------|-----------|-------|
| **Google Maps Platform** | | | | |
| - Maps JavaScript API | ~500 cargas | $7/1000 | ~$3.50 | Cubierto por crédito |
| - Geocoding API | ~500 requests | $5/1000 | ~$2.50 | Cubierto por crédito |
| - Places API | ~500 requests | $17/1000 | ~$8.50 | Cubierto por crédito |
| - Routes API | ~100 requests | $5/1000 | ~$0.50 | Cubierto por crédito |
| - Crédito gratuito | | | -$200 | Cubre todo lo anterior |
| **Subtotal Google** | | | **$0** | Dentro del crédito |
| | | | | |
| **INEGI / Gobierno** | Ilimitado | Gratis | **$0** | |
| | | | | |
| **LLM (deduplicación MVP)** | ~200 pares/mes | ~400 tokens/par | **< $1** | GPT-4o-mini o Haiku |
| | | | | |
| **Proveedores comerciales** | N/A para MVP | N/A | **$0** | Diferido a post-MVP |
| | | | | |
| **TOTAL APIs MVP** | | | **~$1/mes** | |

---

## 2. Costos de Infraestructura (Mensual)

### Escenario A: Stack Simple (Supabase + Vercel/Railway)

| Recurso | Especificación | Provider | Costo/Mes |
|---------|---------------|----------|-----------|
| Base de datos | PostgreSQL + PostGIS, 8 GB | Supabase Pro | $25 |
| Auth | Incluido en Supabase | Supabase | $0 |
| Object storage | Incluido o Cloudflare R2 | Supabase/R2 | $0-5 |
| App backend | 1 servicio | Railway/Render | $10-25 |
| App frontend | Static/SSR | Vercel Free | $0 |
| Dominio | .com | Cualquiera | ~$1 |
| SSL | Let's Encrypt | Gratis | $0 |
| Monitoring | Sentry Free + Uptime Robot | Free tiers | $0 |
| **TOTAL Escenario A** | | | **$36-56/mes** |

### Escenario B: Stack Cloud (AWS/GCP)

| Recurso | Especificación | Provider | Costo/Mes |
|---------|---------------|----------|-----------|
| Base de datos | RDS PostgreSQL t3.small + PostGIS | AWS | $40-60 |
| App server | EC2 t3.small o Cloud Run | AWS/GCP | $20-40 |
| Object storage | S3/R2 (< 10 GB) | AWS/R2 | $1-5 |
| CDN | CloudFront/Cloudflare | AWS/CF | $0-5 |
| Dominio | .com | Route53 | ~$1 |
| SSL | ACM / Let's Encrypt | Gratis | $0 |
| Monitoring | CloudWatch basic | AWS | $0-5 |
| **TOTAL Escenario B** | | | **$62-116/mes** |

### Escenario C: Stack Serverless (Neon + Vercel)

| Recurso | Especificación | Provider | Costo/Mes |
|---------|---------------|----------|-----------|
| Base de datos | PostgreSQL + PostGIS, serverless | Neon Launch | $19 |
| App backend | Serverless functions | Vercel Pro | $20 |
| Object storage | Cloudflare R2 | R2 | $0-5 |
| Dominio + CDN | Incluido | Vercel/CF | $0-1 |
| **TOTAL Escenario C** | | | **$39-45/mes** |

---

## 3. Costos de Desarrollo (One-time)

### Opción: Equipo Outsource (LatAm)

| Fase | Horas Estimadas | Costo/Hora | Total |
|------|----------------|------------|-------|
| Research & Design (ya en curso) | 40-80h | $40-60 | $1,600-$4,800 |
| MVP Backend (API, DB, adquisición datos) | 200-300h | $40-60 | $8,000-$18,000 |
| MVP Frontend (interfaz, mapas) | 150-250h | $40-60 | $6,000-$15,000 |
| Deduplication engine | 40-80h | $50-70 | $2,000-$5,600 |
| GIS / Mapas | 60-100h | $50-70 | $3,000-$7,000 |
| Testing & QA | 40-80h | $30-50 | $1,200-$4,000 |
| Deploy & DevOps | 20-40h | $40-60 | $800-$2,400 |
| **TOTAL Outsource** | **550-930h** | | **$22,600-$56,800** |

### Opción: Equipo In-House (México)

| Rol | Meses | Salario Mensual | Total |
|-----|-------|----------------|-------|
| Backend developer (senior) | 3-4 | $40,000-$60,000 MXN | $120K-$240K MXN |
| Frontend developer (mid) | 2-3 | $30,000-$45,000 MXN | $60K-$135K MXN |
| **TOTAL In-House** | | | ~$180K-$375K MXN ($9K-$19K USD) |

### Opción: Híbrido (Recomendada)

| Componente | Quién | Costo Estimado |
|------------|-------|----------------|
| Arquitectura + MVP backend | Outsource senior | $15,000-$25,000 |
| Frontend + UX | Outsource mid | $8,000-$15,000 |
| Supervisión + product management | In-house (Pablo) | $0 (tiempo propio) |
| **TOTAL Híbrido** | | **$23,000-$40,000** |

---

## 4. Costos Recurrentes (Mensual Post-Launch)

| Concepto | Escenario A | Escenario B | Escenario C |
|----------|------------|------------|------------|
| Infraestructura | $36-56 | $62-116 | $39-45 |
| APIs | ~$1 | ~$1 | ~$1 |
| Mantenimiento (dev) | $500-1,500 | $500-1,500 | $500-1,500 |
| **TOTAL MENSUAL** | **$537-1,557** | **$563-1,617** | **$540-1,546** |

---

## 5. Resumen de Inversión

### Inversión Inicial (Desarrollo)

| Concepto | Mínimo | Máximo |
|----------|--------|--------|
| Desarrollo (híbrido) | $23,000 | $40,000 |
| Setup infraestructura | $500 | $1,000 |
| **TOTAL INICIAL** | **$23,500** | **$41,000** |

### Costo Operativo Primer Año (12 meses)

| Concepto | Mínimo | Máximo |
|----------|--------|--------|
| Infraestructura (Escenario A × 12) | $432 | $672 |
| APIs (× 12) | $12 | $12 |
| Mantenimiento (× 12) | $6,000 | $18,000 |
| **TOTAL OPERATIVO AÑO 1** | **$6,444** | **$18,684** |

### Inversión Total Año 1

| Concepto | Mínimo | Máximo |
|----------|--------|--------|
| Desarrollo | $23,500 | $41,000 |
| Operación año 1 | $6,444 | $18,684 |
| Buffer imprevistos (20%) | $5,989 | $11,937 |
| **TOTAL AÑO 1** | **$35,933** | **$71,621** |

> ✅ **Dentro del presupuesto de $50K-$100K**

---

## 6. Variables que Afectan Costos

| Variable | Impacto | Cómo Mitigar |
|----------|---------|-------------|
| Scope creep (más features) | +$5K-$20K por módulo | Definir MVP estricto y cumplirlo |
| Volumen de datos | Mayor storage/processing | Escalar gradualmente |
| Uso de Google Maps | Pay per use | Evaluar alternativas gratuitas, cachear |
| Features de AI post-MVP | Tokens LLM | Usar modelos mini/haiku, optimizar prompts |
| Número de portales | Más mantenimiento de scrapers | Empezar con 1, expandir solo si necesario |
| Team ramp-up | Tiempo de onboarding | Documentar bien, usar stack común |

---

## 7. Recomendaciones de Ahorro

| Área | Recomendación | Ahorro Estimado |
|------|--------------|-----------------|
| Mapas | Leaflet + OSM para funciones básicas, Google solo para geocoding | $0-15/mes |
| Hosting | Stack simple (Supabase + Vercel/Railway) | $30-60/mes vs cloud |
| AI | GPT-4o-mini / Haiku en vez de GPT-4o | ~90% en tokens |
| Frontend | Low-code (Retool) para MVP interno | $5,000-$10,000 en dev |
| Portales | Solo EasyBroker para MVP | $5,000-$10,000 en dev de scrapers |

---

## 8. Próximos Pasos

1. [ ] Completar investigación de R-1 y R-2 para refinar estimaciones
2. [ ] Obtener cotizaciones de equipos de desarrollo
3. [ ] Elegir escenario de infraestructura
4. [ ] Calcular costos específicos basados en decisiones de stack
5. [ ] Presentar presupuesto a stakeholders para aprobación
6. [ ] Definir presupuesto máximo y contingencia

---

## Notas

- Todos los costos son estimaciones preliminares basadas en precios públicos y rangos de mercado
- Se requiere validación con cotizaciones reales de equipos de desarrollo
- Los costos de infraestructura asumen uso en régimen MVP (bajo volumen)
- Buffer de 20% recomendado para imprevistos
- Los costos de mantenimiento asumen ~10-20 horas/mes de soporte técnico
