# Entretiempos Psicología — Plataforma a Medida

## Contexto del proyecto

**Cliente:** Entretiempos Psicología  
**Web actual:** entretiempospsicologia.com  
**Fundadora:** Psicóloga uruguaya, formada en Universidad Católica del Uruguay y especializada en Psicopatología Clínica en la Universidad de Barcelona. Actualmente cursando maestría en Psicoterapia Junguiana.  
**Alcance:** +600.000 seguidores en Instagram. Plataforma con presencia real en LatAm y Uruguay.

**Desarrollador responsable:** Mauricio Sawicki — mau.sawicki@gmail.com

---

## Problema que resuelve

La plataforma actual corre sobre **WordPress + WooCommerce + Ultimate Member Pro + PayPal**. El stack acumula conflictos de versiones entre plugins, genera errores impredecibles y requiere intervención constante de desarrolladores sin garantía de resolución.

Bug principal actual: cuando un psicólogo paga la membresía, el rol "Psicólogo" no se asigna automáticamente — falla la sincronización entre WooCommerce Subscriptions y Ultimate Member.

**Costo actual de infraestructura:** ~USD 500/año  
**Costo objetivo con nueva plataforma:** ~USD 80/año

---

## Stack tecnológico

| Tecnología | Uso |
|------------|-----|
| **Next.js** | Frontend — web app responsive |
| **Supabase** | Base de datos (PostgreSQL), autenticación y storage |
| **Vercel** | Deploy y hosting |
| **Stripe** | Cobros recurrentes internacionales con tarjeta |
| **dLocal Go** | Pagos locales para Uruguay y LatAm |

> Mercado Pago queda como opción de Fase 2 si hay demanda.  
> La app es **web únicamente** — acceso por URL desde navegador, totalmente responsive. No hay app móvil nativa en el MVP.

---

## Modelo de negocio

- **Psicólogo:** paga USD 40/mes para tener perfil activo en la plataforma
- **Paciente:** acceso completamente gratuito para buscar y agendar sesiones
- **Membresía:** única (no hay niveles ni tiers)
- **Sesiones:** el psicólogo y el paciente coordinan el canal de sesión por fuera de la plataforma (Zoom, Meet, etc.) — no hay videollamada integrada en el MVP
- **Facturación:** mensual, recurrente, automática

---

## Usuarios del sistema

### 1. Psicólogo
- Se registra, paga y queda **pendiente de aprobación** hasta que admin lo aprueba manualmente
- Una vez aprobado, su perfil se publica en el directorio
- Puede editar su propio perfil (foto, bio, especialidades) — admin también puede editarlo
- Carga su disponibilidad horaria en un calendario
- Recibe email automático cuando un paciente agenda
- Sin período de prueba gratuito — paga desde el primer día
- Si deja de pagar: tiene un **período de gracia configurable** antes de que su perfil se oculte automáticamente

### 2. Paciente
- Registro gratuito
- Busca psicólogos en el directorio con filtros
- Agenda sesión eligiendo psicólogo, día y horario disponible
- Recibe confirmación por email
- Puede dejar **reseñas y puntaje** al psicólogo

### 3. Administrador (la fundadora)
- Aprueba o rechaza solicitudes de nuevos psicólogos
- Edita perfiles de psicólogos si es necesario
- Ve lista de psicólogos activos e inactivos
- Ve pagos recibidos y pendientes
- Ve turnos agendados en la plataforma
- Ve reportes de ingresos mensuales
- Configura período de gracia para impagos

---

## Módulos del MVP

