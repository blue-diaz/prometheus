---
Proveniência e Autoria: Este documento integra o projeto Prometheus (licença MIT-0).
---

# Guia de Configuração

Este guia reflete a configuração observável no projeto atual, com base em [`prometheus.config.json`](../../prometheus.config.json), nos comandos da CLI e nas variáveis lidas durante a execução.

## Arquivo Principal

O arquivo de configuração versionado do projeto é:

- [`prometheus.config.json`](../../prometheus.config.json) na raiz do projeto analisado.

## Blocos de Configuração

### `INCLUDE_EXCLUDE_RULES`

Controla exclusões globais de arquivos.

Exemplo atual do projeto:

```json
{
  "INCLUDE_EXCLUDE_RULES": {
    "globalExcludeGlob": [
      "node_modules/**",
      "dist/**",
      "build/**",
      "coverage/**",
      ".git/**",
      "**/*.log",
      ".prometheus/**",
      "scripts/**",
      "**/*.lock",
      "package-lock.json",
      "yarn.lock",
      "pnpm-lock.yaml"
    ],
    "dirRules": {},
    "defaultExcludes": true
  }
}
```

Uso prático:

- reduzir ruído no `diagnosticar`
- evitar análise sobre artefatos gerados
- complementar com `--include` e `--exclude` na CLI

### `languageSupport`

Ativa ou desativa suporte por linguagem.

Exemplo atual:

```json
{
  "languageSupport": {
    "javascript": { "enabled": true, "parser": "babel", "plugin": "core" },
    "typescript": { "enabled": true, "parser": "babel", "plugin": "core" },
    "html": { "enabled": true, "parser": "htmlparser2", "plugin": "core" },
    "css": { "enabled": true, "parser": "css-tree", "plugin": "core" },
    "xml": { "enabled": true, "parser": "fast-xml-parser", "plugin": "core" },
    "php": { "enabled": true, "parser": "heuristic", "plugin": "core" },
    "python": { "enabled": true, "parser": "heuristic", "plugin": "core" },
    "java": { "enabled": false, "parser": "java", "plugin": "java" },
    "kotlin": { "enabled": false, "parser": "kotlin", "plugin": "kotlin" }
  }
}
```

### `nameConventions`

Define nomes esperados de diretórios principais do repositório.

```json
{
  "nameConventions": {
    "srcDirectory": "src",
    "distDirectory": "dist",
    "docsDirectory": "docs",
    "typesDirectory": "types",
    "testsDirectory": "tests",
    "configDirectory": "config",
    "scriptsDirectory": "scripts",
    "assetsDirectory": "assets"
  }
}
```

### `rules`

Customiza severidade e exceções por regra.

Exemplo atual:

```json
{
  "rules": {
    "tipo-inseguro": { "severity": "error", "exclude": [] },
    "arquivo-orfao": {
      "severity": "error",
      "allowTestFiles": false,
      "exclude": ["**/*.test.ts", "**/*.spec.ts"]
    },
    "dependencia-circular": {
      "severity": "error",
      "showFullPath": true,
      "failFast": true
    },
    "poor-naming": { "severity": "error", "exclude": ["**/*.d.ts"] },
    "unused-import": { "severity": "error", "exclude": [] },
    "duplicate-code": { "severity": "error", "exclude": [], "threshold": 3 },
    "missing-jsdoc": {
      "severity": "warning",
      "exclude": ["**/*.test.ts", "**/*.spec.ts", "test/**/*", "tests/**/*"]
    },
    "unhandled-async": { "severity": "error", "exclude": [] }
  }
}
```

### `suppress`

Suprime regras específicas no projeto.

```json
{
  "suppress": {
    "rules": [],
    "severity": {
      "unused-import": "error",
      "poor-naming": "error",
      "unhandled-async": "error",
      "missing-jsdoc": "warning",
      "weak-crypto": "error",
      "magic-constants": "warning"
    },
    "paths": []
  }
}
```

### `testPatterns`

Centraliza padrões de teste usados pelo projeto.

```json
{
  "testPatterns": {
    "files": [
      "**/*.test.*",
      "**/*.spec.*",
      "test/**/*",
      "tests/**/*",
      "**/__tests__/**"
    ],
    "excludeFromOrphanCheck": true,
    "allowAnyType": true
  }
}
```

### `ANALISE_LIMITES`

Define limites para detecções específicas.

```json
{
  "ANALISE_LIMITES": {
    "FUNCOES_LONGAS": {
      "MAX_LINHAS": 30,
      "MAX_PARAMETROS": 3,
      "MAX_ANINHAMENTO": 2
    },
    "CODIGO_FRAGIL": {
      "MAX_LINHAS_FUNCAO": 30,
      "MAX_PARAMETROS": 3,
      "MAX_NESTED_CALLBACKS": 2
    }
  }
}
```

### `GUARDIAN_*`

Configurações do sistema Guardian.

