# AGENTS.md — Prompt Maestro (OpenCode)

## Identidad y contexto del proyecto

Eres un agente de desarrollo experto operando en este repositorio.
Lee este archivo al inicio de cada sesión y respétalo en todo momento.

---

## 🔍 Permisos de lectura y búsqueda

Tienes permiso completo para leer y explorar cualquier archivo o directorio
dentro del repositorio o carpeta donde fue abierto OpenCode.

Esto incluye, sin necesidad de confirmación:

- Leer archivos de código fuente, configuración, documentación y assets
- Buscar patrones con `grep`, `glob`, `find` y herramientas similares
- Listar directorios y explorar la estructura del proyecto
- Leer `.env.example` (nunca `.env` real ni archivos con secretos reales)
- Examinar `package.json`, `tsconfig`, configs de framework, etc.
- Consultar el historial de Git (`git log`, `git diff`, `git status`)

**Antes de modificar cualquier cosa, lee primero.** Entiende el contexto
antes de escribir o cambiar código.

**Nunca necesitas pedir permiso para operaciones de solo lectura.**

---

## 🤖 Selección de modelo por tipo de tarea

Usa el modelo adecuado para cada tarea. Todos son gratuitos; el objetivo
es usar el más eficiente para cada situación, no siempre el más potente.

### 🥇 Qwen3 Coder — Default, mejor calidad general
**Úsalo para la mayoría de tareas de desarrollo:**
- Generar funciones completas o módulos nuevos
- Refactorizar código con criterio
- Desarrollo de APIs y lógica backend
- Trabajo en TypeScript, JavaScript y Python
- Cualquier tarea donde la calidad importa más que la velocidad

Es el modelo más equilibrado. Cuando tengas dudas, usa este.

### 🥈 DeepSeek Coder V2 — Más rápido, muy bueno
**Úsalo cuando la velocidad sea prioridad:**
- Cambios pequeños y localizados
- CRUDs y código repetitivo
- Componentes frontend simples
- Cuando ya entiendes el problema y solo necesitas generar código rápido

Ligeramente menos preciso que Qwen3, pero notablemente más rápido.

### 🥉 Code Llama 70B — Alternativa estable
**Úsalo como alternativa cuando los anteriores fallen o estén lentos:**
- Explicar código existente
- Scripts utilitarios
- Lógica simple y lineal

Más lento y a veces menos preciso. Úsalo como respaldo, no como default.

### ⚡ Phi-3 Mini — Ligero, solo para tareas pequeñas
**Úsalo solo para tareas muy acotadas:**
- Snippets pequeños y helpers
- Validaciones rápidas
- Funciones de una sola responsabilidad

No apto para proyectos grandes ni razonamiento complejo.

---

## 🧩 Tabla de routing por tipo de tarea

```
Generar función / módulo completo      → Qwen3 Coder
Refactorizar con criterio              → Qwen3 Coder
API / backend / lógica de negocio      → Qwen3 Coder
TypeScript / Python complejos          → Qwen3 Coder

Componente frontend                    → DeepSeek Coder V2
CRUD / código repetitivo               → DeepSeek Coder V2
Cambio pequeño y localizado            → DeepSeek Coder V2
Velocidad sobre profundidad            → DeepSeek Coder V2

Explicar código existente              → Code Llama 70B
Scripts simples / utilitarios          → Code Llama 70B
Fallback cuando otros fallan           → Code Llama 70B

Snippet rápido / helper                → Phi-3 Mini
Validación simple / función corta      → Phi-3 Mini
```

---

## ⚙️ Comportamiento general

### Al iniciar una tarea
1. Lee los archivos relevantes antes de escribir código
2. Entiende el patrón existente del proyecto (naming, estructura, estilo)
3. Confirma el plan si la tarea es destructiva o ambigua
4. Prefiere cambios incrementales sobre rewrites completos

### Al escribir código
- Sigue las convenciones existentes del proyecto sin imponer las tuyas
- No añadas dependencias nuevas sin preguntar
- No modifiques configuraciones de Git, CI/CD o infraestructura sin confirmación explícita
- Nunca uses `--force`, `--no-verify` o flags destructivos sin que el usuario lo pida

### Al comunicarte
- Respuestas concisas por defecto; detalla solo cuando se solicite
- Si tienes dudas sobre el alcance, pregunta antes de actuar
- Reporta qué hiciste, qué cambió y por qué

---

## 🧠 Memoria persistente con Obsidian

Este proyecto usa **obsidian-skills** (github.com/kepano/obsidian-skills) como
sistema de memoria entre sesiones. Las skills de Obsidian están instaladas en
`.opencode/skills/` y enseñan a leer y escribir los formatos nativos de
Obsidian (Markdown, Bases, Canvas).

### Vault de memoria

La memoria persistente vive en el vault de Obsidian donde hayas instalado
estas skills (la misma carpeta donde está este `AGENTS.md`).

Dentro del vault, usa esta estructura para tu memoria de trabajo:

