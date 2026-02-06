# Estrategia de Adquisición de Datos

**Fase**: R-1 (Datos Core)
**Estado**: 🔴 Por investigar
**Depende de**: Cuestionarios de producto (R-0)

---

## Objetivo

Evaluar TODOS los métodos de adquisición de datos de propiedades y determinar la estrategia óptima para BEIQA. La arquitectura completa depende de esta decisión.

> **Corrección crítica**: La [Arquitectura del Sistema](../../04-Architecture/System-Architecture.md) asume scraping automatizado. Esto NO está validado. Podría ser API, extensión de Chrome, importación manual, o un híbrido.

---

## Métodos de Adquisición a Evaluar

### 1. API Pública (Cuando Disponible)

| Criterio | Evaluación |
|----------|------------|
| Descripción | Consumir datos via API REST oficial del portal |
| Portales con API conocida | EasyBroker (confirmado) |
| Pros | Legal, estable, estructurado, documentado |
| Contras | Pocos portales la ofrecen, posibles límites |
| Complejidad técnica | Baja |
| Mantenimiento | Bajo |
| Skill requerido del operador | Ninguno (automatizado) |

**Preguntas por investigar:**
- [ ] ¿Qué otros portales tienen API pública o privada?
- [ ] ¿APIs de portales internacionales tienen datos de México?

### 2. Scraping Automatizado (Headless Browser / HTTP)

| Criterio | Evaluación |
|----------|------------|
| Descripción | Bot que extrae datos de páginas web automáticamente |
| Tecnologías | Scrapy, Playwright, Puppeteer, BeautifulSoup, Selenium, Crawlee |
| Pros | Alto volumen, automatizado, configurable |
| Contras | Frágil ante cambios de HTML, legal gris, anti-bot |
| Complejidad técnica | Media-Alta |
| Mantenimiento | Alto (se rompe cuando el portal cambia) |
| Skill requerido del operador | Ninguno (automatizado) |

**Preguntas por investigar:**
- [ ] ¿Qué portales tienen protección anti-bot?
- [ ] ¿Cuál es el riesgo legal real en México?
- [ ] ¿Con qué frecuencia cambian los portales su HTML?

### 3. Extensión de Chrome (Captura Asistida)

| Criterio | Evaluación |
|----------|------------|
| Descripción | El usuario navega el portal normalmente y con un clic captura la propiedad |
| Tecnologías | Chrome Extension API, content scripts |
| Pros | Legal (usuario real), flexible, funciona en cualquier portal |
| Contras | Requiere intervención manual, no escalable para alto volumen |
| Complejidad técnica | Media |
| Mantenimiento | Medio (menos frágil que scraping completo) |
| Skill requerido del operador | Bajo (navegar y hacer clic) |

**Preguntas por investigar:**
- [ ] ¿El equipo usaría esto regularmente?
- [ ] ¿Cuántas propiedades se podrían capturar por hora?
- [ ] ¿Existe alguna extensión similar ya hecha?

### 4. Importación Manual (CSV/Excel)

| Criterio | Evaluación |
|----------|------------|
| Descripción | El equipo exporta datos de portales y los sube a BEIQA |
| Pros | Simple, sin riesgo legal, funciona siempre |
| Contras | Lento, propenso a errores, no escalable |
| Complejidad técnica | Baja |
| Mantenimiento | Bajo |
| Skill requerido del operador | Medio (preparar el CSV) |

### 5. RPA (Robotic Process Automation)

| Criterio | Evaluación |
|----------|------------|
| Descripción | Robot que replica acciones humanas en el navegador |
| Tecnologías | UiPath, Automation Anywhere, Python + pyautogui |
| Pros | Funciona con cualquier sitio, simula usuario real |
| Contras | Frágil, lento, costoso (herramientas enterprise) |
| Complejidad técnica | Media |
| Mantenimiento | Alto |

### 6. Enfoque Híbrido (Recomendación Probable)

| Criterio | Evaluación |
|----------|------------|
| Descripción | Combinar métodos según el portal |
| Ejemplo | API para EasyBroker + Extensión Chrome para otros + CSV como fallback |
| Pros | Pragmático, maximiza cobertura, minimiza riesgo |
| Contras | Más componentes que mantener |

---

## Análisis Detallado: EasyBroker

> EasyBroker es el portal prioritario para el MVP. Tiene API pública confirmada.

### API Pública de EasyBroker

