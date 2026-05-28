---
Proveniência e Autoria: Este documento integra o projeto Prometheus (licença MIT-0).
---

# Roadmap - Prometheus

## Versão Atual: 0.10.0

Planejamento estratégico do desenvolvimento do Prometheus, incluindo features implementadas, melhorias e visão de longo prazo.

---

## Versões Concluídas

### v0.5.0 - Sistema de Analistas Extensível (Q1 2026)

**Objetivo:** Permitir que usuários criem e compartilhem analistas personalizados.

- [x] Interface pública para criar analistas customizados
- [x] Sistema de plugins com autodiscovery
- [x] Validação automática de analistas
- [x] Suporte a GitHub Actions com detecção de segurança
- [x] Documentação completa para criação de analistas
- [x] Analistas multi-linguagem (React, CSS, HTML, Python, Shell, SQL, XML)

---

### v0.6.0 - Expansão de Detecções e Relatórios (Q1 2026)

**Objetivo:** Ampliar cobertura de análise e formatos de relatório.

- [x] Expansão de detecções de código
- [x] Relatórios multi-formato (JSON, Markdown, HTML)
- [x] CLI otimizada para GitHub Actions
- [x] Sistema de scoring adaptativo
- [x] Worker Pool para processamento paralelo

---

### v0.7.0 - Análise + API REST + SDK (Q2-Q3 2026)

**Objetivo:** Integração com análise avançada, API programática e ecossistema GitHub.

- [x] Métricas automáticas calculadas do projeto real
- [x] API REST completa com Swagger UI
- [x] SDK programático para uso como biblioteca
- [x] Dashboard web para visualização de resultados
- [x] Analista avançado de GitHub Actions com plugins
- [x] Sistema de scoring para workflows CI/CD
- [x] GitHub App para análise automática em PRs
- [x] Baseline e comparação de performance (`perf`)
- [x] Histórico de métricas com tendência
- [x] Jobs assíncronos na API para análises longas
- [x] Rate limiting e CORS na API
- [x] Suporte a CI/CD: GitHub Actions, GitLab CI, CircleCI, Jenkins, Azure Pipelines

---

### v0.8.0 - Marketplace, Multi-linguagem e Performance (Q2-Q4 2026)

**Objetivo:** Ecossistema comunitário de analistas, suporte a linguagens adicionais e otimizações.

#### Marketplace

- [x] Marketplace de analistas (registro local + busca remota)
- [x] Sistema de versionamento para analistas externos (semver)
- [x] CLI commands: search, install, remove, list, info, publish
- [x] API REST: `/api/v1/marketplace/*` endpoints

#### Multi-linguagem

- [x] Suporte a Go (analistas: go, seguranca, concorrencia, recursos, boas-praticas)
- [x] Suporte a Rust (analistas: rust, seguranca, memoria, concorrencia, boas-praticas)
- [x] Suporte a Java/Kotlin (analistas: java, seguranca, excecoes, boas-praticas)
- [x] PHP avançado com parser dedicado

#### Performance e Infra

- [x] AnalysisCache persistente entre execuções
- [x] Worker Pool configurável (maxWorkers, batchSize)
- [x] Suporte a múltiplos projetos simultâneos (flag --path)
- [x] Análise de vulnerabilidades em dependências
- [x] SDK programático exposto via `prometheus/sdk` (analyzeFile, analyzeProject)
- [x] Otimizações de performance:
  - [x] Eliminação de fs.stat() redundante no preparo de AST
  - [x] Hash consistente (XXH) entre executor e analysis-cache
  - [x] Paralelismo real entre projetos (Promise.all)
- [ ] Parsing lazy sob demanda
- [x] Cache de AST entre analistas no mesmo escopo

---

## Versões Concluídas

### v0.9.0 - Performance e Ferramentas de Desenvolvimento (Q1 2027)

**Objetivo:** Otimização profunda de performance e ferramentas para desenvolvedores.

#### Performance

- [x] Worker pool como modo padrão (fast automático quando > N arquivos)
- [x] Parsing lazy: só parsear arquivos quando um analista realmente precisar
- [x] AST Cache compartilhado entre analistas no mesmo escopo
- [x] Análise incremental otimizada com diff estrutural
- [x] Memory pooling para redução de GC pause
- [x] Parallel AST parsing por worker
- [x] Streaming de relatórios para projetos grandes

#### Ferramentas de Desenvolvimento

- [x] VSCode Extension com análise em tempo real
- [x] LSP (Language Server Protocol) integration
- [x] Docker image oficial publicada no Docker Hub

---

### v0.10.0 - Estabilidade e Enterprise (Q2-Q3 2027)

**Objetivo:** Maturidade enterprise, estabilidade de API e conformidade.

- [x] Streaming de relatórios para projetos grandes
- [x] API pública consolidada e estável (v1)
- [x] Machine Learning para detecção de bugs
- [x] Suporte a monorepos avançado
- [x] Relatórios de compliance (ISO, SOC2)
- [x] Modo server headless

---

## Estado Geral do Projeto

| Feature                            | Status          | ETA    |
| ---------------------------------- | --------------- | ------ |
| VSCode Extension                   | ✅ Implementado  | v0.9   |
| LSP Integration                    | ✅ Implementado  | v0.9   |
| Performance Optimizations          | ✅ Completo      | v0.9   |
| ├─ fs.stat() redundante            | ✅ Implementado  | v0.9   |
| ├─ Hash SHA256 → XXH no cache      | ✅ Implementado  | v0.9   |
| ├─ Paralelismo multi-projeto       | ✅ Implementado  | v0.9   |
| ├─ Worker pool como padrão         | ✅ Implementado  | v0.9   |
| ├─ Parallel AST Parsing por Worker | ✅ Implementado  | v0.9   |
| ├─ Parsing Lazy                    | ✅ Implementado  | v0.9   |
| ├─ Memory Pooling                  | ✅ Implementado  | v0.9   |
| └─ Differential Analysis           | ✅ Implementado  | v0.9   |
| Docker Image Publicada             | ✅ Implementado  | v0.9   |
| AST Cache Compartilhado            | ✅ Implementado  | v0.9   |
| Compliance Reports (ISO, SOC2)     | ✅ Implementado  | v0.10  |
| Plugin System para Formatters      | ✅ Implementado  | v0.10  |
| Runtime Abstraction (Deno/Bun)     | ✅ Implementado  | v0.10  |
| Machine Learning                   | ✅ Implementado  | v0.10  |
| Suporte a Monorepos                | ✅ Implementado  | v0.10  |
| API Pública Consolidada (v1)       | ✅ Implementado  | v0.10  |
| Modo Server Headless               | ✅ Implementado  | v0.10  |
| Streaming de Relatórios            | ✅ Implementado  | v0.10  |

---

## Ideias em Discussão

- [x] IDE integrations (VSCode, IntelliJ)
- [x] Docker image oficial publicada
- [x] Análise de dependências de terceiros com vulnerabilidades
- [x] Machine Learning para detecção de bugs
- [x] LSP (Language Server Protocol) integration
- [x] Suporte a deno/bun runtime (abstração inicial)
- [x] Plugin system para formatters customizados

---

## Feedback e Sugestões

Tem uma ideia? Abra uma [issue no GitHub](https://github.com/blue-diaz/prometheus/issues) ou participe das discussões!

**Critérios** para features serem consideradas:
1. Alinhamento com visão do projeto
2. Potencial de impacto
3. Complexidade de implementação
4. Demanda da comunidade
5. Recursos disponíveis
