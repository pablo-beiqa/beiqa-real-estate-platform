# MEMORY.md — Memoria Persistente del Proyecto

> Se actualiza al cierre de cada sesión (`/session-close`, paso 7).
> Solo decisiones y contexto que afectan cómo trabajar en la siguiente sesión.

## Decisiones recientes

- **2026-03-11**: CLAUDE.md comprimido de 303→104 líneas. Ya no contiene tabla de stack, tabla de módulos, ni count de ADRs. Para esos datos consultar los archivos fuente directamente.
- **2026-03-11**: AI Brain reestructurado a 7 agentes en 3 tiers (Data Pipeline, Client Intelligence, Intelligence & Analysis). Score Discovery Agent y Property Search & Match Agent son nuevos.
- **2026-03-05**: Mastra adoptado como framework de AI agents (ADR-020). Trigger.dev solo hace ejecución durable (ADR-021). Backboard.io deprecado.
- **2026-03-05**: Roadmap migrado de fases a sprints de 2 semanas con Q/Sprint hierarchy. 13 milestones outcome-based.
- **2026-03-05**: Hosting decidido: Vercel Pro (frontend, $20/mo) + Mastra Cloud beta (agents, gratis). ADR-022.

## Aprendizajes clave

- Las secciones de proceso/burocracia en CLAUDE.md (workflow de tareas, protocolo de cierre) no funcionan — Claude no las sigue. Las instrucciones conductuales directas ("cuestiona antes de ejecutar", "conecta con negocio") sí moldean comportamiento.
- Datos que cambian frecuentemente (status de módulos, stack table, ADR count) no deben vivir en CLAUDE.md — generan mantenimiento imposible y desync. Mejor apuntar al archivo fuente.