```
IA-Memory/
├── _context.md       ← estado actual del proyecto (lee esto cada sesión)
├── _log.md           ← registro cronológico de sesiones
├── decisions/        ← decisiones técnicas importantes (ADRs ligeros)
├── patterns/         ← patrones y soluciones reutilizables descubiertos
└── sessions/         ← resúmenes exportados de sesiones pasadas
```

### Al INICIAR cada sesión

1. **Carga las skills de Obsidian** (si trabajas con memoria persistente):
   - Ejecuta `skill: obsidian-markdown` para crear/editar notas
   - Ejecuta `skill: obsidian-bases` para trabajar con bases de datos
   - Ejecuta `skill: obsidian-cli` para operaciones CLI en el vault
2. Lee `IA-Memory/_context.md` para retomar el estado anterior
3. Lee las entradas recientes de `_log.md` si necesitas más detalle
4. Consulta `decisions/` si la tarea involucra áreas ya documentadas
5. Solo entonces comienza a trabajar — nunca desde cero sin leer el contexto

### Al FINALIZAR cada sesión (o cuando te lo pida el usuario)

Actualiza la memoria en el vault:

1. **`_context.md`** — Reescribe el estado actual: qué está en progreso,
   qué decisiones están tomadas, cuál es el próximo paso claro
2. **`_log.md`** — Agrega una entrada con fecha, lo que se hizo y los
   archivos modificados. Formato: `## YYYY-MM-DD — [resumen breve]`
3. **`decisions/`** — Si se tomó una decisión técnica relevante (arquitectura,
   librería elegida, patrón adoptado), crea o actualiza una nota aquí
4. **`patterns/`** — Si descubriste una solución reutilizable, documéntala aquí

### Reglas de la memoria

- **Lee `_context.md` sin que te lo pidan** al inicio de cada sesión
- **Escribe en el vault solo contenido generado** — no mezcles con notas personales
- Mantén `_context.md` conciso (máximo ~200 líneas); mueve el detalle a `_log.md`
- Usa Obsidian Flavored Markdown: wikilinks `[[nota]]`, tags `#tag`, frontmatter YAML
- Nunca borres entradas de `_log.md`; es un log append-only

---

## 🛠️ Skills de desarrollo instaladas

Este proyecto utiliza **skills especializadas** que amplían tus capacidades para development, frontend design, y patrones de composición. 

### Instalación inicial (ejecuta una sola vez)

Instala las skills de Vercel, Anthropic y Obsidian usando estos comandos:

```bash
npx skills add vercel-labs/agent-skills --skill vercel-react-best-practices
npx skills add vercel-labs/agent-skills --skill vercel-composition-patterns
npx skills add https://github.com/anthropics/skills --skill frontend-design
```

Las skills de Obsidian se instalan automáticamente si usas la aplicación Obsidian con OpenCode; están en `.opencode/skills/`.

### Uso de skills al iniciar cada sesión

**Antes de comenzar a trabajar, carga estas skills:**

1. **Obsidian skills** (si trabajas con memoria persistente):
   - Carga la skill `obsidian-markdown` si vas a crear/editar notas en el vault
   - Carga la skill `obsidian-bases` si necesitas trabajar con vistas de base de datos
   - Carga la skill `obsidian-cli` si necesitas operaciones en el vault desde CLI

2. **Development skills** (para desarrollo):
   - **`vercel-react-best-practices`** — Guía de mejores prácticas en React
   - **`vercel-composition-patterns`** — Patrones de composición y arquitectura
   - **`frontend-design`** — Principios de design de interfaces

### Consultar una skill

Si necesitas información de una skill instalada, úsala directamente:

```
skill: obsidian-markdown
¿Cómo creo una nota con frontmatter en Obsidian?
```

O directamente en el contexto de tu trabajo cuando la necesites.

---

## 🚫 Acciones que siempre requieren confirmación explícita

- Eliminar archivos o directorios
- `git push`, `git reset --hard`, `git commit --amend`
- Modificar variables de entorno o archivos de secretos
- Cambios en configuración de base de datos o infraestructura
- Cualquier acción irreversible o visible para otros

---

## 📁 Estructura del proyecto — Route-Based (ESTÁNDAR)

🎯 **IMPORTANTE**: La arquitectura **Route-Based / Feature-Grouped es el estándar obligatorio** para todos los proyectos nuevos.

Los componentes se organizan **por página/ruta**, no por tipo de archivo. Cada ruta del `app/` tiene su carpeta correspondiente en `src/components/` con todos sus componentes agrupados. Esto hace que sea trivial encontrar, modificar o eliminar todo lo relacionado con una página.

### Principios fundamentales

- ✅ **1 ruta = 1 carpeta de componentes** — todo lo de `/empleos` vive en `components/jobs/`
- ✅ **Cada componente en su propia carpeta** — con `.tsx`, `.hook.ts`, `.functions.ts`, `.types.ts`, `.constants.ts`, `index.ts`
- ✅ **Solo los archivos que se necesiten** — si no hay hooks, no crear `.hook.ts`
- ✅ **`shared/` para reutilización** — componentes usados en 2+ rutas
- ✅ **Dependencias unidireccionales** — route components → shared (nunca al revés)
- ✅ **Barrel exports** — `index.ts` en cada carpeta para imports limpios

