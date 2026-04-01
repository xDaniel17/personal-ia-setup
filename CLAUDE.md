# CLAUDE.md — Prompt Maestro

## Identidad y contexto del proyecto

Eres un agente de desarrollo experto operando en este repositorio.
Lee este archivo al inicio de cada sesión y respétalo en todo momento.

---

## 🔍 Permisos de lectura y búsqueda

Tienes permiso completo para leer y explorar cualquier archivo o directorio
dentro del repositorio o carpeta donde fue abierto Claude Code.

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

Usa el modelo correcto para cada tarea. Esto reduce costos y mejora velocidad
sin sacrificar calidad donde importa.

### 🔵 Haiku 4.5 — Rápido y económico
**Úsalo para tareas simples sin razonamiento profundo:**
- Buscar archivos, símbolos o patrones en el código
- Ejecutar comandos y reportar resultados (`npm test`, `build`, etc.)
- Resumir el contenido de un archivo
- Buscar errores con stack trace claro
- Formatear, lintear, o aplicar cambios mecánicos
- Tareas de subagente tipo "lookup" o "fetch"

### 🟡 Sonnet 4.6 — El caballo de trabajo (80% de las tareas)
**Úsalo para desarrollo cotidiano:**
- Implementar features siguiendo patrones existentes del proyecto
- Escribir o actualizar tests
- Corregir bugs con causa identificada
- Refactorizar código siguiendo convenciones del proyecto
- Agregar endpoints, componentes o módulos nuevos que sigan el patrón actual
- Escribir documentación desde código existente
- Tareas que requieren entender el proyecto pero no diseñar desde cero

### 🔴 Opus 4.6 — El arquitecto (solo cuando sea necesario)
**Úsalo para razonamiento profundo y decisiones complejas:**
- Diseñar nuevos módulos o subsistemas desde cero
- Evaluar trade-offs arquitectónicos
- Depurar bugs sutiles sin stack trace claro
- Revisar arquitectura buscando problemas de diseño o seguridad
- Tomar decisiones con requisitos incompletos o contradictorios
- Planificación multi-sesión o tareas de larga duración

---

## 🧩 Subagentes y routing de modelos

Cuando delegues trabajo a subagentes, asigna el modelo según complejidad:

```
Búsqueda / lookup                    → model: haiku
Ejecutar comando y reportar          → model: haiku
Resumir módulo o archivo             → model: haiku
Análisis de dependencias             → model: sonnet
Implementación siguiendo patrón      → model: sonnet
Code review de estilo y calidad      → model: sonnet
Revisión de arquitectura o seguridad → model: opus
Decisión con trade-offs complejos    → model: opus
```

El agente principal puede correr en Sonnet u Opus; los subagentes de
exploración y búsqueda siempre deben correr en Haiku para conservar tokens.

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

### Gestión del contexto
Tu contexto se compacta automáticamente al acercarse al límite.
No detengas tareas anticipadamente por preocupaciones de tokens.
Si te acercas al límite, guarda el estado actual antes de que se compacte.

---

## 🧠 Memoria persistente con Obsidian

Este proyecto usa **obsidian-skills** (github.com/kepano/obsidian-skills) como
sistema de memoria entre sesiones. Las skills de Obsidian están instaladas en
`.claude/skills/` y te enseñan a leer y escribir los formatos nativos de
Obsidian (Markdown, Bases, Canvas).

### Vault de memoria

La memoria persistente vive en el vault de Obsidian donde hayas instalado
estas skills (la misma carpeta donde está este `CLAUDE.md`).

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
   librería elegida, patrón adoptado), crea o actualiza una nota en esta carpeta
4. **`patterns/`** — Si descubriste una solución reutilizable o un patrón
   recurrente del proyecto, documéntalo aquí

### Reglas de la memoria

- **Lee `_context.md` sin que te lo pidan** al inicio de cada sesión
- **Escribe en el vault solo contenido generado** — no mezcles con notas personales
- Mantén `_context.md` conciso (máximo ~200 líneas); mueve el detalle a `_log.md`
- Usa Obsidian Flavored Markdown: wikilinks `[[nota]]`, tags `#tag`, frontmatter YAML
- Al crear notas en el vault, respeta las convenciones de las obsidian-skills instaladas
- Nunca borres entradas de `_log.md`; es un log append-only

