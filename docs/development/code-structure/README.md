<!-- @prometheus-disable pontuacao-repetida -->
---
Proveniência e Autoria: Este documento integra o projeto Prometheus (licença MIT-0).
---


# Estrutura de Código - Prometheus

Guia detalhado da organização e estrutura do código do Prometheus.

---

## Organização Principal

```
blue-diaz/
├── src/                    # Código-fonte principal
├── tests/                  # Testes unitários e integração
├── docs/                   # Documentação
├── scripts/                # Utilitários e ferramentas
└── [config files]          # Configuração do projeto
```

---

## [ALVO] `src/` - Código-Fonte

### Visão Geral

```
src/
├── bin/                    # CLI Entry points
├── cli/                    # Comandos CLI
├── core/                   # Núcleo do sistema
├── analistas/              # Detectores/Analisadores
├── relatorios/             # Gerador de relatórios
├── shared/                 # Código compartilhado
├── guardian/               # Monitor de saúde
├── licensas/               # Gerenciamento de licenças
├── types/                  # Definições TypeScript
├── zeladores/              # Executores de ações
├── api/                    # Servidor REST API + Swagger
├── sdk/                    # SDK programático
├── app/                    # Aplicação (dashboard web)
└── node.loader.ts          # Node.js module loader
```

---

## [DOCUMENTO] `src/bin/` - Pontos de Entrada

**Responsabilidade:** PROTECTED_1 Inicializar a aplicação

```typescript
// bin/index.ts - Entrada principal
// Função: Determinar qual CLI executar

// bin/cli.ts - CLI Principal
// Função: Processar argumentos e delegar a handlers
```

**Fluxo:** PROTECTED_1
```
node prometheus
  ↓
bin/index.ts
  ↓
bin/cli.ts (parse args)
  ↓
cli/handlers/
```

---

## [CONFIG] `src/cli/` - Interface de Linha de Comando

**Responsabilidade:** PROTECTED_1 Processar entrada do usuário

```
cli/
├── comandos.ts             # Registrar todos comandos
├── options-*.ts            # Opções específicas de comando
├── processamento-*.ts      # Processamento de entrada
├── commands/               # Implementação de comandos
│   ├── diagnosticar.ts
│   ├── corrigir.ts
│   ├── guardian.ts
│   └── ...
├── handlers/               # Handlers de cada comando
├── helpers/                # Helpers de CLI
├── options/                # Parser de opções
└── processing/             # Processamento pós-parse
```

### Fluxo de Comando

```
Usuario: prometheus diagnosticar --foo bar
         ↓
    cli.ts (yargs)
         ↓
   commands/diagnosticar.ts
         ↓
   handlers/diagnosticar-handler.ts
         ↓
   core/execution/ ...
         ↓
   Resultado
```

---

## `src/core/` - Núcleo do Sistema

**Responsabilidade:** PROTECTED_1 Lógica central, processamento

```
core/
├── config/                 # Gerenciamento de configuração
│   ├── config.ts
│   ├── loader.ts
│   └── validator.ts
├── execution/              # Orquestração de análise
│   ├── executor.ts
│   ├── context.ts
│   └── results.ts
├── messages/               # Sistema de mensagens (i18n)
│   ├── index.ts           # Entry point centralizado
│   ├── diagnostico-messages.ts
│   ├── fix-types-messages.ts
│   └── ...
├── parsing/                # Análise de arquivos
├── registry/               # Registro de componentes
├── reporting/              # Geração de relatórios
├── schema/                 # Validação de schema
├── utils/                  # Utilitários gerais
└── workers/                # Workers para paralelismo
```

### `config/` - Configuração

```typescript
// config.ts
export interface ConfiguracaoProjeto {
  nome: string;
  caminhos: string[];
  analistas: { [nome: string]: ConfigAnalista };
  relatorios: ConfigRelatorio;
  autoFix: ConfigAutoFix;
}

// Loader - carregar de prometheus.config.json
// Validator - validar schema
```

### `execution/` - Executor

```typescript
// Orquestra análise de múltiplos arquivos
class ExecutorAnalise {
  async executar(config: ConfiguracaoProjeto): Promise<ResultadoAnalise>

  // 1. Carregar arquivos
  // 2. Descobrir analistas
  // 3. Executar análise (paralelo/serial)
  // 4. Agregar resultados
  // 5. Retornar
}
```

### `messages/` - Sistema de Mensagens

```
messages/
├── pt/
│   ├── cli.json           # Mensagens CLI
│   ├── analistas.json     # Descrição analistas
│   ├── erros.json         # Mensagens de erro
│   └── ...
├── en/
│   ├── cli.json
│   └── ...
├── zh/                     # 中文
├── ja/                     # 日本語
├── loader.ts              # Carregar por idioma
└── formatter.ts           # Formatar mensagens
```