### Organización general de carpetas

```
app/                              # Next.js App Router — rutas URL
├── layout.tsx                    # Root layout
├── page.tsx                      # Home (/)
├── (main)/                       # Route group (sin segmento URL)
│   ├── empleos/
│   │   ├── page.tsx              # Lista de empleos
│   │   └── [id]/[slug]/
│   │       └── page.tsx          # Detalle de empleo
│   ├── sobre-nosotros/
│   │   └── page.tsx
│   ├── como-funciona/
│   │   └── page.tsx
│   └── ...
└── auth/
    ├── login/
    │   └── page.tsx
    └── registro/
        └── page.tsx

src/
├── components/                   # Componentes organizados POR RUTA (kebab-case)
│   ├── home/                     # Componentes de / (home page)
│   │   ├── hero-section/
│   │   │   ├── index.ts
│   │   │   ├── hero-section.tsx
│   │   │   ├── hero-section.hook.ts
│   │   │   └── hero-section.types.ts
│   │   ├── categories-section/
│   │   ├── featured-jobs-section/
│   │   ├── how-it-works-section/
│   │   ├── testimonials-section/
│   │   ├── cta-section/
│   │   ├── faq-section/
│   │   └── index.ts              # Barrel export de todos
│   ├── jobs/                     # Componentes de /empleos
│   │   ├── list/                 # Sub-feature: lista de empleos
│   │   │   ├── job-card/
│   │   │   │   ├── index.ts
│   │   │   │   ├── job-card.tsx
│   │   │   │   ├── job-card.hook.ts
│   │   │   │   ├── job-card.functions.ts
│   │   │   │   ├── job-card.functions.test.ts
│   │   │   │   └── job-card.types.ts
│   │   │   ├── jobs-filters-panel/
│   │   │   ├── jobs-search-bar/
│   │   │   ├── jobs-pagination/
│   │   │   └── index.ts
│   │   ├── detail/               # Sub-feature: detalle de empleo
│   │   │   ├── job-header/
│   │   │   ├── job-description/
│   │   │   ├── job-requirements/
│   │   │   ├── job-benefits/
│   │   │   ├── job-sidebar/
│   │   │   └── index.ts
│   │   ├── apply/                # Sub-feature: aplicar a empleo
│   │   │   ├── job-apply-form/
│   │   │   ├── job-apply-summary/
│   │   │   └── index.ts
│   │   └── index.ts
│   ├── auth/                     # Componentes de /auth/*
│   │   ├── login-form/
│   │   ├── register-form/
│   │   ├── auth-card/
│   │   └── index.ts
│   ├── about-us/                 # Componentes de /sobre-nosotros
│   │   ├── about-us-hero/
│   │   ├── about-us-mission-vision/
│   │   ├── about-us-stats/
│   │   ├── about-us-team/
│   │   ├── about-us-countries/
│   │   └── index.ts
│   ├── how-it-works/             # Componentes de /como-funciona
│   │   ├── how-it-works-hero/
│   │   ├── how-it-works-candidates/
│   │   ├── how-it-works-companies/
│   │   └── index.ts
│   ├── companies/                # Componentes de /empresas
│   │   ├── companies-hero/
│   │   ├── companies-benefits/
│   │   ├── companies-plans/
│   │   └── index.ts
│   ├── learn-more/               # Componentes de /conoce-mas
│   │   ├── learn-more-hero/
│   │   ├── learn-more-coverage/
│   │   ├── learn-more-industries/
│   │   └── index.ts
│   ├── legal/                    # Componentes de páginas legales
│   │   ├── legal-layout/
│   │   └── index.ts
│   ├── layout/                   # Componentes de layout global
│   │   ├── header/
│   │   │   ├── header/
│   │   │   ├── mobile-menu/
│   │   │   ├── nav-bar/
│   │   │   ├── nav-link/
│   │   │   └── theme-toggle/
│   │   ├── footer/
│   │   │   ├── footer/
│   │   │   ├── footer-links/
│   │   │   ├── footer-social/
│   │   │   └── footer-legal/
│   │   └── index.ts
│   └── shared/                   # Reutilizables en 2+ rutas
│       ├── button/
│       ├── badge/
│       ├── section-header/
│       ├── cta-banner/
│       ├── feature-card/
│       ├── page-hero/
│       ├── process-steps/
│       ├── scroll-reveal/
│       ├── animated-counter/
│       ├── stats-grid/
│       ├── empty-state/
│       ├── form-field/
│       ├── skeleton-card/
│       └── index.ts
├── types/                        # Tipos organizados por dominio (kebab-case)
│   ├── job/
│   ├── job-filters/
│   ├── home/
│   ├── faq/
│   ├── layout/
│   ├── navigation/
│   ├── shared/
│   └── index.ts (por carpeta)
├── hooks/                        # Hooks compartidos globales
│   └── shared/
│       ├── use-scroll-animation/
│       ├── use-animated-counter/
│       └── use-view-transition/
├── services/                     # Capa de servicios (interfaces + impl)
│   ├── jobs/
│   │   ├── i-jobs-service.ts
│   │   ├── api-jobs-service.ts
│   │   ├── mock-jobs-service.ts
│   │   └── index.ts
│   ├── companies/
│   ├── filters/
│   ├── testimonials/
│   └── index.ts
├── lib/                          # Utilidades técnicas
│   ├── cache/
│   ├── solr/
│   ├── rate-limiter/
│   └── sanitize/
├── data/                         # Datos estáticos (mock, seed)
│   ├── jobs.data.ts
│   ├── faq.data.ts
│   └── ...
└── config/                       # Configuración de la app
    └── services.config.ts
```

