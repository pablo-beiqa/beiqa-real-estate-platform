# Catálogo de Criterios de Scoring

> **Status**: ✅ Documentado (primera versión)
> **Source**: Entrevista con Pablo, CEO — 2026-03-11
> **Uso**: El Score Discovery Agent mapea requerimientos del cliente a estos criterios

---

## Overview

- 160+ criterios organizados en 7 grupos (A–G)
- Grupos A, F, G siempre activos para cualquier cliente
- Grupos B–E se activan según tipo de propiedad del cliente
- Cada cliente recibe un subconjunto personalizado — NO todos los criterios aplican
- Los criterios evolucionan con cada cliente; nuevos pueden agregarse

---

## Cómo se activan los grupos

| Tipo de propiedad del cliente | Grupos activos |
|-------------------------------|---------------|
| Industrial / Bodega / Manufactura / Logística / CEDIS | A + **B** + F + G |
| Comercial / Restaurante / Cafetería / Retail / Bar | A + **C** + F + G |
| Oficinas / Coworking | A + **D** + F + G |
| Terreno / BTS | A + **E** + F + G |
| Mixto (ej: bodega con oficina) | A + **B** + **D** + F + G (se combinan) |

---

## Grupo A — Universal (siempre aplica) ~30 criterios

Criterios que se extraen para **todo tipo de cliente y propiedad**, sin excepción.

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `superficie_total_m2` | numeric | Superficie total en metros cuadrados |
| `superficie_rentable_m2` | numeric | Superficie rentable/útil en m² |
| `precio_renta_mensual_mxn` | numeric | Precio de renta mensual en MXN |
| `precio_m2_renta_mxn` | numeric | Precio por m² de renta en MXN |
| `precio_venta_total_mxn` | numeric | Precio de venta total en MXN |
| `precio_m2_venta_mxn` | numeric | Precio por m² de venta en MXN |
| `operacion` | enum: `renta` \| `venta` \| `renta-con-opcion-compra` | Tipo de operación |
| `tipo_propiedad` | enum: `industrial` \| `comercial` \| `oficinas` \| `terreno` \| `mixto` | Tipo de inmueble |
| `subtipo` | enum: `bodega` \| `local` \| `nave` \| `piso-oficinas` \| `planta-manufactura` \| `strip-center` \| `CEDIS` \| `local-plaza` \| `restaurante` \| `cafeteria` \| `bar` \| `coworking` \| `otros` | Subtipo específico |
| `uso_de_suelo` | text | Uso de suelo requerido (restaurantero, industrial, habitacional-mixto, servicios, etc.) |
| `estado_construccion` | enum: `obra-gris` \| `semi-acabado` \| `terminado` \| `remodelado` | Estado de la construcción |
| `antiguedad_anos` | numeric | Antigüedad del inmueble en años |
| `zona_objetivo` | array\<text\> | Lista de alcaldías, municipios, corredores, ciudades objetivo |
| `zonas_excluidas` | array\<text\> | Zonas que el cliente no quiere |
| `disponibilidad_fecha` | date | Fecha de disponibilidad requerida |
| `plazo_contrato_anos` | numeric | Plazo de contrato deseado en años |
| `plazo_minimo_anos` | numeric | Plazo mínimo aceptable |
| `meses_gracia_ideales` | numeric | Meses de gracia deseados |
| `meses_gracia_minimos` | numeric | Meses de gracia mínimos aceptables |
| `deposito_garantia_meses` | numeric | Depósito de garantía en meses de renta |
| `ajuste_anual_tipo` | enum: `fijo-pct` \| `inflacion-inpc` \| `UDI` \| `negociable` | Tipo de ajuste anual |
| `ajuste_anual_pct_max` | numeric | Porcentaje máximo de ajuste anual |
| `traspaso_acepta` | boolean | ¿Acepta pagar traspaso? |
| `traspaso_max_mxn` | numeric | Monto máximo de traspaso en MXN |
| `clausula_salida_anticipada` | boolean | ¿Requiere cláusula de salida anticipada? |
| `penalidad_salida_anticipada` | text | Penalidad aceptable por salida anticipada |
| `subarrendamiento_permitido` | boolean | ¿Requiere que el subarrendamiento sea permitido? |
| `adecuaciones_arrendador` | text | Contribución del arrendador en adecuaciones (MXN o meses) |
| `opcion_renovacion` | boolean | ¿Requiere opción de renovación? |
| `opcion_expansion` | boolean | ¿Requiere opción de expansión? |

