---
Proveniência e Autoria: Este documento integra o projeto Prometheus (licença MIT-0).
---


# Árvore Arquitetural - Prometheus

Estrutura completa de diretórios e componentes do Prometheus.

---

## Estrutura de Diretórios

```
prometheus/
├── src/                          # Código-fonte principal
│   ├── bin/                      # Entry points CLI
│   │   ├── cli.ts               # Interface principal do CLI
│   │   └── index.ts             # Inicialização
│   │
│   ├── cli/                      # Sistema de comandos
│   │   ├── index.ts             # Registrador de comandos
│   │   ├── commands/            # Implementação de comandos
│   │   ├── diagnostico/         # Handlers, filtros e exporters
│   │   ├── handlers/            # Handlers específicos por comando
│   │   ├── helpers/             # Utilitários CLI
│   │   ├── options/             # Definição de opções
│   │   └── processing/          # Processamento de dados CLI
│   │
│   ├── core/                     # Sistema central
│   │   ├── config/              # Configuração (prometheus.config.json)
│   │   ├── execution/           # Motor de execução
│   │   ├── messages/            # Sistema i18n + logs centralizados
│   │   ├── parsing/             # Parsing de arquivos
│   │   ├── registry/            # Registro de componentes
│   │   ├── reporting/           # Geração de relatórios
│   │   ├── schema/              # Validação de schemas
│   │   ├── utils/               # Utilitários gerais
│   │   └── workers/             # Workers para paralelismo
│   │
│   ├── analistas/                # Mini-sistema de análise
│   │   ├── README.md            # Guia de analistas
│   │   ├── architects/          # Detector de padrões arquiteturais
│   │   ├── corrections/         # Sistema de correções
│   │   ├── detectores/          # Detectores específicos
│   │   │   ├── detector-codigo-fragil.ts
│   │   │   ├── detector-arquitetura.ts
│   │   │   ├── detector-dependencias.ts
│   │   │   ├── detector-estrutura.ts
│   │   │   ├── detector-agregados.ts
│   │   │   └── ...
│   │   ├── strategists/         # Estratégias de análise
│   │   ├── js-ts/               # Analistas específicos JS/TS
│   │   ├── plugins/             # Sistema de plugins para analistas
│   │   ├── pontuadores/         # Sistema de pontuação
│   │   ├── registry/            # Registro de analistas
│   │   ├── github-actions/      # Analista GitHub Actions
│   │   ├── gitlab-ci/           # Analista GitLab CI
│   │   ├── circleci/            # Analista CircleCI
│   │   ├── jenkins/             # Analista Jenkins
│   │   ├── azure-pipelines/     # Analista Azure Pipelines
│   │   ├── react/               # Analista React
│   │   ├── html/                # Analista HTML
│   │   ├── css/                 # Analista CSS
│   │   ├── css-in-js/           # Analista CSS-in-JS
│   │   ├── tailwind/            # Analista Tailwind
│   │   ├── python/              # Analista Python
│   │   ├── shell/               # Analista Shell
│   │   ├── sql/                 # Analista SQL
│   │   ├── xml/                 # Analista XML
│   │   └── svg/                 # Analista SVG
│   │
│   ├── relatorios/              # Geração de relatórios
│   │   ├── gerador-relatorio.ts
│   │   ├── relatorio-parsetipos.ts
│   │   ├── relatorio-arquetipo.ts
│   │   ├── relatorio-estrutura.ts
│   │   ├── relatorio-padroes.ts
│   │   └── ...
│   │
│   ├── zeladores/               # Sistema de vigilância e limpeza
│   │   └── zelador-messages.ts
│   │
│   ├── guardian/                # Sistema de integridade
│   │   ├── baseline.ts
│   │   ├── constantes.ts
│   │   ├── diff.ts
│   │   ├── hash.ts
│   │   ├── registros.ts
│   │   ├── sentinela.ts
│   │   ├── verificador.ts
│   │   └── vigia-oculto.ts
│   │
│   ├── licensas/                # Sistema de licenças
│   │   ├── disclaimer.ts
│   │   ├── fs-utils.ts
│   │   ├── generate-notices.ts
│   │   ├── licensas.ts
│   │   └── scanner.ts
│   │
│   ├── api/                     # Servidor REST API
│   │   ├── index.ts
│   │   ├── server.ts
│   │   ├── middleware/
│   │   ├── routes/
│   │   └── services/
│   │
│   ├── sdk/                     # SDK programático
│   │   └── index.ts
│   │
│   ├── app/                     # Dashboard web
│   │
│   ├── shared/                  # Código compartilhado
│   │   ├── contexto-projeto.ts
│   │   ├── memory.ts
│   │   ├── data-processing/
│   │   ├── helpers/
│   │   ├── plugins/
│   │   ├── persistence/
│   │   └── validation/
│   │
│   └── types/                   # Definições de tipos TypeScript
│       ├── index.ts
│       ├── types.ts
│       ├── analistas/
│       ├── cli/
│       └── ...
│
├── tests/                        # Testes
│   ├── analistas/
│   ├── cli/
│   └── ...
│
├── docs/                         # Documentação
│   ├── INDICE.md                # Índice completo navegável
│   ├── README.md                # Entry point
│   │
│   ├── guias/                   # Guias do usuário
│   │   ├── GUIA-INICIO-RAPIDO.md
│   │   ├── GUIA-COMANDOS.md
│   │   ├── GUIA-CONFIGURACAO.md
│   │   ├── GITHUB-APP.md
│   │   └── DASHBOARD-GITHUB-PLUGINS.md
│   │
│   ├── arquitetura/             # Arquitetura e design
│   │   ├── README.md
│   │   ├── ARVORE-ARQUITETURAL.md (este arquivo)
│   │   ├── TYPE-SAFETY.md
│   │   ├── SISTEMA-ERROS.md
│   │   ├── MENSAGENS-LOGS.md
│   │   └── SEGURANCA.md
│   │
│   ├── desenvolvimento/         # Para desenvolvedores
│   │   ├── PADROES.md
│   │   ├── SISTEMA-PLUGINS.md
│   │   ├── analistas/
│   │   │   └── CRIAR-ANALISTA.md
│   │   └── estrutura-codigo/
│   │       └── README.md
│   │
│   ├── referencia/              # Referência técnica
│   │   └── comandos/
│   │       └── README.md
│   │
│   ├── api/                     # Documentação da API
│   │   └── REST-API.md
│   │
│   ├── exemplos/                # Exemplos de uso
│   ├── roadmap/                 # Plano de desenvolvimento
│   └── historico/               # Histórico de mudanças
│
├── scripts/                      # Scripts utilitários
│
├── package.json                 # Dependências e scripts
├── tsconfig.json               # Configuração TypeScript
├── vitest.config.ts            # Configuração de testes
├── eslint.config.js            # Configuração de linting
└── prometheus.config.json      # Configuração do Prometheus
```