### Reglas de ubicación

| Situación | Dónde va |
|---|---|
| Componente usado en **1 sola ruta** | `src/components/[ruta]/` |
| Componente usado en **2+ rutas** | `src/components/shared/` |
| Hook específico de un componente | Mismo folder del componente `.hook.ts` |
| Hook reutilizable global | `src/hooks/shared/` |
| Tipo específico de un componente | Mismo folder del componente `.types.ts` |
| Tipo compartido entre rutas | `src/types/shared/` |
| Tipo de un dominio | `src/types/[dominio]/` |

Nunca crear carpetas `ui/`, `common/`, `base/`, `utils/` — siempre `shared/`.

---

## 📂 Estructura interna de cada componente

Cada componente vive en su **propia carpeta** con archivos separados por responsabilidad:

```
component-name/
├── index.ts                      # Barrel export — solo exporta lo público
├── component-name.tsx            # Solo JSX — cero lógica, cero TypeScript inline
├── component-name.hook.ts        # Solo hooks React (useState, useEffect, etc.)
├── component-name.functions.ts   # Solo funciones puras (formateo, cálculo, validación)
├── component-name.types.ts       # Solo interfaces y types
└── component-name.constants.ts   # Solo datos estáticos y constantes
```

Crear **solo los archivos que se necesiten**:
- Si no hay lógica → no crear `.functions.ts`
- Si no hay estado → no crear `.hook.ts`
- Si no hay constantes → no crear `.constants.ts`
- **Siempre** crear `index.ts` y `.tsx`

### Ejemplo real — Componente completo (job-card)

```
src/components/jobs/list/job-card/
├── index.ts                      # export { default as JobCard } from "./job-card"
├── job-card.tsx                  # "use client" — solo JSX
├── job-card.hook.ts              # useJobCard — estado y efectos
├── job-card.functions.ts         # formatSalary, expiryColor, etc.
├── job-card.functions.test.ts    # Tests de funciones puras
└── job-card.types.ts             # JobCardProps, JobCardState
```

### Ejemplo — Componente simple (how-it-works-hero)

```
src/components/how-it-works/how-it-works-hero/
├── index.ts
└── how-it-works-hero.tsx         # Solo JSX, sin hooks ni lógica
```

### Ejemplo — Componente con constantes (how-it-works-candidates)

```
src/components/how-it-works/how-it-works-candidates/
├── index.ts
├── how-it-works-candidates.tsx
└── how-it-works-candidates.constants.ts  # Steps data, icons config
```

---

## 🚫 Regla crítica — Cero TypeScript inline en archivos `.tsx`

Los archivos `.tsx` contienen **únicamente JSX**. Prohibido escribir esto
dentro de un `.tsx`:

```typescript
// ❌ PROHIBIDO en .tsx
interface Props { ... }           // → ComponentName.types.ts
type ButtonVariant = ...          // → ComponentName.types.ts
const options = [...]             // → ComponentName.constants.ts
function formatDate() { ... }     // → ComponentName.functions.ts
const [state, setState] = ...     // → ComponentName.hook.ts
useEffect(() => { ... })          // → ComponentName.hook.ts

// ✅ PERMITIDO en .tsx
import type { ComponentNameProps } from './ComponentName.types'
import { useComponentName } from './ComponentName.hook'
import { helperFunction } from './ComponentName.functions'
import { SOME_CONSTANT } from './ComponentName.constants'

export function ComponentName({ prop }: ComponentNameProps) {
  const { value, handler } = useComponentName()
  return <div>{helperFunction(value)}</div>
}
```

---

## 📐 Responsabilidad de cada tipo de archivo

### `.tsx` — Solo JSX
- Importa tipos, hooks, funciones y constantes desde sus archivos
- Retorna JSX puro
- No define nada — solo consume

### `.hook.ts` — Solo hooks React
- `useState`, `useEffect`, `useCallback`, `useMemo`, `useRef`
- Composición de otros hooks
- Retorna un objeto tipado con `UseComponentNameReturn`
- Sin JSX ni tipos inline

### `.functions.ts` — Solo funciones puras
- Transformaciones, formateo, validaciones, cálculos
- Sin hooks de React ni efectos secundarios
- Cada función con parámetros y retorno tipados explícitamente
- **Testeables de forma aislada** — crear `.functions.test.ts`

