# Evaluación de Proveedores Comerciales de Datos

## Objetivo

Investigar y evaluar proveedores comerciales de datos inmobiliarios y de mercado para México.

**Estado**: 🔴 Por investigar

---

## Categorías de Proveedores

### 1. Consultoras Inmobiliarias Internacionales

Estas empresas publican reportes de mercado y algunos ofrecen APIs o servicios de datos.

#### CBRE

| Aspecto | Información |
|---------|-------------|
| Website | cbre.com.mx |
| Presencia México | Sí, oficinas en CDMX, MTY, GDL |
| Reportes públicos | Trimestrales por sector |
| Datos de mercado | Industrial, oficinas, retail |
| API/Datos | 🔴 Por verificar |
| Costo | 🔴 Por cotizar |

**Acciones**:
- [ ] Contactar para información sobre servicios de datos
- [ ] Revisar reportes públicos disponibles
- [ ] Evaluar partnership potential

#### JLL (Jones Lang LaSalle)

| Aspecto | Información |
|---------|-------------|
| Website | jll.com.mx |
| Presencia México | Sí |
| Reportes públicos | Trimestrales |
| Datos de mercado | Industrial, oficinas |
| API/Datos | 🔴 Por verificar |

#### Cushman & Wakefield

| Aspecto | Información |
|---------|-------------|
| Website | cushmanwakefield.com |
| Presencia México | Sí |
| Reportes públicos | Sí |
| Datos de mercado | Todos los sectores |

#### Newmark

| Aspecto | Información |
|---------|-------------|
| Website | nmrk.com |
| Presencia México | Sí |

#### Colliers

| Aspecto | Información |
|---------|-------------|
| Website | colliers.com/es-mx |
| Presencia México | Sí |
| Reportes públicos | Sí |

---

### 2. Proveedores de Datos Especializados

#### CoStar (Internacional)

| Aspecto | Información |
|---------|-------------|
| Website | costar.com |
| Cobertura México | 🔴 Por verificar |
| Tipo de datos | Database completa de propiedades comerciales |
| API | Sí |
| Costo | Alto (enterprise) |

**Nota**: CoStar es el estándar en US. Verificar si tienen cobertura México.

#### Real Capital Analytics

| Aspecto | Información |
|---------|-------------|
| Website | rcanalytics.com |
| Tipo de datos | Transacciones de inversión |
| Cobertura México | 🔴 Por verificar |

#### Yardi Matrix

| Aspecto | Información |
|---------|-------------|
| Website | yardimatrix.com |
| Tipo de datos | Market data, analytics |
| Cobertura México | 🔴 Por verificar |

---

### 3. Proveedores Locales México

#### Solili

| Aspecto | Información |
|---------|-------------|
| Website | solili.mx |
| Especialización | Inmuebles industriales |
| Tipo de datos | Listings, market data |
| API | 🔴 Por verificar |
| Costo | 🔴 Por cotizar |

**Alta prioridad** para sector industrial.

#### SoftPro

| Aspecto | Información |
|---------|-------------|
| Website | softpro.com.mx |
| Tipo | Software de valuación |
| Datos incluidos | Comparables, mercado |
| Cobertura | Nacional |

#### Datoz

| Aspecto | Información |
|---------|-------------|
| Website | datoz.com |
| Tipo | Data analytics inmobiliario |
| Cobertura | México |

#### 4S Real Estate

| Aspecto | Información |
|---------|-------------|
| Website | 4srealestate.com |
| Tipo | Consultoría con datos |
| Especialización | Residencial y comercial |

---

### 4. Agregadores de Listings

#### Properati

| Aspecto | Información |
|---------|-------------|
| Website | properati.com.mx |
| Tipo | Agregador de listings |
| API | Posiblemente |

#### Segundamano / Vivanuncios (eBay Classifieds)

| Aspecto | Información |
|---------|-------------|
| Tipo | Marketplace |
| API | No pública |

---

## Matriz de Evaluación

| Proveedor | Cobertura MX | Comercial/Industrial | API | Costo Est. | Prioridad |
|-----------|--------------|---------------------|-----|------------|-----------|
| CBRE | ✅ | ✅ | ? | Alto | Alta |
| JLL | ✅ | ✅ | ? | Alto | Alta |
| Solili | ✅ | ✅ Industrial | ? | Medio? | Alta |
| CoStar | ? | ✅ | ✅ | Muy alto | Media |
| SoftPro | ✅ | ✅ | ? | Medio | Media |
| Datoz | ✅ | ? | ? | ? | Baja |

---

## Información a Obtener de Cada Proveedor

### Preguntas de Evaluación

1. **Cobertura**: ¿Qué zonas de México cubren?
2. **Datos**: ¿Qué tipos de datos ofrecen?
   - Listings activos
   - Precios de mercado
   - Tasas de vacancy
   - Transacciones históricas
   - Datos demográficos
3. **Acceso**: ¿Cómo se accede?
   - API
   - Data feed
   - Dashboard
   - Reportes PDF
4. **Actualización**: ¿Con qué frecuencia se actualizan?
5. **Costo**: Modelo de pricing
   - Suscripción mensual/anual
   - Por query
   - Por descarga
6. **Integración**: ¿Se puede automatizar la integración?
7. **Licencia**: ¿Qué usos están permitidos?

---

## Proceso de Evaluación

### Fase 1: Investigación Inicial

1. [ ] Visitar websites de todos los proveedores
2. [ ] Descargar reportes públicos disponibles
3. [ ] Identificar datos que ofrecen

### Fase 2: Contacto

4. [ ] Contactar CBRE, JLL, Solili (prioritarios)
5. [ ] Solicitar demos o muestras de datos
6. [ ] Obtener cotizaciones

### Fase 3: Evaluación

7. [ ] Comparar cobertura vs. necesidades
8. [ ] Analizar costo-beneficio
9. [ ] Probar integración si hay API

### Fase 4: Decisión

10. [ ] Recomendar proveedores para contratar
11. [ ] Documentar en ADR
12. [ ] Negociar contratos

---

## Consideraciones de Costo-Beneficio

### Build vs Buy

| Opción | Pros | Cons |
|--------|------|------|
| **Scraping propio** | Control total, sin costo recurrente | Mantenimiento, legal, menos datos |
| **Comprar datos** | Datos curados, más completos | Costo recurrente, dependencia |
| **Híbrido** | Balance | Complejidad |

### Recomendación Preliminar

**Híbrido**:
1. Scraping propio para listings de portales públicos
2. Reportes públicos de CBRE/JLL para contexto de mercado
3. Evaluar Solili para datos industriales especializados
4. Usar datos de gobierno (INEGI, etc.) para demográficos

---

## Hallazgos

*(Documentar aquí los resultados de la investigación)*

### CBRE
- Contacto:
- Datos ofrecidos:
- Costo:
- Notas:

### JLL
- Contacto:
- Datos ofrecidos:
- Costo:
- Notas:

### Solili
- Contacto:
- Datos ofrecidos:
- Costo:
- Notas:

---

## Próximos Pasos

1. [ ] Completar investigación de websites (esta semana)
2. [ ] Contactar a CBRE, JLL, Solili (próxima semana)
3. [ ] Obtener cotizaciones (semana 2-3)
4. [ ] Hacer recomendación final