---

## Grupo B — Industrial / Bodega / Manufactura / Logística / CEDIS ~35 criterios

Activar si el cliente opera bodega, manufactura, distribución, logística, CEDIS, almacén.

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `altura_libre_techo_m` | numeric | Altura libre de techo en metros |
| `resistencia_piso_ton_m2` | numeric | Resistencia del piso en toneladas/m² |
| `andenes_carga_cantidad` | integer | Cantidad de andenes de carga |
| `altura_anden_m` | numeric | Altura de andén en metros |
| `area_maniobras_m` | numeric | Radio de área de maniobras para tráileres |
| `acceso_trailer` | boolean | ¿Requiere acceso para tráiler? |
| `altura_acceso_vehicular_m` | numeric | Altura de acceso vehicular en metros |
| `patio_maniobras_m2` | numeric | Área de patio de maniobras en m² |
| `area_patio_exterior_m2` | numeric | Área de patio exterior en m² |
| `energia_electrica_kva` | numeric | Capacidad eléctrica en KVA |
| `voltaje_disponible` | text | Voltaje disponible |
| `subestacion_propia` | boolean | ¿Requiere subestación eléctrica propia? |
| `generador_respaldo` | boolean | ¿Requiere generador de respaldo? |
| `gas_industrial_tipo` | enum: `entubado` \| `tanque-LP` \| `gas-natural` \| `no-requiere` | Tipo de gas industrial |
| `cisterna_m3` | numeric | Capacidad de cisterna en m³ |
| `oficinas_dentro_m2` | numeric | Superficie de oficinas dentro de la nave en m² |
| `area_administrativa_m2` | numeric | Área administrativa en m² |
| `vestidores_regaderas` | integer | Cantidad de vestidores/regaderas |
| `estacionamiento_cajones` | integer | Cajones de estacionamiento |
| `estacionamiento_trailers` | integer | Cajones para tráileres |
| `acceso_ferroviario` | boolean | ¿Requiere acceso ferroviario? |
| `sprinklers_sistema_contra_incendio` | boolean | ¿Requiere sistema contra incendio/sprinklers? |
| `vigilancia_24h` | boolean | ¿Requiere vigilancia 24h? |
| `barda_perimetral` | boolean | ¿Requiere barda perimetral? |
| `certificacion_leed` | boolean | ¿Requiere certificación LEED? |
| `parque_industrial_nombre` | text | Nombre del parque industrial preferido |
| `distancia_autopista_km` | numeric | Distancia máxima a autopista en km |
| `distancia_aeropuerto_km` | numeric | Distancia máxima a aeropuerto en km |
| `distancia_puerto_aduana_km` | numeric | Distancia máxima a puerto/aduana en km |
| `distancia_cdmx_km` | numeric | Distancia máxima a CDMX en km |
| `zona_industrial_corredor` | text | Corredor industrial preferido (Tlalnepantla, Cuautitlán, Querétaro, etc.) |
| `cargadores_montacargas` | integer | Cantidad de cargadores para montacargas |
| `numero_montacargas_simultaneos` | integer | Montacargas que operan simultáneamente |
| `area_cuarentena_devoluciones` | numeric | Área de cuarentena/devoluciones en m² |
| `zona_materiales_peligrosos` | boolean + text | ¿Zona para materiales peligrosos? + tipo |
| `camaras_frigorificas` | boolean | ¿Requiere cámaras frigoríficas? |
| `temperatura_operacion_celsius` | numeric | Temperatura de operación en °C |

---

## Grupo C — Comercial / Restaurante / Cafetería / Retail / Bar / Local ~30 criterios