### `.types.ts` — Solo TypeScript
- `interface`, `type`, `enum`
- Re-exports de tipos externos si el componente los extiende
- Sin lógica ni valores en runtime

### `.constants.ts` — Solo datos estáticos
- Arrays con `as const`, objetos de configuración, valores literales
- Sin lógica ni tipos inline

### `index.ts` — Solo barrel exports
- Exporta únicamente lo público del componente
- Formato: `export { default as ComponentName } from './ComponentName'`

---

## 🔗 Reglas de imports

```typescript
// ✅ Dentro del mismo componente — rutas relativas (kebab-case)
import { useJobCard } from './job-card.hook'
import type { JobCardProps } from './job-card.types'

// ✅ Entre features/rutas — siempre alias @/
import { SectionHeader } from '@/components/shared/section-header'
import { useDebounce } from '@/hooks/shared/use-debounce'
import type { JobSummary } from '@/types/job/job.types'
import { jobsService } from '@/services/jobs'

// ❌ Nunca rutas relativas largas entre features
import { Button } from '../../../shared/button'

// ❌ Nunca PascalCase en archivos
import { JobCard } from './JobCard.hook'
import type { JobCardProps } from './JobCard.types'
```

### Orden obligatorio de imports

```typescript
// 1. Librerías externas
import { useState } from 'react'
import Image from 'next/image'

// 2. Código del proyecto (SIEMPRE con @/ y kebab-case)
import { SectionHeader } from '@/components/shared/section-header'
import { jobsService } from '@/services/jobs'

// 3. Importes locales del componente (rutas relativas, kebab-case)
import { useComponent } from './component-name.hook'
import type { ComponentProps } from './component-name.types'
```

---

### Convenciones de nombres de archivos

- **TODOS los archivos**: `kebab-case` (lowercase con guiones) — SIN EXCEPCIONES
  - ❌ `JobCard.tsx`, `LoginForm.tsx`, `UserService.ts`
  - ✅ `job-card.tsx`, `login-form.tsx`, `user-service.ts`
  - ✅ `job-card.hook.ts`, `login-form.functions.ts`
  - ✅ `job-card.types.ts`, `job-card.constants.ts`
  - ✅ `api-jobs-service.ts`, `mock-jobs-service.ts`
  - ✅ `i-jobs-service.ts` (interfaces)
- Las **carpetas** también en `kebab-case`
  - ❌ `components/jobs/list/JobCard/`
  - ✅ `components/jobs/list/job-card/`
- Una excepción: Archivos de configuración (`tsconfig.json`, `.env`, etc.) y `index.ts` mantienen su formato estándar

---

## 💪 TypeScript fuertemente tipado

- **`strict: true`** es OBLIGATORIO en `tsconfig.json`
- **Prohibido usar `any`** — siempre especificar tipos explícitos
- Usar `unknown` cuando no se conoce el tipo, con type guards
- Tipar **todos** los parámetros de funciones y sus retornos
- Tipar **todos** los props de componentes con su interfaz en `.types.ts`
- Tipar **todos** los valores de retorno de hooks en `.types.ts`
- Usar `as const` en constantes para tipado literal estricto
- Usar `satisfies` para validar objetos contra un tipo sin perder inferencia
- Nunca usar `// @ts-ignore` ni `// @ts-expect-error` salvo casos extremos documentados
- Interfaces para contratos públicos, types para datos internos

---

## 🛠️ Stack y tecnologías

### TypeScript estricto
- **`strict: true`** es OBLIGATORIO en `tsconfig.json`
- **Prohibido usar `any`** — siempre especificar tipos explícitos
- Usar `unknown` cuando no se conoce el tipo, con type guards
- Tipos genéricos con constraints cuando sea necesario
- Interfaces para contratos públicos, types para datos internos
- Tipar **todos** los parámetros de funciones y sus retornos
- Tipar **todos** los props de componentes con su interfaz en `.types.ts`
- Tipar **todos** los valores de retorno de hooks
- Usar `as const` en constantes para tipado literal estricto
- Usar `satisfies` para validar objetos contra un tipo sin perder inferencia
- Nunca usar `// @ts-ignore` ni `// @ts-expect-error` salvo casos extremos documentados

### Alias `@` para imports (OBLIGATORIO)
- **Usar alias `@` para todos los imports de proyecto** — nunca rutas relativas entre features
- Configurar en `tsconfig.json`:
  ```json
  {
    "compilerOptions": {
      "baseUrl": ".",
      "paths": {
        "@/*": ["src/*"]
      }
    }
  }
  ```
- En Next.js App Router, ajustar a `"@/*": ["./src/*"]` o `"@/*": ["./*"]` según estructura
- Estructura de imports obligatoria:
  ```typescript
  // 1. Librerías externas
  import { useState } from 'react'
  import Image from 'next/image'
  import { z } from 'zod'
  
  // 2. Código del proyecto (SIEMPRE con @/ y kebab-case)
  import { SectionHeader } from '@/components/shared/section-header'
  import { jobsService } from '@/services/jobs'
  import type { JobSummary } from '@/types/job/job.types'
  
  // 3. Importes locales del componente (rutas relativas cortas, kebab-case)
  import { useComponent } from './component-name.hook'
  import type { ComponentProps } from './component-name.types'
  ```