### Skills de Obsidian disponibles

Las skills en `.claude/skills/` te enseñan los formatos de Obsidian.
Consúltalas cuando vayas a crear o editar archivos en el vault:

- `markdown` — Obsidian Flavored Markdown con wikilinks, embeds, callouts
- `bases` — formato `.base` para bases de datos en Obsidian
- `canvas` — formato `.canvas` para mapas visuales tipo JSON Canvas

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

Las skills de Obsidian se instalan automáticamente si usas la aplicación Obsidian con Claude Code; están en `.claude/skills/`.

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

## 📁 Estructura del proyecto — Feature-Based (ESTÁNDAR)

🎯 **IMPORTANTE**: La arquitectura **Feature-Based / Domain-Driven es el estándar obligatorio** para todos los proyectos nuevos. 

Esta organización por dominio/feature es superior a la organización tradicional por tipo de archivo (components/, services/, lib/) porque:
- ✅ Escalabilidad clara — agregar features es trivial
- ✅ Aislamiento — cada feature es independiente
- ✅ Testing localizado — `features/jobs/__tests__/` contiene todo
- ✅ Equipos paralelos — Equipo A en `jobs/`, Equipo B en `apply/`
- ✅ Documentación — README.md por feature explica su propósito
- ✅ Dependencias unidireccionales — features → shared (nunca al revés)

La estructura exacta **depende del proyecto específico**, pero sigue estos principios:

### Organización general
- **Feature-based / Domain-driven**: Las carpetas se organizan por dominio o feature, no por tipo de archivo
- Ejemplo para un proyecto backend:
  ```
  src/
  ├── features/
  │   ├── auth/
  │   │   ├── domain/
  │   │   ├── application/
  │   │   ├── infrastructure/
  │   │   └── auth.module.ts
  │   ├── users/
  │   │   ├── domain/
  │   │   ├── application/
  │   │   ├── infrastructure/
  │   │   └── users.module.ts
  │   └── ...
  ├── shared/
  │   ├── errors/
  │   ├── logger/
  │   ├── utils/
  │   └── types/
  ├── config/
  └── main.ts
  ```
- Cada feature es **autónomo** y reutilizable
- `shared/` contiene código que atraviesa múltiples features
- `config/` contiene configuración de la aplicación

### Estructura interna de cada feature
Cada feature sigue el patrón **Domain-Driven Design (DDD)** con 4 capas:

**1. `domain/` — Lógica de negocio pura**
- Entidades (`user.entity.ts`)
- Value Objects (`email.value-object.ts`)
- Interfaces / Contracts (`user.repository.ts`)
- **Nunca** dependen de frameworks o infraestructura

**2. `application/` — Casos de uso**
- Use cases / Operaciones (`create-user.use-case.ts`)
- DTOs para entrada/salida (`create-user.dto.ts`)
- Orquestación de lógica
- **Dependen de domain**, no de infraestructura

**3. `infrastructure/` — Detalles técnicos**
- Implementaciones de repositorios (`user.repository.impl.ts`)
- Servicios externos (DB, APIs, cache)
- Adaptadores técnicos
- **Implementan interfaces de domain**

**4. `interface/` — Presentación (solo Frontend/Fullstack)**
- Controllers (API), Pages (Next.js), Components (React)
- Handlers de HTTP / WebSocket
- Componentes visuales específicos de la feature
- **Orquestan use cases de application**

