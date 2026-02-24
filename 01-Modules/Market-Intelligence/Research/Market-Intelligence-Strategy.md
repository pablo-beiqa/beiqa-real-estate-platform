# Estrategia de Inteligencia de Mercado

**Fase**: R-2 (Tecnología y Plataforma)
**Estado**: 🔴 Por investigar
**Depende de**: Cuestionario 04-Inteligencia-de-Mercado (R-0), Competitive Landscape (R-1)

---

## Objetivo

Definir cómo BEIQA sistematiza la inteligencia de mercado que hoy es "gut feeling" del equipo. Transformar conocimiento tácito en datos estructurados, métricas calculadas, y reportes automatizados.

---

## El Problema

Hoy la inteligencia de mercado en Beiqa es:

- **Intuición**: "El precio de bodegas en esta zona está subiendo"
- **Experiencia**: "Este corredor tiene mucha vacancia"
- **Fuentes fragmentadas**: Reportes de CBRE/JLL que se leen pero no se sistematizan
- **Sin historial**: No se trackean cambios de precios, vacancia, o absorción en el tiempo

---

## Componentes de Inteligencia de Mercado

### 1. Precio por m2 por Zona/Tipo

| Aspecto | Investigación Necesaria |
|---------|------------------------|
| Fuente de datos | Precios de oferta de portales (no transacciones reales) |
| Cálculo | Media, mediana, percentiles por zona + tipo |
| Manejo de outliers | 🔴 ¿Cómo detectar y excluir precios anómalos? |
| Zonas | 🔴 ¿AGEBs? ¿Corredores custom? ¿Municipios? |
| Tipos | Industrial, oficinas, retail, terreno |
| Limitación clave | Precios de OFERTA ≠ precios de TRANSACCIÓN |

**Preguntas de investigación:**
- [ ] ¿Cuál es la distribución típica de precios por zona en CDMX?
- [ ] ¿Qué métodos estadísticos son apropiados para datos de precios inmobiliarios?
- [ ] ¿Hay suficientes datos por zona para que las métricas sean significativas?
- [ ] ¿Cómo manejar diferencias entre renta y venta?

### 2. Vacancia y Absorción

| Aspecto | Investigación Necesaria |
|---------|------------------------|
| Definición de vacancia | 🔴 ¿Propiedades disponibles / total inventario? |
| Fuente de datos para inventario total | 🔴 No tenemos total -- solo oferta |
| Absorción | Propiedades que salen del mercado (rentadas/vendidas) en periodo |
| Cómo detectar absorción | 🔴 ¿Listing desaparece = absorbido? Poco confiable |

**Preguntas de investigación:**
- [ ] ¿Podemos calcular vacancia sin conocer el inventario total?
- [ ] ¿Los reportes de CBRE/JLL publican datos de vacancia para CDMX? ¿Podemos usarlos como benchmark?
- [ ] ¿Cómo detectar si un listing fue absorbido vs. simplemente removido?

### 3. Comparables (Comps)

| Aspecto | Investigación Necesaria |
|---------|------------------------|
| Definición de "comparable" | 🔴 ¿Qué campos hacen a dos propiedades comparables? |
| Criterios | Mismo tipo, zona similar, superficie ±20%, precio similar |
| Output | Lista de propiedades comparables con métricas |
| Uso | Justificar precio, negociar, presentar al cliente |

**Preguntas de investigación:**
- [ ] ¿Qué criterios usa el equipo hoy para determinar comparables?
- [ ] ¿Cuántos comps son necesarios para que sea útil? (3? 5? 10?)
- [ ] ¿Se necesitan comps históricos (propiedades ya absorbidas) o solo activas?

### 4. Tendencias de Precio

| Aspecto | Investigación Necesaria |
|---------|------------------------|
| Periodo | 🔴 ¿Mensual? ¿Trimestral? |
| Cálculo | Precio promedio/mediano por periodo por zona/tipo |
| Visualización | Línea de tiempo, comparación entre zonas |
| Limitación | Necesita datos históricos acumulados en el tiempo |

**Preguntas de investigación:**
- [ ] ¿Cuántos meses de datos históricos se necesitan antes de que las tendencias sean útiles?
- [ ] ¿El equipo necesita ver tendencias desde el día 1, o es una funcionalidad que se construye con el tiempo?

### 5. Reportes de Mercado

| Aspecto | Investigación Necesaria |
|---------|------------------------|
| Contenido típico | 🔴 Definir con equipo (parte de cuestionario R-0) |
| Formato de entrega | 🔴 ¿PDF? ¿PPT? ¿Web? |
| Frecuencia | 🔴 ¿Por proyecto? ¿Trimestrales? |
| Automatización | 🔴 ¿Generación automática o asistida? |

**Preguntas de investigación:**
- [ ] ¿Qué contiene un reporte de mercado típico de Beiqa hoy?
- [ ] ¿Qué contiene un reporte de CBRE/JLL que Beiqa quiere igualar o superar?
- [ ] ¿Los clientes valoran más datos crudos o insights/narrativa?

---

## Benchmarking: ¿Qué Hacen los Competidores?

| Empresa | Tipo de Reporte | Métricas Incluidas | Frecuencia | Estado |
|---------|-----------------|-------------------|------------|--------|
| CBRE | MarketView | 🔴 Por investigar | Trimestral | 🔴 |
| JLL | Market Outlook | 🔴 Por investigar | Trimestral | 🔴 |
| Cushman | MarketBeat | 🔴 Por investigar | Trimestral | 🔴 |
| Solili | Market Data | 🔴 Por investigar | Mensual? | 🔴 |

---

## Data Gaps (Lo Que NO Tendremos)

| Dato | Por Qué No lo Tenemos | Impacto | Mitigación |
|------|----------------------|---------|------------|
| Precios reales de transacción | Solo capturamos precios de oferta | Métricas de precio son aproximadas | Indicar siempre "precio de oferta" |
| Inventario total | Solo vemos lo publicado en portales | No podemos calcular vacancia real | Usar datos de terceros (Solili/CBRE) |
| Cap rates | Requiere datos de ingreso + valor de venta | No podemos calcular yield | Diferir a post-MVP |
| Datos históricos (antes de BEIQA) | No tenemos data antes de encender el sistema | Tendencias empiezan desde cero | Ser transparentes con clientes |

---

## Métricas MVP vs. Post-MVP

| Métrica | MVP | Post-MVP | Requiere |
|---------|-----|----------|----------|
| Precio promedio/m2 por zona/tipo | ✅ | | Datos de portales |
| Distribución de precios | ✅ | | Datos de portales |
| Conteo de propiedades por zona/tipo | ✅ | | Datos de portales |
| Nuevos listings esta semana | ✅ | | Tracking en el tiempo |
| Tendencias de precio | | ✅ | Meses de datos históricos |
| Vacancia estimada | | ✅ | Inventario total (externo) |
| Absorción | | ✅ | Tracking de listings removidos |
| Comparables automáticos | ✅ (básico) | ✅ (avanzado) | Algoritmo de matching |
| Reportes PDF | | ✅ | Template engine |

---

## Acciones de Investigación

1. [ ] Definir métricas con el equipo (parte de cuestionarios R-0)
2. [ ] Obtener y analizar reportes públicos de CBRE/JLL/Cushman México
3. [ ] Definir zonas/submercados para agregación
4. [ ] Evaluar métodos estadísticos para datos de precios
5. [ ] Diseñar schema de tablas para métricas de mercado
6. [ ] Investigar si Solili ofrece datos de vacancia/absorción para complementar
7. [ ] Crear mockup de reporte de mercado objetivo
8. [ ] Documentar decisiones