| Módulo | Descripción |
|--------|-------------|
| **Registro de psicólogos** | Formulario → pago → estado pendiente → aprobación manual por admin |
| **Perfil del psicólogo** | Foto, bio, especialidades — editable por el psicólogo y por admin |
| **Directorio de psicólogos** | Búsqueda con filtros para pacientes |
| **Calendario de disponibilidad** | El psicólogo carga sus horarios disponibles |
| **Sistema de agenda** | El paciente elige psicólogo, día y horario — confirmación automática |
| **Notificaciones automáticas** | Email al psicólogo y al paciente en cada evento del flujo |
| **Sistema de reseñas** | Los pacientes puntúan y comentan — visible en el perfil del psicólogo |
| **Membresía y cobro** | Suscripción recurrente USD 40/mes vía Stripe y dLocal Go |
| **Período de gracia** | Perfil activo X días tras vencimiento — se oculta automáticamente si no renueva |
| **Panel de administración** | Gestión de psicólogos, pagos, turnos, solicitudes e ingresos mensuales |
| **Migración de usuarios** | Importación de psicólogos y perfiles existentes desde WordPress |
| **Diseño responsive** | Optimizado para móvil, tablet y escritorio |

---

## Flujos clave

### Flujo de registro de psicólogo
1. El psicólogo completa el formulario de registro
2. Paga USD 40 vía Stripe o dLocal Go
3. Su cuenta queda en estado `pendiente`
4. Admin recibe notificación y revisa la solicitud
5. Admin aprueba → el rol cambia a `psicologo` → perfil se publica en el directorio automáticamente
6. El psicólogo recibe email de bienvenida con acceso confirmado

### Flujo de agenda
1. Paciente entra al directorio y filtra psicólogos
2. Entra al perfil de un psicólogo y ve su disponibilidad
3. Elige día y horario disponible
4. Confirma la agenda (sin pago — es gratuito para el paciente)
5. Psicólogo recibe email con los datos del turno
6. Paciente recibe confirmación por email
7. La sesión se coordina por fuera (Zoom, Meet, etc.)

### Flujo de impago
1. La suscripción del psicólogo vence
2. Se intenta el cobro automático — falla
3. Comienza el período de gracia (X días, configurable por admin)
4. Si no renueva antes de que venza el período de gracia → perfil se oculta automáticamente del directorio
5. El psicólogo puede reactivar pagando en cualquier momento

---

## Decisiones técnicas tomadas

- **Auth:** Supabase Auth con roles (`psicologo`, `paciente`, `admin`)
- **Roles:** asignados directamente en Supabase al momento de aprobación — sin lógica de plugins externos
- **Pagos recurrentes:** Stripe para internacional, dLocal Go para Uruguay/LatAm
- **Storage de imágenes de perfil:** Supabase Storage
- **Deploy:** Vercel (integración nativa con Next.js)
- **Sin videollamada integrada en MVP** — el canal de sesión lo coordinan psicólogo y paciente
- **Sin app móvil nativa** — web responsive accesible por URL

---

## Migración desde WordPress

- La mayoría de los psicólogos actuales tienen perfil completo (foto, bio)
- Se migrará a todos los psicólogos existentes a la nueva plataforma
- Los usuarios migrados recibirán instrucciones para establecer nueva contraseña
- Los turnos históricos no se migran — solo perfiles y datos de usuario

---

## Fase 2 (fuera del MVP actual)

- Videollamada integrada dentro de la plataforma
- App móvil nativa (iOS / Android)
- Mercado Pago como pasarela adicional
- Sistema de mensajería interno entre psicólogo y paciente
- Panel de analytics avanzado para la fundadora

---

## Honorarios acordados

| Concepto | Valor |
|----------|-------|
| Desarrollo del MVP | USD 3.500 |
| Pago 1 — al iniciar | USD 1.000 |
| Pago 2 — al entregar | USD 2.500 |
| Mantenimiento mensual | USD 150/mes |

---

## Tiempo estimado de desarrollo

| Etapa | Duración |
|-------|----------|
| Definición y diseño | 1 semana |
| Backend (auth, DB, roles, pagos) | 2 semanas |
| Frontend (perfiles, directorio, agenda, reseñas, admin) | 2 semanas |
| Migración de datos | 1 semana |
| Testing y deploy | 1 semana |
| **Total** | **7 semanas** |
