---
Proveniência e Autoria: Este documento integra o projeto Prometheus (licença MIT-0).
---


# Changelog

Todas as mudanças notáveis deste repositório serão documentadas aqui.

O formato segue [Keep a Changelog](https://keepachangelog.com/pt-BR/1.0.0/),
e este projeto adere ao [Versionamento Semântico](https://semver.org/lang/pt-BR/).

## [0.9.0] - 2026-05-24

### Adicionado

- **LSP (Language Server Protocol) Integration**
  - Novo comando `prometheus lsp` para iniciar servidor LSP via stdio
  - Suporte a `textDocument/didOpen`, `didChange`, `didSave` com diagnósticos em tempo real
  - Baseado em `vscode-languageserver` e SDK do Prometheus

- **Docker Image Oficial**
  - Dockerfile otimizado com multi-stage build e `USER prometheus`
  - Workflow CI para build e push automático para GitHub Container Registry (GHCR)
  - Imagem publicada em releases com tags semver

- **Parallel AST Parsing por Worker**
  - Parsing de AST distribuído entre Worker Threads
  - Workers recebem apenas conteúdo bruto e parseiam localmente
  - Elimina overhead de structured clone de ASTs complexos

### Melhorado

- **Parsing Lazy**
  - ASTs são parseados sob demanda via `ScopeAstCache` apenas quando um analista realmente precisa
  - `prepararComAst()` removido do fluxo principal — economia de CPU em projetos grandes
  - Cache global (process-level) preservado para reaproveitamento entre execuções

- **Análise Incremental com Diff Estrutural**
  - Nova função `calcularHashEstrutural()` extrai assinatura apenas de nós estruturais (imports, funções, classes, interfaces, tipos)
  - Fallback baseado em regex quando AST não está disponível
  - Mudanças de formatação/comentários não invalidam o cache incremental
  - Campo `hashEstrutural` adicionado ao estado incremental

- **Memory Pooling**
  - Cache de supressões inline por arquivo (`extrairSupressoes`) — evita re-parsing a cada analista
  - Hoist do Map `resultadosOtimizados` no executor — reduz alocações por arquivo
  - Pool genérico `ObjectPool<T>` para reuso de objetos

- **Worker Pool**
  - Stripping de ASTs antes da serialização para workers (structured clone mais leve)

## [0.10.7] - 2026-05-28

### Adicionado

- **Geração de Barrels Estruturados e Organizados**
  - Novo comando `prometheus barrels` para gerenciar barrels (index.ts) do projeto
  - `prometheus barrels --scan` — varre o projeto e mostra quais barrels seriam criados/atualizados (dry-run)
  - `prometheus barrels --generate` — cria e atualiza barrels com exports organizados
  - Classificação automática de `type` vs `value` exports para compatibilidade com `verbatimModuleSyntax`
  - Detecção de conflitos (múltiplos arquivos exportando o mesmo nome)
  - Preservação de barrels "curados manualmente" (com código customizado)
  - Reorganização automática de barrels após movimentação de arquivos

### Alterado

- **Reestruturação Mudou de Modificação Ativa para Sugestões**
  - `reestruturar` agora gera apenas **sugestões** de reorganização — o usuário decide conscientemente o que aplicar
  - A lógica ativa de reestruturação de arquitetura foi removida por ser extremamente complexa e por cada projeto ter suas peculiaridades
  - As sugestões são salvas em arquivo JSON para revisão manual
  - O fluxo de `diagnosticar` passou a incluir sugestões estruturais não-destrutivas

### Infra

- Versão bump: `0.10.6` → `0.10.7`

## [0.10.0] - 2026-05-24

### Adicionado

- **Streaming de Relatórios para Projetos Grandes**
  - Nova classe `RelatorioStream` com gravação incremental em shards NDJSON
  - Compressão gzip por shard com manifesto de índice (`stream-manifest.json`)
  - Rotação automática por contagem de ocorrências ou bytes
  - `pushOcorrencia()` injetado no executor via callback para coleta em tempo real
  - Configs: `REPORT_STREAM_ENABLED`, `REPORT_STREAM_AUTO_THRESHOLD`, `REPORT_STREAM_MAX_OCCURRENCES_PER_SHARD`, `REPORT_STREAM_MAX_BYTES_PER_SHARD`, `REPORT_STREAM_COMPRESS`
  - Flag `--stream` no CLI `diagnosticar`

- **Suporte a Monorepos Avançado**
  - Detector automático de monorepos: pnpm-workspace.yaml, lerna.json, nx.json, turbo.json, npm/yarn workspaces
  - Análise cross-package: versões inconsistentes, dependências internas faltando
  - Flag `--monorepo` no CLI com auto-descoberta de workspaces
  - Registrado como detector global no registry

- **Machine Learning para Detecção de Bugs**
  - 8 padrões de bug com scoring estatístico: null pointer, memory leak, race condition, type assertion insegura, unhandled promise, deep nesting, magic numbers, comparação insegura
  - Extração de features (complexidade ciclomática, aninhamento, uso de `any`, etc.)
  - Probabilidade Bayesiana de bugs por arquivo com confidence scoring (0-100%)
  - Registrado como detector no registry

- **API Pública Consolidada e Estável (v1)**
  - `src/index.ts` — entry point programático unificado
  - Subpath exports estáveis no package.json: `". "`, `"./sdk"`, `"./api"`, `"./types"`, `"./bin"`
  - `analyzeFile`, `analyzeProject`, `iniciarServidorApi` como API pública

- **Modo Server Headless**
  - Novo comando `prometheus serve` para iniciar servidor HTTP REST API
  - Flags: `--port`, `--host`, `--cors-origin`, `--daemon`
  - Graceful shutdown em SIGTERM/SIGINT

### Corrigido

- `swagger-jsdoc` com `require()` em módulo ESM — convertido para `import()` dinâmico (`src/api/services/swagger-config.ts`)
- Adicionados aliases `@api` e `@api/server` no tsconfig.json

### Infra

- Versão bump: `0.9.0` → `0.10.0`

---

## [0.8.0] - 2026-05-20

### Adicionado

- **Marketplace de Analistas**
  - Registro local e busca remota de analistas comunitários
  - Sistema de versionamento semver para analistas externos
  - CLI commands: `search`, `install`, `remove`, `list`, `info`, `publish`
  - API REST: `/api/v1/marketplace/*`

- **Suporte Multi-linguagem**
  - Go: analistas para código, segurança, concorrência, recursos, boas práticas
  - Rust: analistas para código, segurança, memória, concorrência, boas práticas
  - Java/Kotlin: analistas para código, segurança, exceções, boas práticas
  - PHP avançado com parser dedicado

### Melhorado

- **Performance e Infraestrutura**
  - `AnalysisCache` persistente entre execuções com TTL configurável
  - Worker Pool configurável (`maxWorkers`, `batchSize`)
  - Suporte a múltiplos projetos simultâneos (flag `--path`)
  - Análise de vulnerabilidades em dependências
  - SDK exposto via `prometheus/sdk` (`analyzeFile`, `analyzeProject`)
  - Eliminação de `fs.stat()` redundante no preparo de AST
  - Hash consistente (XXH) entre executor e analysis-cache
  - Paralelismo real entre projetos (`Promise.all`)
  - Cache de AST entre analistas no mesmo escopo (`ScopeAstCache`)

## [0.7.0] - 2026-05-18

### Adicionado

- **Análise Assistida por IA**
  - Integração com modelos de IA para sugestões de refatoração
  - Detecção de padrões avançada com IA
  - Recomendações automáticas de melhoria de código

- **Suporte Completo a Supressão Inline em Markdown**
  - `<!-- @prometheus-disable regra1 regra2 -->` para supressão em bloco
  - `<!-- @prometheus-disable-next-line regra -->` para supressão de linha única
  - `<!-- @prometheus-disable-all -->` para suprimir todas as regras
  - Funciona com `detector-markdown` e `analista-formatador`
  - Compatibilidade com padrão legado `<!-- prometheus-ignore: regra -->`

### Melhorado

- **GitHub Actions**
  - Análise estruturada com YAML AST
  - Sistema de scoring (0-100) para workflows
  - Detectores de segurança, performance e boas práticas

- **Métricas Automáticas**
  - Cálculo automático de scores baseado na estrutura real
  - Histórico de tendências com dados persistentes

- **SDK Programático**
  - `PrometheusSDK.analisarGithubActions()` para análise programática
  - `PrometheusSDK.analisarRepositorio()` para análise global
  - `PrometheusSDK.registrarDetector()` para plugins customizados

### Corrigido

- Detecções falsas positivas reduzidas com análise contextual
- Tratamento de erros aprimorado em workflows malformados

## [0.6.0] - 2026-04-13

### Adicionado

- **Análise Avançada de GitHub Actions**
  - Detecção aprimorada de problemas de segurança, performance e boas práticas
  - Análise estruturada com YAML AST para detecções mais precisas
  - Sistema de scoring (0-100) para workflows individuais

- **Métricas Automáticas do Projeto**
  - Cálculo automático de scores baseado na estrutura real do projeto
  - Histórico de tendências com dados persistentes

### Melhorado

- **SDK Programático**
  - `PrometheusSDK.analisarGithubActions()` para análise programática de workflows
  - `PrometheusSDK.analisarRepositorio()` para análise global do repositório
  - `PrometheusSDK.registrarDetector()` para registro de plugins customizados

- **Performance**
  - Processamento paralelo de detectores
  - Cache de análise para workflows não modificados

### Corrigido

- Detecções falsas positivas reduzidas com análise contextual
- Tratamento de erros aprimorado em workflows malformados

## [0.5.0] - 2026-03-15

### Adicionado

- Sistema de plugins para GitHub Actions com detecção de segurança e boas práticas
- Análise avançada de workflows com detecção de anti-padrões
- Integração com SDK para análise programática

### Melhorado

- Performance do scanner de arquivos otimizada com processamento paralelo
- Precisão dos detectores de código frágil e vazamentos de memória

## [0.4.3] - 2026-02-20

### Adicionado

  - **Gerenciamento de Nomes de Variáveis**
  - Novo comando `names`: Varre o projeto e extrai nomes de variáveis para mapeamento em `names/name.txt`.
  - Novo comando `rename`: Aplica renomeações de variáveis em massa baseadas no arquivo de mapeamento.
  - Script automatizado para sugestão de traduções (Português) para nomes de variáveis.

### Alterado

- **Segurança de Tipagem**
  - Refinamento massivo de tipos `any` e `unknown` em todo o core e CLI via `fix-types`.
  - Melhoria na inferência de tipos em callbacks assíncronos e interfaces de plugins.
- **Robustez do Renomeador**
  - Implementação de lista de nomes protegidos para evitar renomeação acidental de propriedades nativas (Node.js/JS).
  - Ajuste no gerador de código para evitar erros de sintaxe em casts do TypeScript.

### Corrigido

- **Erros de Compilação** Correção de erros de sintaxe gerados pelo Babel em `analista-html.ts`, `detector-markdown.ts` e `processamento-diagnostico.ts`.
- **CLI** Resolução do erro `ERR_INVALID_MODULE_SPECIFIER` na execução global do pacote via ESM loader.

## [0.3.9] - 2026-02-19

### Adicionado

- **Gerenciamento** de Nomes de Variáveis*
  - Novo comando `names`: Varre o projeto e extrai nomes de variáveis para mapeamento em `names/name.txt`.
  - Novo comando `rename`: Aplica renomeações de variáveis em massa baseadas no arquivo de mapeamento.
  - Script automatizado para sugestão de traduções (Português) para nomes de variáveis.

### Alterado

- **Segurança** de *Tipagem*
  - Refinamento massivo de tipos `any` e `unknown` em todo o core e CLI via `fix-types`.
  - Melhoria na inferência de tipos em callbacks assíncronos e interfaces de plugins.
- **Robustez** do *Renomeador*
  - Implementação de lista de nomes protegidos para evitar renomeação acidental de propriedades nativas (Node.js/JS).
  - Ajuste no gerador de código para evitar erros de sintaxe em casts do TypeScript.

### Corrigido

- **Erros** de Compilação* Correção de erros de sintaxe gerados pelo Babel em `analista-html.ts`, `detector-markdown.ts` e `processamento-diagnostico.ts`.
  - *CLI* Resolução do erro `ERR_INVALID_MODULE_SPECIFIER` na execução global do pacote via ESM loader.

## [0.3.8] - 2026-02-18

### Adicionado

- **Manutenção** Documentação e CHANGELOG atualizados para refletir o estado real do projeto.
- **Refatoração** Pequenas melhorias e correções de bugs.

## [0.3.6] - 2026-01-23

### Adicionado

- **Extração Avançada de Sinais** Sistema inteligente de análise de sinais do projeto para reestruturação mais precisa
  - Detecção de padrões arquiteturais e tecnologias dominantes
  - Análise de complexidade estrutural para decisões estratégicas
  - Ajuste contextual de destinos baseado em sinais detectados
  - Integração com `OperarioEstrutura` para planejamento estratégico aprimorado

- **Validações Aprimoradas** Melhorias em plugins de análise para CSS, HTML e XML
  - Validações de qualidade de código e acessibilidade
  - Verificações de segurança aprimoradas
  - Mensagens de validação expandidas

- **Sistema de Pontuação Contextual** Mecanismo de pontuação que considera sinais avançados
  - Ajustes contextuais baseados na análise de projeto
  - Pontuação mais precisa para diferentes tipos de projeto

### Alterado

- **Configuração Lint-Staged** Otimização da configuração para melhor legibilidade e performance
- **Mapeamento de Reversão** Melhorias na organização de importações e consistência

### Corrigido

- **Segurança HTML** Correção de expressão regular vulnerável na filtragem HTML (Code Scanning Alert #47)
- **Validações de Segurança** Reforço das verificações de segurança em análise de HTML

### Interno

- **Tipos Expandidos** Novos campos em `SinaisProjetoAvancados` para detecção de padrões e tecnologias
- **Mensagens de Plugin** Expansão das mensagens de validação para melhor feedback

## [0.3.5] - 2026-01-23

## [0.3.4] - 2026-01-23

### Adicionado

- **Manutenção** Documentação e CHANGELOG atualizados para refletir o estado real do projeto.
- **Refatoração** Pequenas melhorias e correções de bugs.

## [0.3.3] - 2026-01-21

### Adicionado

- Melhoria na estabilidade da saída JSON do comando `diagnosticar` para integração em CI.
- Testes adicionais e validações em analistas que processam arquivos Markdown.

### Alterado

- Redução de falsos positivos em deteções de async não tratado em fluxos assíncronos.

### Corrigido

- Tratamento de erros assíncronos em `mapa-reversao` e correções menores em logs e mensagens.

## [0.3.2] - 2026-01-19

### Adicionado

- Script `md: add-disclaimer` para inserir aviso de proveniência em arquivos Markdown.
- Utilitários de manutenção para geração e verificação de relatórios.

### Alterado

- Melhorias na geração de relatórios e pequenas otimizações de performance.

### Corrigido

- Ajustes em documentação e comentários internos; correções de pequenos bugs em scripts.

## [0.3.1] - 2026-01-17

### Adicionado

- Baseline de testes e ajustes iniciais na configuração do CI para validação de analistas.

### Corrigido

- Correções em flags do CLI (`--export`, `--json`) e comportamento de exportação de relatórios.

## [0.3.0] - 2026-01-15

### Adicionado

- **Novos Comandos CLI**
  - `formatar` - Aplica formatação estilo Prometheus (whitespace, seções, finais de linha) com suporte a Prettier
  - `otimizar-svg` - Otimiza SVGs do projeto usando otimizador interno (svgo-like)
  - `atualizar` - Atualiza o Prometheus verificando integridade via Guardian antes
  - `reverter` - Gerencia mapa de reversão para moves aplicados (listar, arquivo, move, limpar)

- **Novos Analistas e Detectores**
  - `detector-arquitetura` - Análise de padrões arquiteturais
  - `detector-codigo-fragil` - Identifica código frágil e propenso a bugs
  - `detector-construcoes-sintaticas` - Detecta construções sintáticas problemáticas
  - `detector-duplicacoes` - Identifica código duplicado
  - `detector-interfaces-inline` - Detecta interfaces inline que deveriam ser extraídas
  - `detector-contexto-inteligente` - Análise contextual avançada
  - `detector-fantasmas` - Detecta arquivos órfãos e não utilizados
  - `detector-performance` - Identifica problemas de performance

- **Plugins Multi-linguagem**
  - `analista-react` e `analista-react-hooks` - Análise específica para React
  - `analista-tailwind` - Análise de classes Tailwind
  - `analista-css` e `analista-css-in-js` - Análise de estilos
  - `analista-html` e `analista-xml` - Análise de markup
  - `analista-svg` - Análise e otimização de SVGs
  - `analista-python` - Suporte heurístico para Python
  - `detector-markdown` - Validação de arquivos Markdown
  - `detector-documentacao` - Análise de qualidade de documentação

- **Sistema de Supressão Inline** `@prometheus-disable-next-line <regra>` funciona para todos os analistas

- **Modos de Execução Expandidos**
  - `--executive` - Modo executivo (apenas problemas críticos/alta prioridade)
  - `--compact` - Modo compacto (consolida progresso e mostra o essencial)
  - `--trust-compiler` - Confia no compilador para reduzir falsos positivos

- **Suporte a Mais Linguagens** PHP, Python (heurístico)

### Alterado

- **Node.js 24+** Agora requer Node.js ≥ 24.0.4
- **Refatoração do Sistema de Mensagens** Mensagens centralizadas por domínio
- **Melhoria no Sistema de Pontuação** Ajustes contextuais mais precisos
- **Guardian** Integração mais profunda com comandos de atualização

### Corrigido

- Normalização de line endings para compatibilidade Windows/Linux
- Falsos positivos em detecção de segredos em arquivos de documentação
- Tratamento de placeholders em strings de configuração

### Removido

- Dependência circular interna em `prometheus: file: prometheus-0.3.0.tgz` (problema de empacotamento)

---

## [0.2.0] - 2025-08-28

### Adicionado

- **Pool de Workers** Sistema completo de paralelização por arquivo para melhorar performance em projetos grandes
  - Classe WorkerPool com gerenciamento de workers paralelos
  - Sistema de lotes configurável (batchSize padrão: 10 arquivos por worker)
  - Timeout individual por analista (30s padrão) com cancelamento automático
  - Fallback automático para processamento sequencial quando workers desabilitados
  - Worker executor em JavaScript puro para threads separadas
  - Configuração centralizada via variáveis de ambiente
  - Função de conveniência `processarComWorkers()` para fácil integração
  - Estatísticas detalhadas do pool (workers ativos, erros, duração)
  - Testes completos com 9 cenários cobrindo configuração e processamento

- **Sistema de Schema Versioning** Versionamento completo dos relatórios JSON
  - Metadados de versão (`_schema`) em todos os relatórios JSON
  - Validação automática de schema com compatibilidade backward
  - Migração automática de relatórios legados
  - Utilitários para leitura de relatórios versionados
  - Integração com `gerador-relatorio.ts` e `relatorio-arquetipos.ts`
  - Testes completos (27 testes passando)

- **Sistema de Pontuação Adaptativa** Pontuação inteligente baseada no tamanho do projeto
  - Constantes adaptativas baseadas em número de arquivos e diretórios
  - Sistema de fatores escaláveis (1x a 5x) para diferentes tamanhos de projeto
  - Configuração centralizada em `configuracao-pontuacao.ts` com 3 modos
  - Pesos de arquétipo recalibrados para maior realismo
  - Sistema de confiança inteligente com ajustes contextuais

- **Correção Crítica** Exclusão padrão de `node_modules` no comando `diagnosticar`
  - Aplicação automática de padrões de exclusão padrão quando nenhum filtro é especificado
  - Redução de ~70% nos arquivos escaneados (2111 → 633 arquivos)
  - Manutenção da compatibilidade com filtros explícitos
  - Validação através de testes específicos

### Alterado

- **Correção de Exclusão Padrão** Comando `diagnosticar` agora aplica corretamente padrões de exclusão padrão (`node_modules/**`, `dist/**`, `coverage/**`, etc.) quando nenhum filtro explícito é fornecido
- **Timeout por Analista** Implementado timeout individual de 30 segundos por analista com cancelamento automático

### Corrigido

- **Problema de Exclusão** Correção crítica onde `node_modules` era escaneado mesmo sem filtros explícitos devido a configuração vazia de `CLI_EXCLUDE_PATTERNS`

## [0.1.0] - 2025-08-18

### Adicionado

- CLI inicial com comandos: `diagnosticar`, `guardian`, `podar`, `analistas`, `perf`.
- Biblioteca inicial de analistas (padrões de uso, funções longas, TODOs, estrutura, dependências).
- Guardian com baseline e diffs; saída `--json`.

### Infra

- CI: lint, typecheck, testes, cobertura e gates.
- Licenças e avisos de terceiros; scripts utilitários.

---