---

## Fluxo de Dados e Processamento

```
┌─────────────────────────────────────────────────────────────┐
│                    ENTRADA: Projeto JS/TS                  │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
        ┌────────────────────────────────┐
        │  1. PARSING (src/core/parsing)  │
        │  - Análise de arquivos          │
        │  - Construção de AST            │
        │  - Extração de informações      │
        └────────┬─────────────────────────┘
                 │
                 ▼
        ┌────────────────────────────────┐
        │  2. ANALISTAS (src/analistas)   │
        │  - Código frágil                │
        │  - Dependências                 │
        │  - Segurança                    │
        │  - Padrões                      │
        │  - Estrutura                    │
        └────────┬─────────────────────────┘
                 │
                 ▼
        ┌────────────────────────────────┐
        │  3. AGREGAÇÃO (src/shared)      │
        │  - Consolidação de dados        │
        │  - Cálculo de métricas          │
        │  - Priorização                  │
        └────────┬─────────────────────────┘
                 │
                 ▼
        ┌────────────────────────────────┐
        │  4. RELATÓRIOS                  │
        │  (src/relatorios)               │
        │  - JSON/Markdown                │
        │  - Templates                    │
        │  - Filtros inteligentes         │
        └────────┬─────────────────────────┘
                 │
                 ▼
        ┌────────────────────────────────┐
        │  5. AÇÕES                       │
        │  - Guardian (verificação)       │
        │  - Reestruturação               │
        │  - Correções                    │
        │  - Limpeza                      │
        └────────┬─────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────────────────────────┐
│                  SAÍDA: Relatórios/Ações                   │
└─────────────────────────────────────────────────────────────┘
```