**Uso:** PROTECTED_1
```typescript
const msg = messages.obter('cli.diagnosticar.descricao');
// Retorna: "Diagnosticar o projeto ..."
```

### `parsing/` - Análise de Código

```typescript
// AST (Abstract Syntax Tree)
// Converte código em estrutura processável

const ast = parseAST(codigo, linguagem);
// Para JS/TS: Babel/TypeScript parser
// Para HTML: Parse5
// etc.
```

### `registry/` - Descoberta de Componentes

```typescript
class RegistroAnalistas {
  // Registra analistas dinamicamente
  registrar(nome: string, classe: typeof Analista): void
  obter(nome: string): typeof Analista
  listar(): string[]

  // Descobrir plugins: scan de arquivos
  descobrir(caminhoPlugins: string): Promise<void>
}
```

### `reporting/` - Relatórios

```
Tipos suportados:
- JSON: Estruturado, para processamento
- Markdown: Legível, para docs
- HTML: Visual, para navegador
- CSV: Tabular, para planilhas
```

---

## [BUSCA] `src/analistas/` - Detectores

**Responsabilidade:** PROTECTED_1 Detectar problemas específicos

```
analistas/
├── README.md              # Guia dos analistas
├── tipos.ts               # Interfaces comuns
├── arquitetos/            # Análise arquitetural (architects/)
├── architects/            # Detectores de padrões arquiteturais
├── detectores/            # Detecção de problemas
│   ├── detector-codigo-fragil.ts
│   ├── detector-duplicacoes.ts
│   ├── detector-performance.ts
│   ├── detector-seguranca.ts
│   └── ...
├── strategists/           # Análise de estratégia
├── pontuadores/           # Scoring
├── js-ts/                 # Detectores JS/TS
├── plugins/               # Plugins customizados
├── registry/              # Registro de analistas
├── corrections/           # Sistema de correções automáticas
├── converters/            # Conversores entre formatos
│── github-actions/        # Analista de GitHub Actions
├── gitlab-ci/             # Analista de GitLab CI
├── circleci/              # Analista de CircleCI
├── jenkins/               # Analista de Jenkins
├── azure-pipelines/       # Analista de Azure Pipelines
├── react/                 # Analista React
├── html/                  # Analista HTML
├── css/                   # Analista CSS
├── css-in-js/             # Analista CSS-in-JS
├── tailwind/              # Analista Tailwind
├── python/                # Analista Python
├── shell/                 # Analista Shell
├── sql/                   # Analista SQL
├── xml/                   # Analista XML
└── svg/                   # Analista SVG
```

### Interface Padrão

Todos analistas implementam:

```typescript
interface Analista {
  nome: string;
  descricao: string;
  linguagens: string[];

  analisar(
    arquivo: Arquivo,
    conteudo: string,
    config?: ConfigAnalista
  ): ResultadoOcorrencia[];
}
```

### Exemplo: Detector Código Frágil

```typescript
// src/analistas/detectores/fragilidade.ts

export class DetectorCodigoFragil implements Analista {
  nome = 'CodigoFragil';
  descricao = 'Detecta padrões de código frágil';
  linguagens = ['typescript', 'javascript'];

  analisar(arquivo: Arquivo, conteudo: string): ResultadoOcorrencia[] {
    // 1. Parse AST
    const ast = parseAST(conteudo, 'typescript');

    // 2. Buscar padrões (ex: funções muito longas)
    // 3. Criar ocorrências

    return ocorrencias;
  }
}
```

---

## [GRAFICO] `src/relatorios/` - Relatórios

**Responsabilidade:** PROTECTED_1 Formatar e apresentar resultados

```
relatorios/
├── gerador-relatorio.ts        # Orquestrador
├── filtro-inteligente.ts       # Filtrar resultados
├── relatorio-arquetipos.ts     # Padrões encontrados
├── relatorio-estrutura.ts      # Estrutura do projeto
├── relatorio-padroes-uso.ts    # Padrões de uso
├── relatorio-poda.ts           # Oportunidades de limpeza
├── relatorio-reestruturar.ts   # Restruturação sugerida
├── relatorio-zelador-saude.ts  # Status de saúde
├── conselheiro-prometheus.ts   # Recomendações
└── analise-async-patterns.ts   # Análise async/await
```

### Fluxo

```
Resultados Brutos
    ↓
Filtro (aplicar regras)
    ↓
Analisadores (relatorio-*.ts)
    ↓
Formatadores (json/md/html)
    ↓
Saída Final
```

---

## `src/shared/` - Código Compartilhado

**Responsabilidade: PROTECTED_1 Utilitários**, tipos e operações reutilizáveis