Activar si el cliente opera local comercial, restaurante, cafetería, bar, tienda, gym, clínica, etc.

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `planta_baja` | boolean | ¿Requiere planta baja? (discriminante crítico) |
| `acceso_independiente_calle` | boolean | ¿Requiere acceso independiente desde la calle? |
| `acceso_interior_plaza` | boolean | ¿Acceso desde interior de plaza comercial? |
| `frente_lineal_m` | numeric | Frente lineal mínimo en metros |
| `visibilidad_desde_banqueta` | boolean | ¿Requiere visibilidad desde banqueta? |
| `altura_espacio_m` | numeric | Altura del espacio en metros |
| `letrero_exterior_posible` | boolean | ¿Es posible colocar letrero exterior? |
| `letrero_luminoso` | boolean | ¿Letrero luminoso? |
| `uso_suelo_restaurantero` | boolean | ¿Uso de suelo permite restaurante? |
| `uso_suelo_alcoholes` | boolean | ¿Uso de suelo permite venta de alcohol? |
| `tiro_extraccion_disponible` | boolean | ¿Tiene tiro de extracción disponible? |
| `tiro_extraccion_instalable` | boolean | ¿Se puede instalar tiro de extracción? |
| `gas_tipo` | enum: `entubado` \| `tanque-exterior` \| `no-requiere` | Tipo de gas |
| `cocina_instalada` | boolean | ¿Tiene cocina instalada? |
| `equipos_incluidos` | array\<text\> | Lista de equipos incluidos |
| `banos_cantidad` | integer | Cantidad de baños |
| `terraza_exterior_posible` | boolean | ¿Es posible tener terraza exterior? |
| `terraza_m2` | numeric | Superficie de terraza en m² |
| `mesas_banqueta_permitidas` | boolean | ¿Se permiten mesas en banqueta? |
| `estacionamiento_propio_cajones` | integer | Cajones de estacionamiento propio |
| `valet_parking_cercano` | boolean | ¿Hay valet parking cercano? |
| `acceso_discapacidades` | boolean | ¿Tiene acceso para personas con discapacidad? |
| `ubicacion_tipo` | enum: `standalone` \| `strip-center` \| `plaza-comercial` \| `interior-edificio` \| `mercado` | Tipo de ubicación |
| `plaza_nombre` | text | Nombre de la plaza (si aplica) |
| `horario_operacion_max` | text | Restricciones de horario del local |
| `permiso_alcohol_tipo` | enum: `capacidad` \| `venta-mesa` \| `consumo` \| `terraza` \| `ninguno` | Tipo de permiso de alcohol |
| `distancia_competencia_directa_m` | numeric | Distancia mínima a competencia directa (no-canibalización) |
| `distancia_locales_propios_min_m` | numeric | Distancia mínima a locales existentes del cliente |
| `flujo_peatonal_estimado` | enum: `muy-bajo` \| `bajo` \| `medio` \| `alto` \| `muy-alto` | Flujo peatonal estimado |
| `perfil_socioeconomico_zona` | enum: `A/B` \| `C+` \| `C` \| `C-` \| `D+` | Perfil socioeconómico de la zona |
| `densidad_oficinas_radio_500m` | enum: `baja` \| `media` \| `alta` | Densidad de oficinas en radio de 500m |
| `densidad_residencial_radio_500m` | enum: `baja` \| `media` \| `alta` | Densidad residencial en radio de 500m |
| `densidad_gimnasios_fitness_radio_500m` | enum: `baja` \| `media` \| `alta` | Densidad de gimnasios/fitness en radio de 500m |
| `locales_existentes_cliente` | array\<text\> | Lista de ubicaciones actuales del cliente |

---

## Grupo D — Oficinas / Coworking ~20 criterios

Activar si el cliente busca espacio de oficinas, coworking, consultorio, o piso corporativo.

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `piso_numero` | integer | Número de piso |
| `total_pisos_edificio` | integer | Total de pisos del edificio |
| `clasificacion_edificio` | enum: `A+` \| `A` \| `B` \| `C` | Clasificación del edificio |
| `layout_tipo` | enum: `open-space` \| `semi-abierto` \| `divisiones-fijas` \| `mixto` \| `planta-libre` | Tipo de layout |
| `eficiencia_piso_pct` | numeric | Eficiencia del piso (área neta / área bruta) |
| `salas_juntas_cantidad` | integer | Cantidad de salas de juntas |
| `sala_juntas_m2_max` | numeric | Superficie de la sala más grande |
| `recepcion_compartida` | boolean | ¿Recepción compartida del edificio? |
| `estacionamiento_ratio` | numeric | Ratio de cajones por 100 m² |
| `acceso_metro_minutos` | integer | Tiempo caminando al metro más cercano |
| `lineas_metro_cercanas` | array\<text\> | Líneas de metro cercanas |
| `acceso_transporte_publico` | boolean | ¿Acceso a transporte público? |
| `rutas_autobus` | array\<text\> | Rutas de autobús cercanas |
| `amenidades_edificio` | array\<text\> | Amenidades (comedor, gym, terraza, lockers, lavandería, sanitarios) |
| `generador_electrico` | boolean | ¿Tiene generador eléctrico? |
| `ups_regulador` | boolean | ¿Tiene UPS/regulador? |
| `aire_acondicionado_tipo` | enum: `central` \| `mini-splits` \| `VRV` \| `fan-coil` \| `chiller` | Tipo de aire acondicionado |
| `certificacion_leed_nivel` | enum: `platino` \| `oro` \| `plata` \| `certificado` \| `ninguno` | Nivel de certificación LEED |
| `control_acceso_24h` | boolean | ¿Control de acceso 24h? |
| `seguridad_vigilancia` | boolean | ¿Seguridad/vigilancia? |
| `fibra_optica_dedicada` | boolean | ¿Fibra óptica dedicada? |
| `datacenter_cuarto_servidores` | boolean | ¿Cuarto de servidores/datacenter? |
| `columnas_modulacion_m` | numeric | Modulación entre columnas en metros |
| `altura_entrepiso_m` | numeric | Altura de entrepiso en metros |

