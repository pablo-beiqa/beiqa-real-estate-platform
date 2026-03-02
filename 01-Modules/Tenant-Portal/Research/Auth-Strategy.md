# Estrategia de Autenticacion — Tenant Portal

**Estado**: ✅ Decidido
**Fecha**: 2026-03-02
**Decision**: Magic link (primario) + password (fallback) via Supabase Auth

---

## Contexto

El Tenant Portal necesita autenticacion para clientes corporativos de Beiqa (5-15 activos simultaneamente). Los requisitos son:

- **Baja friccion**: Los clientes son ejecutivos ocupados, no deben batallar para entrar
- **Aislamiento estricto**: Cada cliente solo ve sus propios datos (scorings, shortlists, feedback)
- **No registro complejo**: El equipo Beiqa crea las cuentas; el cliente solo entra
- **Compatible con cualquier email**: Gmail, Outlook corporativo, dominios propios

---

## Decision

### Magic Link (metodo primario)

El cliente recibe un email con un link unico que lo autentica automaticamente. Sin password, sin registro.

**Implementacion**: `supabase.auth.signInWithOtp({ email })`

**Flujo**:
1. Cliente ingresa su email en la pagina de login
2. Supabase envia magic link al email
3. Cliente hace click en el link → entra directo al portal
4. Sesion persistente (no necesita repetir cada vez)

**Ventajas**:
- 0 friccion: solo necesita acceso a su email
- No hay passwords que olvidar o gestionar
- Cada link es unico y expira en 1 hora

**Limitaciones**:
- Requiere acceso al email en el momento del login
- Algunos firewalls corporativos pueden retrasar la entrega del email
- Usuarios no tecnicos pueden no entender el concepto

### Password (metodo secundario / fallback)

Para clientes que prefieran credenciales tradicionales o tengan problemas con magic links.

**Implementacion**: `supabase.auth.signInWithPassword({ email, password })`

**Flujo**:
1. Equipo Beiqa crea cuenta del cliente en Supabase Auth
2. Se comparte email + password temporal
3. Cliente entra con credenciales y cambia password (opcional)

---

## Aislamiento por Cliente (RLS)

Row Level Security en Supabase garantiza que cada cliente solo accede a sus propios datos.

**Patron de politica RLS**:
```sql
-- Ejemplo para tabla shortlists
CREATE POLICY "clients_see_own_shortlists" ON shortlists
  FOR SELECT
  USING (auth.uid() = client_id);

-- Ejemplo para tabla feedback
CREATE POLICY "clients_manage_own_feedback" ON feedback
  FOR ALL
  USING (auth.uid() = client_id);
```

**Tablas que necesitan RLS** (cuando se creen):
- `shortlists` → Solo ver las de su client_id
- `shortlist_items` → Solo las propiedades de sus shortlists
- `feedback` → Solo leer/escribir su propio feedback
- Scorings → Se leen del filesystem por ahora, pero cuando migren a DB necesitaran RLS

---

## Infraestructura Existente

El Supabase SSR client ya esta configurado en `beiqa-frontend`:

- **Browser client**: `src/lib/supabase.ts` → `createClient()` con anon key
- **Server client**: `src/lib/supabase.ts` → `createServerClient()` con service role key
- **Dependencias**: `@supabase/ssr` (0.8.0) + `@supabase/supabase-js` (2.98.0)

Lo que falta implementar:
- [ ] Pagina de login con formulario de email + opcion de password
- [ ] Middleware de Next.js para proteger rutas (`middleware.ts`)
- [ ] Redirect a login si no hay sesion activa
- [ ] RLS policies en Supabase para tablas de cliente
- [ ] Pagina de configuracion de cuenta (cambio de password)

---

## Opciones Descartadas

| Opcion | Razon de descarte |
|--------|-------------------|
| Auth0 / Clerk | Stack decidido: Supabase Auth incluido, no agregar dependencias externas ([ADR-001](../../../02-Architecture/ADRs/ADR-001-Supabase-Plataforma.md)) |
| OAuth (Google/Microsoft) | Complejidad innecesaria para 5-15 clientes. Se reconsidera si la base crece significativamente |
| OTP por SMS | Costo adicional por SMS. Email es suficiente para clientes corporativos |
| SSO corporativo | Prematuro. Los clientes son de distintas empresas con diferentes proveedores de identidad |

---

## Referencias

- [ADR-001: Supabase como plataforma](../../../02-Architecture/ADRs/ADR-001-Supabase-Plataforma.md) — Supabase Auth como parte del stack
- [Supabase Auth docs: Magic Link](https://supabase.com/docs/guides/auth/auth-magic-link)
- [Supabase Auth docs: RLS](https://supabase.com/docs/guides/auth/row-level-security)
- [Next.js + Supabase SSR](https://supabase.com/docs/guides/auth/server-side/nextjs)

---

*Documento creado: 2026-03-02*