| Campo | Hallazgo |
|-------|----------|
| Documentación | 🔴 Por localizar URL exacta |
| Tipo | REST API |
| Autenticación | 🔴 Por investigar (API key?) |
| Rate limits | 🔴 Por investigar |
| Endpoints relevantes | 🔴 Por investigar |
| Campos disponibles | 🔴 Por investigar |
| ¿Incluye coordenadas? | 🔴 Por verificar |
| ¿Incluye imágenes? | 🔴 Por verificar (URLs?) |
| ¿Permite filtrar por CDMX? | 🔴 Por verificar |
| ¿Permite filtrar por tipo comercial/industrial? | 🔴 Por verificar |
| Volumen estimado CDMX comercial/industrial | 🔴 Por contar |

### Acciones de Investigación para EasyBroker

1. [ ] Localizar documentación oficial de la API
2. [ ] Obtener API key de prueba
3. [ ] Listar todos los endpoints disponibles
4. [ ] Hacer request de prueba y documentar la estructura de respuesta
5. [ ] Contar propiedades comerciales/industriales en CDMX
6. [ ] Verificar rate limits y restricciones
7. [ ] Evaluar si la API cubre suficientes datos para el MVP

### EasyBroker: Si la API es insuficiente

| Campo | Hallazgo |
|-------|----------|
| ¿Sitio estático o dinámico (JS)? | 🔴 Por analizar |
| Protección anti-bot | 🔴 Por verificar |
| robots.txt | 🔴 Por revisar |
| Términos de servicio sobre scraping | 🔴 Por revisar |

---

## Matriz de Decisión por Portal

| Portal | Método Recomendado | Justificación | Estado |
|--------|-------------------|---------------|--------|
| EasyBroker | API (si suficiente) | API pública confirmada | 🔴 Por validar |
| Inmuebles24 | 🔴 Por determinar | Sin API conocida | 🔴 |
| Vivanuncios | 🔴 Por determinar | | 🔴 |
| Lamudi | 🔴 Por determinar | | 🔴 |
| Solili | 🔴 Por determinar | Posiblemente suscripción | 🔴 |
| Propiedades.com | 🔴 Por determinar | | 🔴 |
| Metros Cúbicos | 🔴 Por determinar | | 🔴 |
| LoopNet MX | 🔴 Por determinar | | 🔴 |

---

## Tecnologías de Scraping (Si Aplica)

| Herramienta | Tipo | Mejor para | Lenguaje | Curva de aprendizaje |
|-------------|------|------------|----------|---------------------|
| Scrapy | Framework completo | Alto volumen, sitios estáticos | Python | Media |
| Playwright | Browser automation | Sitios dinámicos (JS-heavy) | Python/Node | Media |
| Puppeteer | Browser automation | Sitios dinámicos | Node.js | Media |
| BeautifulSoup | Parser HTML | HTML estático simple | Python | Baja |
| Selenium | Browser automation | Sitios complejos, legacy | Python/Java | Baja |
| Crawlee | Framework moderno | Full-featured, anti-detection | Node.js | Media |
| requests-html | Hybrid | JS rendering simple | Python | Baja |

**Preguntas por investigar:**
- [ ] ¿Qué herramienta tiene mejor soporte para sitios mexicanos?
- [ ] ¿Qué herramienta se integra mejor con el stack probable (Python/Node)?

---

## Consideraciones Legales

### Marco Legal en México

- [ ] ¿Ley Federal de Protección de Datos Personales (LFPDPPP) aplica a datos de listings públicos?
- [ ] ¿Los términos de servicio de cada portal prohíben scraping explícitamente?
- [ ] ¿Hay precedentes legales en México sobre web scraping?
- [ ] ¿Existe distinción legal entre scraping para uso interno vs. reventa?

### Mitigaciones

- Respetar robots.txt
- Rate limiting (no sobrecargar servidores)
- No almacenar datos personales de terceros (contactos de agentes)
- Uso interno, no reventa de datos
- API primero, scraping solo como fallback

---

## Decisión (Por Tomar)

### Estrategia Recomendada para MVP

🔴 _Por definir después de la investigación_

### Estrategia para Post-MVP

🔴 _Por definir después de la investigación_

---

## Acciones de Investigación Generales

1. [ ] Investigar API de EasyBroker en detalle (PRIORIDAD MÁXIMA)
2. [ ] Verificar APIs de otros portales
3. [ ] Evaluar extensiones de Chrome existentes para captura de datos
4. [ ] Revisar términos de servicio de cada portal
5. [ ] Estimar volumen de propiedades por portal en CDMX
6. [ ] Crear PoC con el método más prometedor
7. [ ] Documentar decisión y justificación