---

## Grupo E — Terreno ~15 criterios

Activar si el cliente busca terreno para desarrollo, construcción a la medida (BTS) o expansión.

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `superficie_terreno_m2` | numeric | Superficie del terreno en m² |
| `frente_lineal_m` | numeric | Frente lineal del terreno en metros |
| `fondo_m` | numeric | Fondo del terreno en metros |
| `forma_regular` | boolean | ¿El terreno es de forma regular? |
| `topografia` | enum: `plano` \| `leve-desnivel` \| `accidentado` | Topografía del terreno |
| `uso_suelo_actual` | text | Uso de suelo actual |
| `uso_suelo_potencial` | text | Uso de suelo potencial |
| `cus` | numeric | Coeficiente de Utilización del Suelo |
| `cos` | numeric | Coeficiente de Ocupación del Suelo |
| `numero_niveles_permitidos` | integer | Número de niveles permitidos |
| `servicios_disponibles` | array\<text\> | Servicios disponibles (agua, luz, drenaje, gas, telecomunicaciones) |
| `escritura_libre_gravamen` | boolean | ¿Escritura libre de gravamen? |
| `regimen_propiedad` | enum: `privado` \| `ejidal` \| `federal` | Régimen de propiedad |
| `tiempo_regularizacion_meses` | integer | Tiempo de regularización en meses |
| `acceso_carretero` | boolean | ¿Tiene acceso carretero? |
| `pavimentacion_frente` | boolean | ¿Tiene pavimentación al frente? |
| `posibilidad_subdivision` | boolean | ¿Es posible subdividir? |
| `build_to_suit` | boolean | ¿Se busca construir a la medida (BTS)? |

---

## Grupo F — Operacional del Cliente (siempre extraer) ~15 criterios

Criterios sobre la **operación del cliente**, no sobre la propiedad. Siempre se deben extraer.

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `empleados_totales` | integer | Total de empleados |
| `empleados_por_turno` | integer | Empleados por turno |
| `numero_turnos` | integer | Número de turnos |
| `horario_operacion` | text | Horario de operación |
| `vehiculos_propios_cantidad` | integer | Cantidad de vehículos propios |
| `tipo_vehiculos` | array\<text\> | Tipos (autos, camionetas, camiones-3.5t, tráileres, montacargas, refrigerados) |
| `clientes_presenciales_dia` | integer | Clientes presenciales por día (si aplica) |
| `tipo_clientes` | text | Tipo de clientes (B2B, B2C, corporativo, consumidor final) |
| `proveedores_dia` | integer | Proveedores que visitan por día |
| `frecuencia_reabastecimiento` | text | Frecuencia de reabastecimiento |
| `rutas_distribucion_servir` | array\<text\> | Rutas de distribución a servir |
| `ciudades_servir` | array\<text\> | Ciudades a servir |
| `plan_expansion_locales` | text | Plan de expansión (cuántos locales y en qué tiempo) |
| `numero_locales_actual` | integer | Número de locales actuales |
| `facturacion_mensual_aprox` | numeric | Facturación mensual aproximada |
| `estacionalidad` | text | Temporadas altas/bajas |
| `certificaciones_requeridas` | array\<text\> | Certificaciones (ISO-9001, ISO-14001, HACCP, BRC, FDA, FSMA, CTPAT, otros) |

---

## Grupo G — Negociación y Contexto (siempre extraer) ~15 criterios