- **Ventajas**:
  - Imports independientes de profundidad de carpetas
  - Refactorización segura sin romper rutas
  - Claridad: `@/components/shared/X` vs `@/components/jobs/Y` es obvio
  - Depuración de importes circulares más fácil

### Next.js App Router
- **Server Components por defecto** — `page.tsx`, `layout.tsx` son server
- **`"use client"` SOLO cuando sea estrictamente necesario**:
  - `useState`, `useEffect`, `useRef`
  - Event listeners del navegador
  - APIs del browser (`window`, `document`, `navigator`)
- **Streaming con Suspense** para contenido pesado
- **`revalidate`** configurado en páginas con datos dinámicos
- **`generateStaticParams`** para páginas de detalle
- **`generateMetadata`** obligatorio en cada `page.tsx`

### Testing
- **Unitarios**: Vitest o Jest
- **Funciones puras**: `.functions.test.ts` junto al `.functions.ts`
- **E2E**: Playwright o Cypress según el proyecto
- Tests deben ejecutarse **antes de cada commit** (hook pre-commit)
- Cobertura mínima esperada: 70% en lógica crítica

### Logging
- **Formato JSON** para todos los logs
- Estructura esperada:
  ```json
  {
    "timestamp": "2024-04-01T10:30:00Z",
    "level": "info|warn|error|debug",
    "module": "user-service",
    "message": "User created successfully",
    "data": { "userId": "123", "email": "user@example.com" },
    "error": null
  }
  ```
- En `catch` blocks: incluir siempre stack trace en logs de error
- Usar `console.log/error` pero con formato JSON — no `console.error` sin estructura

### Service Layer — Interface + API + Mock
- Cada dominio tiene interfaz + implementación real + mock
- DI container resuelve cuál usar según `SERVICE_MODE`
- Permite desarrollo offline y testing sin dependencias externas

---

## 🎯 Patrones de diseño

### 1. Service Layer — Interface + API + Mock (ESTÁNDAR)

Cada servicio tiene 3 implementaciones con interfaz compartida:

```typescript
// services/jobs/i-jobs-service.ts — Contrato
export interface IJobsService {
  getAll(): Promise<JobSummary[]>
  getById(id: string): Promise<JobDetail | null>
  search(filters: JobFilters): Promise<JobSummary[]>
}

// services/jobs/api-jobs-service.ts — Implementación real
export class ApiJobsService implements IJobsService {
  async getAll(): Promise<JobSummary[]> { /* fetch real */ }
  async getById(id: string): Promise<JobDetail | null> { /* fetch real */ }
}

// services/jobs/mock-jobs-service.ts — Mock para desarrollo
export class MockJobsService implements IJobsService {
  async getAll(): Promise<JobSummary[]> { /* return mock data */ }
  async getById(id: string): Promise<JobDetail | null> { /* return mock */ }
}

// services/jobs/index.ts — Export singleton
import { container } from '../container'
export const jobsService = container.resolve<IJobsService>('jobsService')
```

**Resolución por DI container:**

```typescript
// services/container.ts
import { SERVICE_MODE } from '@/config/services.config'

export const container = {
  resolve<T>(key: string): T {
    const mode = SERVICE_MODE // 'api' | 'mock'
    const map = {
      jobsService: mode === 'mock' ? MockJobsService : ApiJobsService,
      // ...
    }
    return new map[key]() as T
  }
}
```

**Cuándo usarlo:**
- Cualquier acceso a datos externos (APIs, DB, cache)
- Permite desarrollar con mocks sin tocar la UI
- Switch entre API/mock con una variable de entorno

### 2. Page Component — Server + Client split

**Server Component (page.tsx)** — fetch data, compone layout:

```typescript
// app/(main)/empleos/page.tsx
export const revalidate = 3600

export default async function EmpleosPage() {
  const allJobs = await jobsService.getAll()
  return (
    <div>
      <h1>Empleos ejecutivos</h1>
      <JobsPageWrapper allJobs={allJobs} />
    </div>
  )
}
```

**Client Component (wrapper)** — interactividad, estado, filtros:

```typescript
// components/jobs/list/jobs-page-wrapper/jobs-page-wrapper.tsx
'use client'

export function JobsPageWrapper({ allJobs }: JobsPageWrapperProps) {
  const { filteredJobs, filters, setFilters } = useJobsPageWrapper(allJobs)
  return (
    <>
      <JobsFiltersPanel filters={filters} onChange={setFilters} />
      <JobsGrid jobs={filteredJobs} />
      <JobsPagination />
    </>
  )
}
```

**Cuándo usarlo:**
- Todas las páginas de Next.js App Router
- Server por defecto, Client solo cuando haya interactividad

### 3. Component Composition — Barrel + Suffix files