Estructura completa:
```
src/features/users/
├── domain/
│   ├── user.entity.ts          # Entidad User
│   ├── user.value-object.ts    # Email, Phone (inmutables)
│   └── user.repository.ts      # IUserRepository (interfaz)
├── application/
│   ├── create-user.use-case.ts
│   ├── get-user.use-case.ts
│   ├── dtos/
│   │   ├── create-user.dto.ts
│   │   └── get-user.dto.ts
│   └── index.ts                # Exporta use cases
├── infrastructure/
│   ├── user.repository.ts      # Implementación de IUserRepository
│   ├── adapters/
│   │   └── solr-user.adapter.ts # Adaptador a Solr
│   └── index.ts
├── interface/                   # Solo si hay HTTP / componentes
│   ├── user.controller.ts      # POST/GET /api/users
│   ├── components/
│   │   └── user-card.tsx
│   └── index.ts
├── types/
│   └── users.types.ts          # Tipos internos (DTOs, queries)
├── __tests__/
│   ├── create-user.use-case.test.ts
│   ├── user.repository.test.ts
│   └── user-card.test.tsx
├── README.md                    # Documentación de la feature
└── index.ts                     # Barrel — export público

### Convenciones de nombres de archivos
- **Todos los archivos**: `kebab-case` (todo lowercase con guiones)
  - ❌ `UserService.ts`, `userService.ts`
  - ✅ `user-service.ts`
  - ✅ `product-controller.ts`
  - ✅ `validation-error.ts`
- Las **carpetas** también en `kebab-case`
- Una excepción: Archivos de configuración (`tsconfig.json`, `.env`, etc.) mantienen su formato estándar

---

## 🛠️ Stack y tecnologías

### TypeScript estricto
- **`strict: true`** es OBLIGATORIO en `tsconfig.json`
- **Prohibido usar `any`** — siempre especificar tipos explícitos
- Usar `unknown` cuando no se conoce el tipo, con type guards
- Tipos genéricos con constraints cuando sea necesario
- Interfaces para contratos públicos, types para datos internos

### Alias `@` para imports (OBLIGATORIO)
- **Usar alias `@` para todos los imports de proyecto** — nunca rutas relativas
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
- En Next.js, ajustar a `"@/*": ["./*"]`
- Estructura de imports obligatoria:
  ```typescript
  // 1. Librerías externas
  import { Request, Response } from 'express';
  import { z } from 'zod';
  
  // 2. Código del proyecto (SIEMPRE con @/)
  import { UserService } from '@/features/users/application/user.service';
  import { AppError } from '@/shared/errors/app-error';
  
  // 3. Importes locales (última opción, solo si es necesario)
  import { validateInput } from './validators';
  ```
- **Ventajas**:
  - Imports independientes de profundidad de carpetas
  - Refactorización segura sin romper rutas
  - Claridad: `@/features/X` vs `@/shared/Y` es obvio
  - Depuración de importes circulares más fácil

### Testing
- **Unitarios**: Jest
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

### CI/CD
- **GitHub Actions** para automatización
- Workflows esperados:
  - Tests en cada PR
  - Lint en cada PR
  - Build en cada PR
  - Deploy automático en merge a `main`/`develop`
- Archivos: `.github/workflows/*.yml`

---

## 🎯 Patrones de diseño

Elige el patrón **más apropiado para cada problema**, pero considera estos recurrentes:

### 1. Arquitectura de capas (Estándar)
```
Controller (request handling)
    ↓
Service (lógica de negocio)
    ↓
Repository (acceso a datos)
    ↓
Model (entidad)
```

**Cuándo usarlo:**
- CRUD estándar
- Lógica de negocio separada de infraestructura
- Proyectos pequeños a medianos

**Naming convention:**
- `user.controller.ts` — recibe requests, valida, delega
- `user.service.ts` — lógica de negocio pura
- `user.repository.ts` — acceso a DB/cache
- `user.model.ts` — entidad/interfaz

### 2. Clean Architecture / DDD (Dominios complejos)
```
Domain (entities, value objects, business rules)
    ↓
Application (use cases, DTOs)
    ↓
Infrastructure (db, external services)
    ↓
Interfaces (controllers, presenters)
```

**Cuándo usarlo:**
- Lógica de negocio muy compleja
- Múltiples formas de acceso (API REST, GraphQL, WebSocket)
- Necesidad de cambiar detalles técnicos sin afectar core

**Estructura esperada:**
```
src/features/users/
├── domain/
│   ├── user-entity.ts
│   ├── user-repository.interface.ts
│   └── user-aggregate.ts
├── application/
│   ├── create-user.use-case.ts
│   ├── get-user.use-case.ts
│   └── dtos/
│       └── create-user.dto.ts
├── infrastructure/
│   ├── user-repository.implementation.ts
│   └── user.db-model.ts
└── interface/
    └── user.controller.ts
```

### 3. Value Objects (Valores inmutables)
```typescript
// Para cosas como Email, Money, UUID
class Email {
  private value: string;
  constructor(value: string) {
    if (!this.isValid(value)) throw new InvalidEmailError();
    this.value = value;
  }
  getValue(): string { return this.value; }
  equals(other: Email): boolean { return this.value === other.value; }
}
```

**Cuándo usarlo:**
- Datos que tienen significado en el dominio (Email, Money, PhoneNumber)
- Necesidad de validación encapsulada
- Comparación por valor, no por referencia

### 4. Factory Pattern
```typescript
class UserFactory {
  static create(data: CreateUserDTO): User {
    // Validaciones, transformaciones
    return new User(data);
  }
}
```

**Cuándo usarlo:**
- Creación compleja de objetos
- Diferentes estrategias de construcción
- Encapsular lógica de instanciación

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
- ❌ `class user-service { }` (en archivos sí, pero la clase es PascalCase)

### Exportaciones
```typescript
// ✅ Exportar solo lo público
export class UserService { }
export interface IUserRepository { }
export type CreateUserDTO = { email: string };

// ❌ Evitar exports nombrados de constantes internas
// internal/logger.ts (no exportar)
```

### Imports
- **Obligatorio usar alias `@/` para imports de proyecto** — nunca rutas relativas
- Agrupar en este orden:
  1. Librerías externas
  2. Código del proyecto (siempre con `@/`)
  3. Importes locales (solo en casos excepcionales)
- Ordenar alfabéticamente dentro de cada grupo
```typescript
// ✅ CORRECTO
import { Request, Response } from 'express';
import { z } from 'zod';

import { UserService } from '@/features/users/application/user.service';
import { AppError } from '@/shared/errors/app-error';

import { validateInput } from './validators';

// ❌ EVITAR
import { UserService } from '../../application/user.service';
import { AppError } from '../../../../shared/errors/app-error';
```

---

## 📚 Documentación esperada

### Headers de archivo
```typescript
/**
 * User Service
 * 
 * Contiene la lógica de negocio para gestión de usuarios.
 * Responsabilidades:
 * - Crear usuarios
 * - Validar datos
 * - Interactuar con repositorio
 * 
 * @author Your Team
 * @since 2024-01-15
 */
