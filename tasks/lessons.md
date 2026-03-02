# Lecciones Aprendidas

> Este archivo es el **loop de auto-mejora** del equipo con Claude. Después de cada corrección,
> Claude agrega la lección aquí para que el error no se repita.
>
> **Regla**: Al inicio de cada sesión, Claude lee este archivo completo.
> **Formato**: Cada lección tiene fecha, contexto y la regla concreta.

---

## Cómo funciona

1. El usuario corrige a Claude (error, ajuste de tono, decisión incorrecta, etc.)
2. Claude propone una lección con fecha y categoría
3. El usuario aprueba o ajusta la lección
4. Claude la agrega aquí bajo la categoría correspondiente
5. En sesiones futuras, Claude consulta estas lecciones antes de actuar

---

## Lecciones

### Documentación

#### 2026-03-02 — El scoring dashboard es Tenant Portal, no Internal App
**Contexto**: El módulo Tenant Portal estaba documentado como "Fase 4+ Post-MVP" y sin owner. En realidad, el scoring dashboard implementado en beiqa-frontend ES el Tenant Portal (client-facing), no la Internal App (team-facing). La Internal App (gestión de operaciones, data, GIS) aún no existe como repo.
**Regla**: Antes de documentar un módulo, verificar en qué repo vive la implementación y a quién está orientado (equipo interno vs cliente). La distinción Internal App vs Tenant Portal es fundamentalmente sobre la **audiencia**: equipo Beiqa vs clientes corporativos.

### Arquitectura y stack

_Aún no hay lecciones en esta categoría._

### Proceso y workflow

_Aún no hay lecciones en esta categoría._

### Comunicación y tono

_Aún no hay lecciones en esta categoría._

---

*Última actualización: 2026-03-02*
