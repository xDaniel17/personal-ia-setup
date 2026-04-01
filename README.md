# 🚀 Personal IA Setup

Configuración centralizada de estándares, patrones y guías para desarrollo con agentes de IA (OpenCode y Claude).

---

## 📚 Documentación

### Estándares principales

- **[AGENTS.md](./AGENTS.md)** — Instrucciones para OpenCode
  - Permisos y comportamiento esperado
  - Selección de modelos por tipo de tarea
  - Arquitectura Feature-Based (ESTÁNDAR)
  - Stack y tecnologías (TypeScript, Testing, Logging)
  - Patrones de diseño y manejo de errores
  - Convenciones de código y documentación
  - Versionado, commits y buenas prácticas

- **[CLAUDE.md](./CLAUDE.md)** — Instrucciones para Claude
  - Equivalente a AGENTS.md adaptado para Anthropic
  - Especificaciones de modelos Haiku, Sonnet, Opus
  - Mismos estándares de arquitectura y código

---

## 🎯 Cómo usar

### Para desarrolladores con OpenCode:
```
1. Lee AGENTS.md al inicio de cada sesión
2. Aplica los estándares y patrones en tus proyectos
3. Sigue la arquitectura Feature-Based para nuevas features
```

### Para desarrolladores con Claude:
```
1. Lee CLAUDE.md al inicio de cada sesión
2. Aplica los estándares y patrones en tus proyectos
3. Sigue la arquitectura Feature-Based para nuevas features
```

---

## 🏗️ Arquitectura Feature-Based en 30 segundos

```
Domain-Driven Design / Feature-Based Architecture

Cada feature tiene 4 capas:

1. domain/        — Lógica pura (sin frameworks)
2. application/   — Casos de uso (orquestación)
3. infrastructure/ — Detalles técnicos (APIs, BD, caché)
4. interface/     — Presentación (React, Controllers)

shared/  — Código reutilizado en 2+ features
config/  — Configuración centralizada
data/    — Datos estáticos (fallback, seed, mock)
```

---

## 📋 Stack recomendado

| Componente | Recomendación | 
|-----------|---|
| **Lenguaje** | TypeScript estricto |
| **Framework** | Next.js 13+ (App Router) |
| **Testing** | Jest + Playwright |
| **Logging** | JSON estructurado |
| **Validación** | Zod |
| **Versionado** | SemVer + Git Flow |
| **Commits** | Conventional Commits |

---

## ✅ Buenas prácticas NO NEGOCIABLES

1. **Tests antes de commit** — Hook pre-commit ejecuta tests (70%+ cobertura)
2. **No hardcodear secretos** — Usar `.env` y variables de entorno
3. **Logs estructurados (JSON)** — timestamp, level, module, message
4. **Documentación actualizada** — README, JSDoc, ADRs, CHANGELOG
5. **Versionado semántico** — MAJOR.MINOR.PATCH
6. **Commits descriptivos** — Conventional Commits obligatorio
7. **Code review** — Approval + tests pasando antes de merge

---

## 🚫 Prohibiciones explícitas

- ❌ Usar `any` en TypeScript — siempre tipo explícito
- ❌ Hardcodear URLs/config — usar variables de entorno
- ❌ Merges sin tests — bloquear en GitHub
- ❌ Commits sin scope/type — `feat(auth):` es obligatorio
- ❌ Funciones sin JSDoc — documentación pública mínima
- ❌ Exports de internals — solo API pública
- ❌ Cambios destructivos sin CHANGELOG — documentar breaking changes
- ❌ Importes con rutas relativas en proyecto — SIEMPRE usar `@/`

---

## 📁 Estructura

```
/
├── AGENTS.md      # Instrucciones para OpenCode
├── CLAUDE.md      # Instrucciones para Claude
├── README.md      # Este archivo
└── .gitignore
```

---

## 🔗 Links

- **GitHub**: [xDaniel17/personal-ia-setup](https://github.com/xDaniel17/personal-ia-setup)
- **Rama principal**: `main`

---

**Última actualización**: 2024-04-01  
**Versión**: 1.0  
**Licencia**: MIT