```

### Funciones públicas (JSDoc)
```typescript
/**
 * Crea un nuevo usuario en el sistema
 * 
 * @param dto - Datos del usuario a crear
 * @returns Usuario creado con ID
 * @throws ValidationError si datos inválidos
 * @throws ConflictError si el email ya existe
 * 
 * @example
 * const user = await userService.create({ email: 'user@example.com', name: 'John' });
 */
export async function create(dto: CreateUserDTO): Promise<User> {
  // implementación
}
```

### README por módulo/feature
```markdown
# Users Feature

Gestión del dominio de usuarios.

## Estructura

- `domain/` - Entidades y interfaces
- `application/` - Use cases y DTOs
- `infrastructure/` - Implementaciones técnicas
- `interface/` - Controllers y adapters

## Cómo usar

...
```

### Architecture Decision Records (ADRs)
Ubicar en `docs/adr/` con formato: `YYYY-MM-DD-decision-name.md`
```markdown
# ADR-001: Usar Feature-Based Architecture

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
- ❌ **Hardcodear URLs/config** — usar variables de entorno
- ❌ **Merges a `main` sin tests pasando** — bloquear en GitHub
- ❌ **Commits sin scope/type** — `feat(auth):` es obligatorio
- ❌ **Funciones sin documentación** (públicas) — JSDoc mínimo
- ❌ **Exports de internals** — solo API pública
- ❌ **Cambios destructivos sin CHANGELOG** — documentar breaking changes
- ❌ **Importes con rutas relativas en imports de proyecto** — SIEMPRE usar `@/`
- ❌ **Rutas relativas tipo `../../../`** — refactorización frágil, usar alias `@/`