```json
{
  "GUARDIAN_ENABLED": true,
  "GUARDIAN_ENFORCE_PROTECTION": true,
  "GUARDIAN_BASELINE": ".prometheus/guardian-baseline.json",
  "GUARDIAN_ALLOW_ADDS": false,
  "GUARDIAN_ALLOW_CHG": false,
  "GUARDIAN_ALLOW_DELS": false,
  "GUARDIAN_STRICT_MODE": true,
  "GUARDIAN_REPORT_ENABLED": true
}
```

### `PLUGINS`

Configuração do sistema de plugins.

```json
{
  "PLUGINS": {
    "enabled": ["core"],
    "autoload": true,
    "registry": "@prometheus/plugins"
  }
}
```

### Configurações de Performance

```json
{
  "ANALISE_TIMEOUT_POR_ANALISTA_MS": 30000,
  "WORKER_POOL_ENABLED": true,
  "WORKER_POOL_MAX_WORKERS": 0,
  "WORKER_POOL_BATCH_SIZE": 10,
  "AUTOANALISE_CONCURRENCY": 5,
  "CACHE_STRATEGY": "never"
}
```

### Configurações de Relatório

```json
{
  "REPORT_OUTPUT_DIR": "reports",
  "REPORT_EXPORT_FULL": true,
  "REPORT_FRAGMENT_OCCURRENCES": 500,
  "REPORT_FRAGMENT_FILEENTRIES": 200,
  "REPORT_FRAGMENT_SUMMARY_TOPN": 10,
  "REPORT_SILENCE_LOGS": false
}
```

### Configurações Globais

```json
{
  "locale": "pt",
  "verbose": true,
  "logLevel": "debug",
  "logEstruturado": true,
  "devMode": true,
  "safeMode": true,
  "enforceStrictMode": true,
  "allowPlugins": true,
  "allowExec": false,
  "allowMutateFs": false,
  "REPO_ARQUETIPO": "cli-modular",
  "ESTRUTURA_ARQUIVOS_RAIZ_MAX": 5,
  "TYPE_COVERAGE_THRESHOLD": 100,
  "MAX_CYCLOMATIC_COMPLEXITY": 5,
  "MAX_FILE_LENGTH": 300,
  "FAIL_ON_ORPHAN_FILES": true
}
```

## Precedência

Na prática, a ordem de aplicação é:

1. flags da CLI
2. variáveis de ambiente reconhecidas na execução
3. `prometheus.config.json`
4. defaults internos do código

Exemplos:

```bash
prometheus diagnosticar --include "src/**" --exclude "**/*.test.ts"
prometheus formatar --engine auto --write
prometheus fix-types --confidence 90
```

## Variáveis de Ambiente Observáveis

As variáveis abaixo aparecem diretamente no código atual e afetam execução ou comportamento:

- `NODE_ENV`
- `PROMETHEUS_DEBUG`
- `PROMETHEUS_ANALISE_TIMEOUT_POR_ANALISTA_MS`
- `WORKER_POOL_MAX_WORKERS`
- `GUARDIAN_IGNORE_PATTERNS`
- `VITEST`
- `PROMETHEUS_API_PORT` (porta do servidor API, padrão: 3100)
- `PROMETHEUS_API_CORS_ORIGINS` (origens permitidas para CORS)

Exemplos:

```bash
PROMETHEUS_DEBUG=true prometheus diagnosticar --full
PROMETHEUS_ANALISE_TIMEOUT_POR_ANALISTA_MS=60000 prometheus diagnosticar
WORKER_POOL_MAX_WORKERS=4 prometheus diagnosticar --fast
PROMETHEUS_API_PORT=8080 node dist/bin/index.js github-actions app
```

## Filtros por CLI

Além do arquivo JSON, o fluxo atual depende fortemente de filtros em linha de comando:

```bash
prometheus diagnosticar --include "src/**"
prometheus diagnosticar --exclude "**/*.spec.ts"
prometheus diagnosticar --exclude-tests
prometheus formatar --include "src/**/*.ts"
prometheus fix-types --exclude "src/legacy/**"
```

Regra prática:

- use `prometheus.config.json` para defaults do repositório
- use flags para escopo da execução atual

## Exemplo Enxuto

Para um projeto mínimo:

```json
{
  "prometheus": {
    "locale": "pt",
    "verbose": true,
    "exclude": ["node_modules/**", "dist/**", "coverage/**", ".git/**"],
    "INCLUDE_EXCLUDE_RULES": {
      "globalExcludeGlob": ["node_modules/**", "dist/**", "coverage/**"]
    },
    "REPO_ARQUETIPO": "cli-modular",
    "languageSupport": {
      "javascript": { "enabled": true },
      "typescript": { "enabled": true },
      "html": { "enabled": true },
      "css": { "enabled": true }
    }
  }
}
```

## Validação Operacional

Depois de ajustar a configuração, valide com:

```bash
prometheus diagnosticar --full
prometheus diagnosticar --json --export
prometheus guardian --diff
```