```
shared/
├── contexto-projeto.ts         # Contexto da análise
├── memory.ts                   # Cache e memória
├── data-processing/            # Processamento de dados
├── helpers/                    # Helpers gerais
├── persistence/                # Persistência de dados
├── plugins/                    # Sistema de plugins
└── validation/                 # Validação
```

### Exemplo: Contexto

```typescript
// shared/contexto-projeto.ts

export class ContextoProjeto {
  caminhos: Set<string>;
  config: ConfiguracaoProjeto;
  analistas: Analista[];
  mensagens: Mensagens;

  // Métodos auxiliares
  obterArquivos(): Arquivo[]
  temArquivo(caminho: string): boolean
  obterConfig(chave: string): any
}
```

---

## `src/guardian/` - Monitor de Saúde

**Responsabilidade:** PROTECTED_1 Monitorar mudanças no projeto

```
guardian/
├── baseline.ts             # Baseline de referência
├── constantes.ts           # Constantes do sistema
├── diff.ts                 # Calcular diferenças
├── hash.ts                 # Hash de conteúdo
├── registros.ts            # Registro de mudanças
├── sentinela.ts            # Monitoramento
├── verificador.ts          # Verificação de integridade
└── vigia-oculto.ts         # (Hidden watcher)
```

---

## `src/api/` - Servidor REST API

**Responsabilidade:** PROTECTED_1 Servir API REST com Swagger UI

```
api/
├── index.ts              # Entry point da API
├── server.ts             # Configuração do servidor Express
├── middleware/           # Middlewares (CORS, rate limit, etc)
├── routes/               # Rotas da API
└── services/             # Serviços de negócio
```

### Endpoints Principais

- `GET /api/health` — Health check
- `GET /api/v1/analistas` — Listar analistas (paginado)
- `POST /api/v1/diagnosticar` — Executar diagnóstico (sync/async)
- `GET /api/v1/guardian` — Status do Guardian
- `POST /api/v1/guardian/baseline` — Criar baseline
- `GET /api/v1/metricas` — Histórico de métricas

Ver [REST-API.md](../../api/REST-API.md) para documentação completa.

---

## `src/sdk/` - SDK Programático

**Responsabilidade:** PROTECTED_1 Interface programática para uso como biblioteca

```
sdk/
└── index.ts              # Exports públicos do SDK
```

Permite usar o Prometheus como biblioteca em outros projetos Node.js.

---

## `src/app/` - Landing (Next.js)

**Responsabilidade:** Página estática exportada (`out/`) servida pelo GitHub App — cards informativos e botão de instalar o app.

A API operacional (webhooks, stats, jobs) fica em `src/app/github/app/`. Não há dashboard React separado no repositório.

---

## `src/licensas/` - Gerenciamento de Licenças

**Responsabilidade:** PROTECTED_1 Scanner de licenças de dependências

```
licensas/
├── disclaimer.ts           # Aviso de licença
├── fs-utils.ts            # File system utils
├── generate-notices.ts    # Gerar THIRD-PARTY-NOTICES.txt
├── licensas.ts            # Lógica de licença
├── normalizer.ts          # Normalizar nomes
├── scanner.ts             # Escanear dependências
├── spdx.d.ts             # Tipos SPDX
└── types.ts              # Tipos locais
```

### Fluxo

```
package.json
    ↓
Ler dependências (diretas)
    ↓
Escanear node_modules
    ↓
Extrair licenses
    ↓
Gerar relatório (THIRD-PARTY-NOTICES.txt)
```

---

## ⚡ `src/zeladores/` - Executores de Ação

**Responsabilidade:** PROTECTED_1 Executar correções e ações

```
zeladores/
├── zelador-base.ts         # Classe base
├── zelador-*.ts            # Implementações específicas
└── registry.ts             # Registro de zeladores
```

Exemplos:
- `zelador-variaveis.ts` - Remover variáveis não usadas
- `zelador-espacos.ts` - Limpar espaços em branco
- `zelador-console.ts` - Remover console.log

---

## `src/types/` - Definições TypeScript

**Responsabilidade:** PROTECTED_1 Interfaces e tipos central

```
types/
├── index.ts                # Export central
├── types.ts                # Tipos base
├── postcss-scss.d.ts      # Type stubs
├── analistas/              # Tipos analistas
├── cli/                    # Tipos CLI
├── core/                   # Tipos core
└── ...
```

**Exemplo:** PROTECTED_1
```typescript
export interface Arquivo {
  relPath: string;           // Caminho relativo
  ext: string;               // Extensão
  linguagem: string;         // Linguagem detectada
  ocorrencias: Ocorrencia[];
}

export interface ResultadoOcorrencia {
  linha: number;
  coluna: number;
  tipo: string;              // Type de problema
  mensagem: string;
  gravidade: 'critic' | 'major' | 'minor';
  pode_corrigir?: boolean;
  sugestao?: string;
}
```

---