```
component-name/
├── index.ts                      # export { default as ComponentName }
├── component-name.tsx            # Solo JSX
├── component-name.hook.ts        # Solo hooks
├── component-name.functions.ts   # Solo funciones puras
├── component-name.types.ts       # Solo tipos
└── component-name.constants.ts   # Solo constantes
```

**Cuándo usarlo:**
- Todos los componentes del proyecto
- Crear solo los archivos necesarios

### 4. Feature Grouping — Sub-rutas dentro de una ruta

```
src/components/jobs/
├── list/          # Componentes de la lista /empleos
│   ├── job-card/
│   ├── jobs-filters-panel/
│   └── jobs-search-bar/
├── detail/        # Componentes del detalle /empleos/[id]/[slug]
│   ├── job-header/
│   ├── job-description/
│   └── job-sidebar/
├── apply/         # Componentes de aplicar /empleos/[id]/[slug]/aplicar
│   ├── job-apply-form/
│   └── job-apply-summary/
└── index.ts       # Barrel de todo
```

**Cuándo usarlo:**
- Rutas con sub-secciones lógicas
- Mantiene componentes agrupados por contexto de uso

---

## ⚠️ Manejo de errores

### Custom error classes (OBLIGATORIO)
```typescript
// shared/errors/app-error.ts
export abstract class AppError extends Error {
  abstract readonly statusCode: number;
  abstract readonly message: string;

  constructor(message?: string) {
    super(message || this.message);
    Object.setPrototypeOf(this, new.target.prototype);
  }
}

// Errores específicos
export class ValidationError extends AppError {
  readonly statusCode = 400;
  readonly message = 'Validation error';
  constructor(public details: string) {
    super(details);
  }
}

export class NotFoundError extends AppError {
  readonly statusCode = 404;
  readonly message = 'Resource not found';
}

export class ConflictError extends AppError {
  readonly statusCode = 409;
  readonly message = 'Resource already exists';
}

export class UnauthorizedError extends AppError {
  readonly statusCode = 401;
  readonly message = 'Unauthorized';
}
```

### Patrones de uso
```typescript
try {
  // lógica
} catch (error) {
  if (error instanceof ValidationError) {
    logger.warn('Validation failed', {
      module: 'user-service',
      error: error.message,
      details: error.details
    });
    throw error;
  }
  
  logger.error('Unexpected error', {
    module: 'user-service',
    message: error instanceof Error ? error.message : 'Unknown error',
    stack: error instanceof Error ? error.stack : null
  });
  throw new AppError('Internal server error');
}
```

### Reglas
- **Siempre lanzar `AppError` o subclases**, nunca strings o números
- **Loguear con contexto** antes de re-lanzar
- **No exponer detalles internos** al cliente (sanitizar mensajes de error)
- En logs: incluir stack trace (`error.stack`) solo en desarrollo

---

## 📝 Convenciones de código

### Variables y funciones
- ✅ `const getUserData = () => {}`
- ✅ `const isActive = true`
- ✅ `const MAX_RETRIES = 3`
- ❌ `const get_user_data = () => {}`
- ❌ `const user_is_active = true`

### Clases y tipos
- ✅ `class UserService { }`
- ✅ `interface IUserRepository { }`
- ✅ Componentes en PascalCase (clases/funciones): `export function JobCard()`
- ✅ Archivos en kebab-case: `job-card.tsx`, `login-form.hook.ts`
- ❌ `class user-service { }` (en archivos sí, pero la clase es PascalCase)

### Exportaciones
```typescript
// ✅ Barrel export en index.ts
export { default as ComponentName } from './component-name'
export type { ComponentNameProps } from './component-name.types'

// ✅ Exportar solo lo público del servicio
export class UserService { }
export interface IUserRepository { }
export type CreateUserDTO = { email: string };

// ❌ Evitar exports nombrados de constantes internas
// internal/logger.ts (no exportar)
```

### Imports
- **Obligatorio usar alias `@/` para imports entre features/rutas** — nunca rutas relativas largas
- Rutas relativas SOLO dentro del mismo componente (archivos hermanos)
- Agrupar en este orden:
  1. Librerías externas
  2. Código del proyecto (siempre con `@/`)
  3. Importes locales del componente (solo rutas relativas cortas)
- Ordenar alfabéticamente dentro de cada grupo
```typescript
// ✅ CORRECTO — kebab-case + alias @/
import { useState } from 'react'
import Image from 'next/image'

import { SectionHeader } from '@/components/shared/section-header'
import { jobsService } from '@/services/jobs'
import type { JobSummary } from '@/types/job/job.types'

import { useJobCard } from './job-card.hook'
import type { JobCardProps } from './job-card.types'

// ❌ EVITAR — PascalCase en archivos
import { JobCard } from '@/components/jobs/list/JobCard'

// ❌ EVITAR — rutas relativas entre features
import { JobCard } from '../../jobs/list/job-card'
import { AppError } from '../../../../shared/errors/app-error'
```

---

## 📚 Documentación esperada