---

## Camadas da Arquitetura

### 1. **Camada CLI** (Interface)

- Responsável por: Interação com usuário, parsing de flags, definição de contexto
- Localização: `src/cli/`
- Entrada: Linhas de comando
- Saída: Execução de handlers

### 2. **Camada Core** (Motor)

- Responsável por: Parsing, Configuração, Execução, Registro
- Localização: `src/core/`
- Entrada: Projeto a analisar
- Saída: Dados estruturados

### 3. **Camada de Análise** (Lógica)

- Responsável por: Análises específicas, detecção de problemas, pontuação
- Localização: `src/analistas/`
- Entrada: AST e metadados
- Saída: Descobertas e recomendações

### 4. **Camada de Relatórios** (Consolidação)

- Responsável por: Agregação, Formatação, Templates
- Localização: `src/relatorios/`
- Entrada: Descobertas dos analistas
- Saída: Relatórios estruturados

### 5. **Camada de Ação** (Execução)

- Responsável por: Guardian, Reestruturação, Correções
- Localização: `src/guardian/`, correções em `src/analistas/`
- Entrada: Plano de ação
- Saída: Mudanças aplicadas

---

## Componentes Chave

### Analistas

- **Detectores** Identificam problemas (detector-código-fragil, detector-arquitetura, etc)
- **Arquitetos** Identificam padrões arquiteturais
- **Estrategistas** Propõem estratégias de solução
- **Pontuadores** Atribuem scores

### Registry (Registro)

- Registro centralizado de analistas, handlers, comandos
- Permite descoberta dinâmica de componentes
- Suporta plugin system

### Messages (Mensagens i18n)

- Sistema de mensagens em múltiplos idiomas
- Estruturado por módulo
- Suporta: PT, EN, ZH, JA

### Configuration (Configuração)

- `prometheus.config.json` na raiz do projeto
- Valores padrão em `src/core/config/`
- Validação via schema JSON

---

## Metricas e Pontuação

```
┌──────────────────────────────────────────┐
│         ANÁLISE DE QUALIDADE             │
├──────────────────────────────────────┘
│
└─→ Segurança (0-100)
    ├─ Issues críticos
    ├─ Vulnerabilidades conhecidas
    └─ Boas práticas

└─→ Manutenibilidade (0-100)
    ├─ Complexidade
    ├─ Coesão
    ├─ Acoplamento
    └─ Tamanho de funções

└─→ Confiabilidade (0-100)
    ├─ Tratamento de erros
    ├─ Type safety
    ├─ Testes
    └─ Performance

└─→ Estrutura (0-100)
    ├─ Organização
    ├─ Nomes
    ├─ Padrões
    └─ Dependências
```

---

## Dependências Principais

- **Node.js** v24.15.0+
- **TypeScript** v6.0+
- **Vitest** Testing framework
- **ESLint** Code quality
- **Commander** CLI framework
- **Express** REST API server
- **Zod** Validação de schemas
- **Babel** Parsing JS/TS

---

## Próximas Etapas de Documentação

Veja também:
- [Como Criar Analistas](../desenvolvimento/analistas/CRIAR-ANALISTA.md)
- [Padrões de Desenvolvimento](../desenvolvimento/PADROES.md)
- [Sistema de Plugins](../desenvolvimento/SISTEMA-PLUGINS.md)
- [Estrutura de Código](../desenvolvimento/estrutura-codigo/README.md)
- [REST API](../api/REST-API.md)