Criterios sobre el **contexto de negociación**. Siempre se deben extraer.

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `budget_total_adecuaciones_mxn` | numeric | Budget total para adecuaciones en MXN |
| `budget_mensual_max_mxn` | numeric | Budget mensual máximo en MXN |
| `tiene_broker_actual` | boolean | ¿Tiene broker actual? |
| `broker_nombre` | text | Nombre del broker actual |
| `deadline_ocupacion` | date | Fecha límite de ocupación |
| `razon_urgencia` | text | Razón de la urgencia |
| `esta_en_mora_actual` | boolean | ¿Está en mora con su arrendador actual? |
| `conflicto_arrendador_actual` | boolean | ¿Tiene conflicto con arrendador actual? |
| `presion_externa` | enum: `vencimiento-contrato` \| `expansion-urgente` \| `relocation` \| `cierre-operaciones` | Tipo de presión externa |
| `decision_maker` | text | Nombre y cargo de quien decide |
| `aprobadores_adicionales` | array\<text\> | Lista de aprobadores adicionales |
| `proceso_aprobacion_interno` | enum: `directo` \| `comité` \| `consejo` | Proceso de aprobación interno |

---

## ScoringDocument Schema (propuesto)

Interface TypeScript propuesta para el documento estructurado que el Score Discovery Agent produce por cada cliente.

```typescript
// ============================================================
// ScoringDocument — output del Score Discovery Agent
// ============================================================

interface ScoringDocument {
  client: {
    tenant_id: string;           // ID del tenant en Supabase
    company_name: string;
    contact_name?: string;
    decision_maker?: string;     // Nombre y cargo de quien decide
  };

  property_type: 'industrial' | 'comercial' | 'oficinas' | 'terreno' | 'mixto';
  operation_type: 'renta' | 'venta' | 'renta-con-opcion-compra';

  /** Grupos de criterios activos para este cliente (A, F, G siempre; B-E según tipo) */
  active_groups: Array<'A' | 'B' | 'C' | 'D' | 'E' | 'F' | 'G'>;

  /** Criterios organizados por grupo */
  criteria: {
    A: ScoringCriterion[];       // Universal — siempre presente
    B?: ScoringCriterion[];      // Industrial (opcional)
    C?: ScoringCriterion[];      // Comercial (opcional)
    D?: ScoringCriterion[];      // Oficinas (opcional)
    E?: ScoringCriterion[];      // Terreno (opcional)
    F: ScoringCriterion[];       // Operacional — siempre presente
    G: ScoringCriterion[];       // Negociación — siempre presente
  };

  /** Zonas geográficas */
  zones: {
    target: string[];            // Alcaldías, municipios, corredores, ciudades objetivo
    excluded: string[];          // Zonas que el cliente NO quiere
  };

  /** Condiciones comerciales extraídas del discovery */
  commercial_conditions: {
    plazo_contrato_anos?: number;
    meses_gracia_ideales?: number;
    deposito_garantia_meses?: number;
    ajuste_anual_tipo?: 'fijo-pct' | 'inflacion-inpc' | 'UDI' | 'negociable';
    budget_mensual_max_mxn?: number;
    budget_total_adecuaciones_mxn?: number;
  };

  /** Criterios sin valor — el equipo debe llenarlos */
  gaps: ScoringGap[];

  /** Metadatos del documento */
  metadata: {
    scoring_id: string;          // UUID
    status: 'draft' | 'approved';
    created_at: string;          // ISO 8601
    approved_at?: string;        // ISO 8601, null si status = draft
    transcript_ids: string[];    // IDs de transcripciones de donde se extrajeron los criterios
    version: number;
  };
}

interface ScoringCriterion {
  name: string;                  // Nombre del campo (ej: "superficie_total_m2")
  value: string | number | boolean | string[] | null;
  weight: number;                // 0–100, importancia para este cliente
  is_required: boolean;          // true = deal-breaker, false = nice-to-have
  source: 'transcript' | 'manual' | 'inferred';
}

interface ScoringGap {
  name: string;                  // Nombre del campo faltante
  group: 'A' | 'B' | 'C' | 'D' | 'E' | 'F' | 'G';
  reason: string;                // Por qué no se pudo extraer
  suggested_question?: string;   // Pregunta para que el broker llene el gap
}
```

---

## Notas

- Este catálogo es un **documento vivo** que crecerá con cada nuevo tipo de cliente
- No todos los criterios están implementados en el schema de base de datos todavía — el golden record necesita evolucionar para soportarlos
- El Score Discovery Agent usa este catálogo como referencia para extraer y mapear criterios desde transcripciones