### README por ruta/feature
```markdown
# Jobs Feature

Gestión del dominio de empleos.

## Rutas
- `/empleos` — Lista de empleos con filtros
- `/empleos/[id]/[slug]` — Detalle de empleo
- `/empleos/[id]/[slug]/aplicar` — Formulario de aplicación

## Componentes
- `list/` — job-card, jobs-filters-panel, jobs-search-bar, jobs-pagination
- `detail/` — job-header, job-description, job-requirements, job-sidebar
- `apply/` — job-apply-form, job-apply-summary

## Servicios
- `i-jobs-service.ts` — Contrato
- `api-jobs-service.ts` — Implementación real (Solr)
- `mock-jobs-service.ts` — Mock para desarrollo
```

### Architecture Decision Records (ADRs)
Ubicar en `docs/adr/` con formato: `YYYY-MM-DD-decision-name.md`
```markdown
# ADR-001: Usar Route-Based Architecture

## Contexto
...

## Decisión
...

## Consecuencias
...
```

### CHANGELOG
Mantener actualizado en `CHANGELOG.md` con versión y cambios

---

## 🔄 Versionado y Commits

### Estrategia de branches (Git Flow)
```
main (production) ← release/v1.2.0 ← develop ← feature/user-auth
                                              ← fix/login-bug
```

- **`main`**: Solo código en producción, tagged con versiones
- **`develop`**: Rama de integración, la más estable del desarrollo
- **`feature/*`**: Nuevas funcionalidades desde `develop`
- **`fix/*`**: Bugs críticos desde `develop`
- **`release/*`**: Preparación de release, merge a `main` y back a `develop`
- **`hotfix/*`**: Bugs críticos de producción desde `main`

### Conventional Commits (OBLIGATORIO)
```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types:**
- `feat:` - Nueva funcionalidad
- `fix:` - Bug fix
- `docs:` - Cambios en documentación
- `style:` - Formateo, sin cambio de lógica
- `refactor:` - Cambio de código sin nuevas features
- `test:` - Tests nuevos o actualizados
- `chore:` - Dependencias, config, scripts

**Ejemplos:**
```
feat(auth): Add JWT token refresh mechanism

fix(user-service): Handle null email validation

docs(readme): Update installation instructions

chore(deps): Upgrade TypeScript to 5.2
```

### Reglas
- ✅ Commits pequeños y atómicos
- ✅ Mensaje descriptivo en presente
- ✅ Reference a issues: `Closes #123`
- ❌ "wip", "fix", "update" sin contexto

---

## ✅ Buenas prácticas NO NEGOCIABLES

1. **Tests antes de commit**
   - Hook pre-commit ejecuta tests
   - Cobertura mínima: 70%
   - Tests E2E para flows críticos

2. **No hardcodear secretos/config**
   - Usar `.env` (localmente) y variables de entorno
   - CI/CD usa secrets seguros
   - `.env.example` para referencia

3. **Logs estructurados (JSON)**
   - Siempre timestamp, level, module, message
   - En errores: incluir stack trace
   - En operaciones: incluir IDs de contexto

4. **Documentación actualizada**
   - README funcional
   - JSDoc en funciones públicas
   - ADRs para decisiones importantes
   - CHANGELOG en cada release

5. **Versionado semántico**
   - `MAJOR.MINOR.PATCH` (ej: 2.1.3)
   - MAJOR: breaking changes
   - MINOR: nuevas features
   - PATCH: bug fixes
   - Tags en git: `v2.1.3`

6. **Commits descriptivos**
   - Conventional Commits obligatorio
   - Mensaje en presente
   - Contextualizado (scope)

7. **Code review**
   - Toda entrada a `develop` requiere approval
   - Toda entrada a `main` requiere approval + tests pasando

---

## 🚫 Prohibiciones explícitas

- ❌ **Usar `any` en TypeScript** — siempre tipo explícito o `unknown`
- ❌ **TypeScript inline en `.tsx`** — interfaces, types, funciones, hooks van en sus archivos
- ❌ **Hardcodear URLs/config** — usar variables de entorno
- ❌ **Merges a `main` sin tests pasando** — bloquear en GitHub
- ❌ **Commits sin scope/type** — `feat(auth):` es obligatorio
- ❌ **Funciones sin documentación** (públicas) — JSDoc mínimo
- ❌ **Exports de internals** — solo API pública
- ❌ **Cambios destructivos sin CHANGELOG** — documentar breaking changes
- ❌ **Importes con rutas relativas entre features** — SIEMPRE usar `@/`
- ❌ **Rutas relativas tipo `../../../`** — refactorización frágil, usar alias `@/`
- ❌ **Crear carpetas `ui/`, `common/`, `base/`, `utils/`** — siempre `shared/`
- ❌ **`<img>` directamente** — siempre `next/image` con dimensiones explícitas
- ❌ **`<a>` para navegación interna** — siempre `next/link`
- ❌ **`"use client"` innecesario** — solo cuando haya estado o eventos del navegador
- ❌ **Archivos en PascalCase** — TODOS los archivos en `kebab-case`
- ❌ **Imports sin alias `@/`** entre features — obligatorio usar `@/`
