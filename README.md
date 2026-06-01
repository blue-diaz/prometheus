---
Proveniência e Autoria: Este documento integra o projeto Prometheus (licença MIT-0).
---

---

<div align="center">

<img src="./svg/banner-prometheus.svg" width="100%">

---

[![npm version](https://img.shields.io/npm/v/prometheus.svg)](https://www.npmjs.com/package/prometheus)
[![Node](https://img.shields.io/badge/node-%3E%3D24.16.0-brightgreen)](package.json)
[![License](https://img.shields.io/badge/license-MIT--0-blue)](LICENSE)
[![TypeScript](https://img.shields.io/badge/TypeScript-6.0+-3178C6)](tsconfig.json)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen)](CONTRIBUTING.md)

---

<p><b>CLI modular para análise, diagnóstico e manutenção de projetos JavaScript/TypeScript com suporte multi-linguagem</b><p>

</div>

---

## Índice

- [Começar Rápido (30 segundos)](#começar-rápido-30-segundos)
- [Fluxo Passo a Passo](#fluxo-passo-a-passo)
- [Funcionalidades](#funcionalidades)
- [Instalação](#instalação)
- [Configuração](#configuração)
- [Comandos da CLI](#comandos-da-cli)
- [REST API](#rest-api)
- [SDK Programático](#sdk-programático)
- [GitHub App](#github-app)
- [Editor (LSP / VSCode)](#editor-lsp--vscode)
- [CI/CD](#cicd)
- [Linguagens Suportadas](#linguagens-suportadas)
- [Documentação Completa](#documentação-completa)
- [Desenvolvimento](#desenvolvimento)
- [Contribuir](#contribuir)
- [Licença](#licença)

---

## Começar Rápido (30 segundos)

```bash
# 1. Instalar
npm install -g prometheus-cli

# 2. Analisar seu projeto
cd meu-projeto
prometheus diagnosticar

# 3. Ver problemas críticos
prometheus diagnosticar --gravidade critic

# 4. Exportar relatório
prometheus diagnosticar --relatorio json --saida relatorio.json
```

**Próximos passos imediatos:**

```bash
# Revisar correções disponíveis
prometheus corrigir --revisar

# Aplicar correções automáticas seguras
prometheus corrigir --auto

# Estabelecer baseline de saúde (assinatura GPG)
prometheus guardian --accept-baseline

# Verificar mudanças
prometheus guardian
```

---

## Fluxo Passo a Passo

O Prometheus foi projetado para guiar você por um fluxo completo de análise e manutenção do seu projeto:

### Passo 1: Diagnóstico Inicial

Analise todo o projeto para identificar problemas de qualidade, segurança e arquitetura:

```bash
# Análise completa
prometheus diagnosticar

# Mais detalhes
prometheus diagnosticar --verbose

# Apenas críticos
prometheus diagnosticar --gravidade critic

# Exportar resultados
prometheus diagnosticar --relatorio json --saida analise.json
prometheus diagnosticar --relatorio markdown --saida RELATORIO.md
```

### Passo 2: Estabelecer Baseline

Crie um baseline criptográfico com assinaturas GPG do projeto para monitorar mudanças:

```bash
prometheus guardian --accept-baseline
```

### Passo 3: Corrigir Problemas

Aplique correções automáticas com segurança:

```bash
# Revisar o que pode ser corrigido
prometheus corrigir

# Aplicar correções automáticas
prometheus corrigir --auto

# Corrigir tipos inseguros
prometheus fix-types --auto
```

### Passo 4: Organizar com Sugestões e Barrels

Revise sugestões estruturais e organize os barrels do projeto:

```bash
# Ver sugestões de reorganização (apenas sugestões, sem modificações)
prometheus diagnosticar --export --export-to ./sugestoes

# Gerar barrels estruturados (index.ts)
prometheus barrels --scan          # preview do que será criado
prometheus barrels --generate      # criar/atualizar barrels

# Remover arquivos órfãos
prometheus podar
```

### Passo 5: Monitoramento Contínuo

Mantenha a saúde do projeto ao longo do tempo:

```bash
# Verificar integridade contra baseline
prometheus guardian

# Mostrar diferenças detalhadas
prometheus guardian --diff

# Comparar performance
prometheus perf snapshot --baseline
prometheus perf compare

# Ver histórico de métricas
prometheus metricas
```

---

## Funcionalidades

### Análise Inteligente

- **Diagnóstico completo** — qualidade, segurança e arquitetura
- **15+ detectores especializados** — código frágil, duplicação, complexidade, performance, vazamentos de memória, tipos inseguros
- **18+ plugins multi-linguagem** — React, CSS, HTML, Python, Shell, SQL, Tailwind, XML, SVG
- **Detecção de bugs via ML** — 8 padrões com pontuação Bayesiana (v0.10.0)
- **Registro inteligente** — descoberta automática de analistas built-in e customizados
- **Análise de monorepos** — pnpm-workspace, lerna, nx, turbo (v0.10.0)

### Manutenção Assistida

- **Auto-fix seguro** — correções automáticas com validação (`corrigir`)
- **Guardian** — monitoramento contínuo com assinatura GPG, baselines criptográficos e verificação de integridade
- **Reestruturação** — reorganização automática seguindo padrões arquiteturais
- **Poda inteligente** — identificação e remoção de arquivos órfãos e código morto
- **Fix Types** — correção automática de `any` e `unknown`
- **Formatação** — formatação automática do código
- **Names/Rename** — extração e renomeação em massa de variáveis
- **Reverter** — rollback de movimentações aplicadas

### Relatórios Profissionais

- **Múltiplos formatos** — JSON, Markdown, HTML, CSV com sharded export
- **Métricas detalhadas** — complexidade ciclomática, duplicação, cobertura, performance com histórico
- **Baseline de performance** — snapshots e comparação ao longo do tempo
- **Scan de licenças** — verificação de dependências e geração de THIRD-PARTY-NOTICES
- **Relatórios de compliance** — ISO 27001, SOC 2
- **Streaming** — NDJSON sharded com gzip para projetos grandes (v0.10.0)

### Extensível

- **Plugin system** — crie analistas customizados com autodiscovery
- **API modular** — use como biblioteca (`prometheus/sdk`)
- **REST API** — servidor HTTP com Swagger UI
- **Worker pool** — processamento paralelo para projetos grandes
- **Marketplace** — analistas comunitários

### Integrações

- **GitHub App** — análise automática de workflows em PRs e pushes
- **GitHub Actions** — scan de workflows, score 0-100, check runs
- **GitLab CI / CircleCI / Jenkins / Azure Pipelines** — análise de pipelines
- **LSP** — diagnóstico em tempo real para editores
- **VSCode Extension** — análise ao salvar/abrir arquivos

---

## Instalação

### npm (Recomendado)

**Uso global:**
```bash
npm install -g prometheus
prometheus --version
```

**Uso local no projeto:**
```bash
npm install --save-dev prometheus
npx prometheus --help
```

### Do Repositório

```bash
git clone https://github.com/blue-diaz/prometheus.git
cd prometheus
npm install
npm run build
npm link
```

### Docker

```bash
docker build -t prometheus .
docker run --rm -v $(pwd):/project prometheus diagnosticar
```

### Verificar Instalação

```bash
prometheus --version       # v0.10.2
prometheus --help          # lista todos os comandos
prometheus analistas --listar  # analistas disponíveis
```

---

## Configuração

O Prometheus lê um arquivo `prometheus.config.json` na raiz do seu projeto. Se não existir, usa configurações padrão sensatas.

### Configuração Mínima

```json
{
  "locale": "pt",
  "verbose": true,
  "exclude": ["node_modules/**", "dist/**", "coverage/**", ".git/**"],
  "languageSupport": {
    "javascript": { "enabled": true },
    "typescript": { "enabled": true },
    "html": { "enabled": true },
    "css": { "enabled": true }
  }
}
```

### Blocos Principais

| Bloco | Descrição |
|-------|-----------|
| `locale` | Idioma (`pt`, `en`, `zh`, `ja`) |
| `verbose` | Saída detalhada |
| `logLevel` | Nível de log (`debug`, `info`, `warn`, `error`) |
| `safeMode` | Modo seguro (sem operações destrutivas) |
| `exclude` | Glob patterns para exclusão |
| `INCLUDE_EXCLUDE_RULES` | Regras globais de inclusão/exclusão |
| `languageSupport` | Configuração por linguagem |
| `rules` | Customização de severidade por regra |
| `suppress` | Supressão de avisos por regra/caminho |
| `testPatterns` | Padrões para detecção de testes |
| `analystsExclude` | Analistas a desabilitar |
| `GUARDIAN_*` | Configurações do Guardian (assinatura GPG) |
| `REPORT_*` | Configurações de relatórios |
| `WORKER_POOL_*` | Pool de workers paralelos |
| `PLUGINS` | Sistema de plugins |
| `CACHE_STRATEGY` | Estratégia de cache |

### Variáveis de Ambiente

| Variável | Descrição | Padrão |
|----------|-----------|--------|
| `PROMETHEUS_API_PORT` | Porta da API | `3100` |
| `PROMETHEUS_API_CORS_ORIGINS` | Origens CORS | `localhost: 00` |
| `LOG_LEVEL` | Nível de log | `info` |
| `LOG_ESTRUTURADO` | Logs em JSON | `false` |
| `PROMETHEUS_DEBUG` | Modo debug | — |
| `GITHUB_APP_ID` | ID do GitHub App | — |
| `GITHUB_PRIVATE_KEY` | Chave RSA do GitHub App | — |
| `GITHUB_WEBHOOK_SECRET` | Webhook secret | — |

> 📖 [Guia de Configuração Completo →](./docs/guias/GUIA-CONFIGURACAO.md)

---

## Comandos da CLI

### Comandos Principais

| Comando | Descrição | Exemplo |
|---------|-----------|---------|
| `diagnosticar` | Análise completa do projeto | `prometheus diagnosticar --verbose` |
| `corrigir` | Auto-fix com validação | `prometheus corrigir --auto` |
| `guardian` | Monitoramento contínuo | `prometheus guardian --baseline` |
| `reestruturar` | Reorganizar estrutura do código | `prometheus reestruturar --auto` |
| `podar` | Remover código morto | `prometheus podar` |
| `fix-types` | Corrigir tipos inseguros | `prometheus fix-types --auto` |
| `formatar` | Formatação automática | `prometheus formatar --write` |
| `analistas` | Listar analistas | `prometheus analistas --listar` |

### Diagnóstico Avançado

```bash
prometheus diagnosticar --full               # análise completa
prometheus diagnosticar --fast               # modo rápido
prometheus diagnosticar --compact            # saída compacta
prometheus diagnosticar --json               # saída JSON
prometheus diagnosticar --stream             # streaming para projetos grandes
prometheus diagnosticar --executive          # relatório executivo
prometheus diagnosticar --monorepo           # análise de monorepo
prometheus diagnosticar --guardian-check     # verificar Guardian
prometheus diagnosticar --include "src/**"   # incluir padrão
prometheus diagnosticar --exclude "**/*.test.ts"  # excluir padrão
prometheus diagnosticar --path ./frontend --path ./backend  # múltiplos diretórios
```

### Correção e Manutenção

```bash
prometheus corrigir --revisar        # revisar antes de aplicar
prometheus corrigir --auto           # aplicar automaticamente
prometheus corrigir --tipo variavel-nao-usada  # tipo específico
prometheus fix-types --dry-run       # simular correção
prometheus fix-types --target src    # diretório alvo
```

### Guardian (Monitoramento com GPG)

O Guardian utiliza **assinatura GPG** (Ed25519) em vez de hashes convencionais para garantir a integridade dos arquivos. Cada baseline armazena assinaturas criptográficas individuais, permitindo autenticação e não-repúdio.

```bash
prometheus guardian                   # verificar integridade
prometheus guardian --accept-baseline # aceitar estado atual como baseline
prometheus guardian --diff            # mostrar diferenças
prometheus guardian --full-scan       # scan completo sem ignorar padrões
prometheus guardian --json            # saída JSON estruturada
```

**Como funciona:**
1. **Chaves** — um par de chaves GPG (Ed25519) é gerado automaticamente em `.prometheus/`
2. **Baseline** — cada arquivo é assinado com a chave privada; a assinatura é armazenada no baseline
3. **Verificação** — a assinatura armazenada é verificada contra o conteúdo atual usando a chave pública
4. **Drift** — arquivos adicionados, removidos ou com assinatura inválida são detectados

> As chaves GPG ficam em `.prometheus/` e são automaticamente excluídas do versionamento (`.gitignore`).

### Nomes e Renomeação

```bash
prometheus names                     # extrair nomes de variáveis
prometheus rename --apply            # aplicar renomeações
prometheus reverter listar           # listar reversões
prometheus reverter move <id>        # reverter movimento específico
```

### Licenças e Compliance

```bash
prometheus licencas scan                          # scan de licenças
prometheus licencas notices generate              # gerar THIRD-PARTY-NOTICES
prometheus licencas disclaimer add                # adicionar disclaimer
prometheus licencas disclaimer verify             # verificar disclaimers
prometheus compliance report iso-27001            # relatório ISO 27001
prometheus compliance report soc2 --json          # relatório SOC 2
```

### CI/CD

```bash
prometheus github-actions scan                    # scan de workflows
prometheus github-actions report --format html    # relatório HTML
prometheus github-actions gate --threshold 80     # quality gate
prometheus gitlab-ci scan                         # scan GitLab CI
prometheus circleci scan                          # scan CircleCI
prometheus jenkins scan                           # scan Jenkins
prometheus azure scan                             # scan Azure Pipelines
```

### Métricas e Performance

```bash
prometheus metricas                               # histórico de métricas
prometheus metricas --analistas                   # métricas por analista
prometheus perf snapshot --baseline               # baseline de performance
prometheus perf compare                           # comparar performance
```

### Utilitários

```bash
prometheus otimizar-svg --write                   # otimizar SVGs
prometheus atualizar                              # atualizar Prometheus
prometheus plugins list                           # listar plugins
prometheus marketplace search                     # buscar analistas
prometheus vulnerabilidades scan                  # scan de vulnerabilidades
prometheus formatters list                        # listar formatadores
prometheus lsp                                    # iniciar servidor LSP
prometheus serve --port 3100                      # iniciar REST API
prometheus convert                                # converter projeto
prometheus imports --scan                         # gerenciar imports
```

> 📖 [Guia de Comandos Completo →](./docs/guias/GUIA-COMANDOS.md)

---

## REST API

O Prometheus pode ser executado como um servidor HTTP RESTful para integração com outras ferramentas.

```bash
# Iniciar servidor
prometheus serve --port 3100
```

**Base URL:** `http://localhost:3100/api/v1`
**Swagger UI:** `http://localhost:3100/api/docs`

### Endpoints

| Endpoint | Método | Descrição |
|----------|--------|-----------|
| `/api/health` | GET | Health check |
| `/api/v1/analistas` | GET | Listar analistas |
| `/api/v1/analistas/stats` | GET | Estatísticas |
| `/api/v1/diagnosticar` | POST | Executar diagnóstico |
| `/api/v1/diagnosticar/jobs` | GET | Listar jobs |
| `/api/v1/diagnosticar/jobs/:id` | GET | Status do job |
| `/api/v1/guardian` | GET | Status do Guardian |
| `/api/v1/guardian/baseline` | POST | Criar baseline |
| `/api/v1/guardian/diff` | GET | Comparar baseline |
| `/api/v1/metricas` | GET | Métricas históricas |

### Exemplo

```bash
# Diagnóstico assíncrono
curl -X POST http://localhost:3100/api/v1/diagnosticar \
  -H "Content-Type: application/json" \
  -d '{"async": true, "fast": true}'
```

> 📖 [Documentação da API →](./docs/api/REST-API.md)

---

## SDK Programático

Use o Prometheus como biblioteca no seu código:

```bash
npm install prometheus
```

```typescript
import { analyzeFile, analyzeProject } from 'prometheus/sdk'

// Analisar um arquivo
const result = await analyzeFile('/caminho/arquivo.ts', conteudo)

// Analisar projeto inteiro
const projeto = await analyzeProject('/caminho/projeto')
```

```typescript
import { PrometheusSDK } from 'prometheus/sdk'

const sdk = new PrometheusSDK()
await sdk.analisarGithubActions('./.github/workflows')
sdk.registrarDetector('meu-detector', meuDetector)
```

**Subpath exports:**

| Import Path | Descrição |
|-------------|-----------|
| `prometheus` | Entry principal |
| `prometheus/sdk` | SDK programático |
| `prometheus/api` | REST API server |
| `prometheus/types` | Typescript types |
| `prometheus/bin` | CLI binary |

---

## GitHub App

O Prometheus pode operar como um **GitHub App** que analisa automaticamente workflows do GitHub Actions em PRs, pushes e issues.

### Funcionalidades

- **Análise automática** de workflows em PRs e pushes
- **Score de qualidade** 0-100 para cada workflow
- **Detecção** de script injection, secrets hardcoded, permissões excessivas
- **Comentários automáticos** em PRs e issues
- **Check runs** com annotations no código
- **Análise sob demanda** via `@prometheus analyze` em reviews
- **Scan sob demanda** via `@prometheus scan` em issues

### Eventos Suportados

| Evento | Comportamento |
|--------|---------------|
| Check Suite | Cria check run com análise completa |
| Pull Request | Posta comentário detalhado |
| Push | Análise silenciosa |
| Review | Responde a `@prometheus analyze` |
| Issues | Responde a `@prometheus scan` |

### Comandos

```bash
prometheus github-actions scan                    # scan local
prometheus github-actions report --format html    # relatório
prometheus github-actions gate --threshold 80     # quality gate
prometheus github-actions app --port 3000         # iniciar servidor
```

> 📖 [Guia do GitHub App →](./docs/guias/GITHUB-APP.md)

---

## Editor (LSP / VSCode)

### LSP Server

```bash
prometheus lsp
```

Suporta `textDocument/didOpen`, `didChange`, `didSave` para diagnóstico em tempo real.

### VSCode Extension

Uma extensão VSCode está disponível em `extensions/vscode/` com:
- Análise ao salvar (`prometheus.runOnSave`)
- Análise ao abrir (`prometheus.runOnOpen`)
- Painel de resultados
- Comandos: Analyze File, Analyze Project, Show Panel, Clear Diagnostics

> 📖 [Guia VSCode →](./docs/guias/GUIA-VSCODE.md)

---

## CI/CD

O Prometheus analisa pipelines de CI/CD para detectar problemas de segurança, performance e boas práticas.

| Plataforma | Comando |
|------------|---------|
| GitHub Actions | `prometheus github-actions scan` |
| GitLab CI | `prometheus gitlab-ci scan` |
| CircleCI | `prometheus circleci scan` |
| Jenkins | `prometheus jenkins scan` |
| Azure Pipelines | `prometheus azure scan` |

```bash
# Quality gate para CI (falha se score < 80)
prometheus github-actions gate --threshold 80 --fail-on erro
```

---

## Linguagens Suportadas

| Linguagem | Parser | Status |
|-----------|--------|--------|
| JavaScript | Babel | Nativo |
| TypeScript | Babel | Nativo |
| HTML | htmlparser2 | Nativo |
| CSS | css-tree | Nativo |
| XML | fast-xml-parser | Nativo |
| Python | Heurístico | Nativo |
| PHP | Heurístico | Nativo |
| Shell | Heurístico | Plugin |
| SQL | Heurístico | Plugin |
| Go | Heurístico | Nativo (v0.8.0) |
| Rust | Heurístico | Nativo (v0.8.0) |
| Java | java-parser | Disponível |
| Kotlin | Heurístico | Disponível |

---

## Documentação Completa

### Guias

- [Guia de Início Rápido](./docs/guias/GUIA-INICIO-RAPIDO.md) — tutorial de 10 minutos
- [Guia de Configuração](./docs/guias/GUIA-CONFIGURACAO.md) — personalizar setup
- [Guia de Comandos](./docs/guias/GUIA-COMANDOS.md) — referência completa
- [Guia Marketplace](./docs/guias/GUIA-MARKETPLACE.md) — analistas comunitários
- [Guia VSCode](./docs/guias/GUIA-VSCODE.md) — extensão VSCode
- [Guia Compliance](./docs/guias/GUIA-COMPLIANCE.md) — ISO 27001, SOC 2
- [Guia Formatters](./docs/guias/GUIA-FORMATTERS.md) — plugins de saída

### Arquitetura

- [Árvore Arquitetural](./docs/arquitetura/ARVORE-ARQUITETURAL.md) — estrutura visual
- [Type Safety](./docs/arquitetura/TYPE-SAFETY.md) — garantias de tipo
- [Segurança](./docs/arquitetura/SEGURANCA.md) — práticas de segurança
- [Sistema de Erros](./docs/arquitetura/SISTEMA-ERROS.md) — tratamento de erros

### Desenvolvimento

- [Criar Analistas](./docs/desenvolvimento/analistas/CRIAR-ANALISTA.md) — guia de 6 passos
- [Estrutura do Código](./docs/desenvolvimento/estrutura-codigo/README.md) — mapa do src/
- [Padrões de Desenvolvimento](./docs/desenvolvimento/PADROES.md) — convenções
- [Sistema de Plugins](./docs/desenvolvimento/SISTEMA-PLUGINS.md) — como estender

### Referência

- [REST API](./docs/api/REST-API.md) — documentação da API v1
- [Exemplos de Uso](./docs/exemplos/EXEMPLOS-USO.md) — casos reais e CI/CD
- [Roadmap](./docs/roadmap/ROADMAP.md) — futuro do projeto
- [Índice Completo](./docs/INDICE.md) — navegação por tópico

---

## Desenvolvimento

### Setup

```bash
git clone https://github.com/blue-diaz/prometheus.git
cd prometheus
npm install
npm run build
npm link
prometheus --version
```

### Scripts

```bash
npm run build          # compilar TypeScript
npm run typecheck      # verificar tipos
npm run lint           # ESLint
npm run test           # testes (Vitest)
npm run coverage       # cobertura de testes
npm run start          # executar Prometheus
npm run diagnosticar   # auto-análise
```

### Qualidade

- TypeScript 6.0+ — type-safe, sem `any` desnecessários
- Testes com Vitest — cobertura target > 90%
- ESLint 9.x — configuração strict
- Worker Pool — processamento paralelo
- Schema Versioning — relatórios versionados
- i18n — PT, EN, ZH, JA
- CodeQL — segurança auditada

### Padrões

- TypeScript type-safe (sem `any` ou `unknown` sem necessidade)
- Nomes descritivos em camelCase/PascalCase
- Testes com cobertura > 90%
- Sem `console.log` em produção (use sistema de mensagens)
- Imports organizados e ordenados
- Padrões arquiteturais (Registry, Strategy, Singleton)

---

## Contribuir

1. Leia [CONTRIBUTING.md](./CONTRIBUTING.md)
2. Fork o repositório
3. Crie branch: `git checkout -b feature/sua-feature`
4. Commit: `git commit -m 'Add: sua feature'`
5. Push: `git push origin feature/sua-feature`
6. Abra Pull Request

### Áreas para Contribuir

- **Bug fixes** — reportar e corrigir bugs
- **Novas features** — analistas, comandos, plugins
- **Documentação** — guias, exemplos
- **Testes** — aumentar cobertura
- **Analisadores** — criar detectores e plugins

> 📖 [Guia: Como Criar Analistas →](./docs/desenvolvimento/analistas/CRIAR-ANALISTA.md)

---

## Licença

Prometheus é licenciado sob **MIT-0** (MIT No Attribution).

- Sem restrições de uso, modificação ou distribuição
- Sem atribuição necessária (mas apreciada!)
- Sem garantias

Dependências de terceiros listadas em [THIRD-PARTY-NOTICES.txt](./THIRD-PARTY-NOTICES.txt).

---

<div align="center">

[![npm](https://img.shields.io/badge/npm-prometheus-CB3837)](https://www.npmjs.com/package/prometheus)
[![GitHub](https://img.shields.io/badge/github-blue--diaz/prometheus-181717)](https://github.com/blue-diaz/prometheus)
[![Issues](https://img.shields.io/badge/issues-report-FF4500)](https://github.com/blue-diaz/prometheus/issues)
[![Releases](https://img.shields.io/badge/releases-latest-2ea44f)](https://github.com/blue-diaz/prometheus/releases)

**Feito para os iniciantes** — [Voltar ao topo](#prometheus)

</div>
# prometeus