## `tests/` - Testes

**Responsabilidade:** PROTECTED_1 Verificar funcionalidade

```
tests/
├── analistas/              # Testes de analistas
├── cli/                    # Testes de CLI
├── core/                   # Testes de núcleo
├── shared/                 # Testes de shared
└── fixtures/               # Dados de teste (arquivos)
```

### Estrutura de Teste

```
analistas/
├── detector-fragil.test.ts
├── fixtures/
│   ├── codigo-fragil.ts    # Arquivo com problema
│   ├── codigo-ok.ts        # Arquivo sem problema
│   └── ...
```

---

## Fluxo Geral

```
┌─────────────────────────────────────┐
│  Usuario: prometheus diagnosticar   │
└────────┬────────────────────────────┘
         │
         ↓
┌─────────────────────────────────────┐
│  bin/cli.ts - Parse argumentos      │
│  (yargs processor)                  │
└────────┬────────────────────────────┘
         │
         ↓
┌─────────────────────────────────────┐
│  cli/commands/diagnosticar.ts       │
│  (handler de comando)               │
└────────┬────────────────────────────┘
         │
         ↓
┌─────────────────────────────────────┐
│  core/execution/executor.ts         │
│  (Orquestrador principal)           │
│  1. Load config                     │
│  2. Discover files                  │
│  3. Load analyzers                  │
└────────┬────────────────────────────┘
         │
         ├─────────────────────────────────┐
         │                                 │
         ↓                                 ↓
┌──────────────────────────┐  ┌──────────────────────────┐
│  analistas/              │  │  core/parsing/          │
│  (Detectores)            │  │  (Parse código)         │
│  - Fragil                │  │                         │
│  - Morto                 │  │  1. Read file           │
│  - Duplicacao            │  │  2. Detect language     │
│  - ...                   │  │  3. Parse AST           │
└──────────────────────────┘  │  4. Analyze             │
         │                     └──────────────────────────┘
         │
         ↓
┌─────────────────────────────────────┐
│  core/reporting/                    │
│  (Format resultados)                │
│  - JSON report                      │
│  - Markdown report                  │
│  - HTML report                      │
│  - CSV export                       │
└────────┬────────────────────────────┘
         │
         ↓
┌─────────────────────────────────────┐
│  Output para usuario                │
│  (arquivo ou stdout)                │
└─────────────────────────────────────┘
```

---

## [GRADUACAO] Arquitetura em Camadas

```
┌────────────────────────────┐  User Interface
│  CLI / Commands            │  (bin/, cli/)
├────────────────────────────┤
│  Execution Orchestration   │  Business Logic
│  (core/execution/)         │  (core/)
├────────────────────────────┤
│  Analysis Layer            │  Domain
│  (analistas/)              │  (analyzers)
├────────────────────────────┤
│  Reporting Layer           │  Output
│  (relatorios/)             │
├────────────────────────────┤
│  Shared Utilities          │  Cross-cutting
│  (shared/)                 │
└────────────────────────────┘
```

---

## [PACOTE] Dependências Internas

```
bin/
├── → cli/        (CLI setup)
└── → core/       (Initialize)

cli/
├── → core/       (Execute analysis)
├── → analistas/  (Load analyzers)
└── → shared/     (Utilities)

core/
├── → analistas/  (Load analyzers)
├── → relatorios/ (Format output)
├── → shared/     (Utilities)
└── → messages/   (Translations)

analistas/
├── → shared/     (Utilities)
└── → types/      (Interfaces)

relatorios/
├── → shared/     (Data processing)
└── → core/       (Message formatting)

api/
├── → core/       (Execute analysis)
├── → analistas/  (List analyzers)
└── → guardian/   (Baseline operations)

sdk/
├── → core/       (Core functionality)
└── → analistas/  (Analysis functions)

app/
├── → api/        (REST API)
└── → core/       (Data access)
```

---

## [LANCAMENTO] Adicionando Novo Componente

### Novo Analista/Detector

1. **Criar arquivo** em `src/analistas/detectores/` ou `src/analistas/plugins/`
2. **Implementar interface** `Analista`
3. **Registrar** em `src/analistas/registry/` (autodiscovery para plugins)
4. **Adicionar testes** em `tests/analistas/`
5. **Documentar** em `docs/`

Ver [Criar Analista](../analistas/CRIAR-ANALISTA.md) para detalhe completo.

### Nova Rota API

1. **Criar rota** em `src/api/routes/`
2. **Registrar** em `src/api/index.ts`
3. **Documentar** com JSDoc/Swagger
4. **Adicionar testes** em `tests/api/`

---

## [BIBLIOTECA] Referências

- [Typescript Handbook](https://www.typescriptlang.org/docs/)
- [Node.js API](https://nodejs.org/api/)
- [Vitest](https://vitest.dev/)
