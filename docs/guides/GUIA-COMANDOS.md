---
Proveniência e Autoria: Este documento integra o projeto Prometheus (licença MIT-0).
---

# Referência de Comandos - Prometheus

Referência completa de todos os comandos expostos pela CLI do Prometheus v0.8.0.

---

## Visão Geral

```bash
prometheus --help
```

### Lista de Comandos

| Comando | Alias | Descrição |
|---|---|---|
| `diagnosticar` | `diag` | Análise completa do repositório |
| `guardian` | — | Baseline e verificação de integridade |
| `formatar` | — | Formatação interna do projeto |
| `otimizar-svg` | — | Otimização de arquivos SVG |
| `podar` | — | Remoção de arquivos órfãos e lixo |
| `barrels` | — | Geração e organização de barrels (index.ts) |
| `atualizar` | — | Atualização segura do Prometheus |
| `analistas` | — | Lista analistas registrados |
| `metricas` | — | Histórico de métricas de execução |
| `fix-types` | — | Detecta e corrige `any` e `unknown` |
| `licencas` | — | Scan de licenças de dependências |
| `names` | — | Gera mapeamento de nomes de variáveis |
| `rename` | — | Aplica renomeações em massa |
| `reverter` | — | Gerencia mapa de reversão de moves |
| `perf` | — | Baseline e comparação de performance |
| `marketplace` | — | Gerencia analistas do Marketplace (search, install, remove, list, info, publish) |

---

## diagnosticar

Análise completa do repositório com todos os analistas registrados.

```bash
prometheus diagnosticar [opções]
```

### Flags

| Flag | Tipo | Descrição |
|---|---|---|
| `--listar-analistas` | boolean | Lista todos os analistas que serão executados |
| `--guardian-check` | boolean | Executa verificação do Guardian após análise |
| `--json` | boolean | Saída em formato JSON |
| `--json-ascii` | boolean | Saída JSON com caracteres ASCII-safe |
| `--fast` | boolean | Modo rápido (analistas essenciais apenas) |
| `--trust-compiler` | boolean | Confia no compilador TypeScript para certas verificações |
| `--verify-cycles` | boolean | Verifica dependências circulares |
| `--criar-arquetipo` | boolean | Cria arquétipo do projeto |
| `--salvar-arquetipo` | boolean | Salva arquétipo gerado |
| `--include <padrao>` | string | Padrão glob de inclusão |
| `--exclude <padrao>` | string | Padrão glob de exclusão |
| `--exclude-tests` | boolean | Exclui arquivos de teste da análise |
| `--full` | boolean | Análise completa com todos os detalhes |
| `--compact` | boolean | Saída compacta |
| `--log-level <nivel>` | string | Nível de log (debug, info, warn, error) |
| `--executive` | boolean | Modo executivo (resumo para gestão) |
| `--auto-fix` | boolean | Aplica correções automáticas |
| `--auto-fix-mode <modo>` | string | Modo de auto-fix (conservative, permissive) |
| `--auto-fix-conservative` | boolean | Auto-fix apenas em casos seguros |
| `--fix` | boolean | Alias para --auto-fix |
| `--fix-safe` | boolean | Aplica apenas correções seguras |
| `--show-fixes` | boolean | Mostra correções disponíveis sem aplicar |
| `--export` | boolean | Exporta relatório para arquivo |
| `--export-full` | boolean | Exportação completa com sharding |
| `--export-to <dir>` | string | Diretório de exportação |
| `--path <diretorio>` | string | Caminho(s) para projeto(s) a analisar (repetível para multi-projeto) |

### Exemplos

```bash
# Análise padrão
prometheus diagnosticar

# Modo compacto
prometheus diagnosticar --compact

# Análise completa com Guardian
prometheus diagnosticar --full --guardian-check

# Exportar JSON para diretório customizado
prometheus diagnosticar --json --export --export-to relatorios

# Filtrar escopo
prometheus diagnosticar --include "src/**" --exclude "**/*.test.ts"

# Auto-fix conservador
prometheus diagnosticar --fix-safe

# Análise multi-projeto
prometheus diagnosticar --path ./projeto-a --path ./projeto-b

# Análise multi-projeto com JSON
prometheus diagnosticar --path ./frontend --path ./backend --json
```

---

## guardian

Baseline e verificação de integridade do projeto.

```bash
prometheus guardian [opções]
```

### Flags

| Flag | Tipo | Descrição |
|---|---|---|
| `--accept-baseline` | boolean | Aceita/cria baseline atual |
| `--diff` | boolean | Mostra diferenças em relação ao baseline |
| `--full-scan` | boolean | Verificação completa |
| `--json` | boolean | Saída em formato JSON |

### Exemplos

```bash
# Criar baseline inicial
prometheus guardian --accept-baseline

# Verificar mudanças
prometheus guardian --diff

# Verificação completa em JSON
prometheus guardian --diff --json --full-scan
```

---

## formatar

Formatação interna do projeto.

```bash
prometheus formatar [opções]
```

### Flags

| Flag | Tipo | Descrição |
|---|---|---|
| `--check` | boolean | Verifica formatação sem alterar |
| `--write` | boolean | Aplica formatação |
| `--engine <engine>` | string | Engine de formatação (auto, internal) |
| `--include <padrao>` | string | Padrão glob de inclusão |
| `--exclude <padrao>` | string | Padrão glob de exclusão |

### Exemplos

```bash
# Verificar formatação
prometheus formatar --check

# Aplicar formatação automática
prometheus formatar --write --engine auto
```

---

## otimizar-svg

Otimização de arquivos SVG.

```bash
prometheus otimizar-svg [opções]
```

### Flags

| Flag | Tipo | Descrição |
|---|---|---|
| `--dir <caminho>` | string | Diretório de SVGs |
| `--write` | boolean | Aplica otimizações |
| `--dry` | boolean | Preview sem alterar arquivos |
| `--include <padrao>` | string | Padrão glob de inclusão |
| `--exclude <padrao>` | string | Padrão glob de exclusão |

### Exemplos

```bash
# Preview
prometheus otimizar-svg --dry

# Otimizar diretório específico
prometheus otimizar-svg --dir assets/icons --write
```

---

## podar

Remoção de arquivos órfãos e lixo do repositório.

```bash
prometheus podar [opções]
```

### Flags

| Flag | Tipo | Descrição |
|---|---|---|
| `--force` | boolean | Remove sem confirmação |
| `--include <padrao>` | string | Padrão glob de inclusão |
| `--exclude <padrao>` | string | Padrão glob de exclusão |

### Exemplos

```bash
# Listar arquivos órfãos
prometheus podar

# Forçar remoção
prometheus podar --force
```

---

## barrels

Geração e organização de barrels (arquivos `index.ts`) para o projeto.

```bash
prometheus barrels [opções]
```

### Flags

| Flag | Tipo | Descrição |
|---|---|---|
| `--scan` | boolean | Varre o projeto e mostra quais barrels seriam criados/atualizados (dry-run) |
| `--generate` | boolean | Cria e atualiza barrels com exports organizados |
| `--dry-run` | boolean | Simula a operação sem modificar arquivos |

### Exemplos

```bash
# Preview do que seria gerado
prometheus barrels --scan

# Gerar barrels
prometheus barrels --generate

# Simular geração
prometheus barrels --generate --dry-run
```

---

## atualizar

Atualização segura do Prometheus.

```bash
prometheus atualizar [opções]
```

### Flags

| Flag | Tipo | Descrição |
|---|---|---|
| `--global` | boolean | Atualiza instalação global |

---

## analistas

Lista analistas registrados e seus metadados.

```bash
prometheus analistas [opções]
```

### Flags

| Flag | Tipo | Descrição |
|---|---|---|
| `--json` | boolean | Saída em formato JSON |
| `--output <arquivo>` | string | Salva em arquivo |
| `--doc <arquivo>` | string | Gera documentação |

---

## metricas

Consulta histórico de métricas de execução.

```bash
prometheus metricas [opções]
```

### Flags

| Flag | Tipo | Descrição |
|---|---|---|
| `--json` | boolean | Saída em formato JSON |
| `--limite <n>` | number | Número máximo de registros |
| `--export <arquivo>` | string | Exporta para arquivo |
| `--analistas` | boolean | Inclui métricas por analista |

### Exemplos

```bash
# Ver métricas
prometheus metricas

# Métricas por analista
prometheus metricas --analistas

# Exportar JSON limitado
prometheus metricas --json --limite 20
```

---

## fix-types

Detecta e corrige tipos inseguros (`any`, `unknown`).

```bash
prometheus fix-types [opções]
```

### Flags

| Flag | Tipo | Descrição |
|---|---|---|
| `--dry-run` | boolean | Preview sem alterar |
| `--target <path>` | string | Diretório alvo |
| `--confidence <number>` | number | Limiar de confiança (0-100) |
| `--verbose` | boolean | Saída detalhada |
| `--interactive` | boolean | Modo interativo |
| `--export` | boolean | Exporta relatório |
| `--include <padrao>` | string | Padrão glob de inclusão |
| `--exclude <padrao>` | string | Padrão glob de exclusão |

### Exemplos

```bash
# Preview
prometheus fix-types --dry-run

# Correção com confiança mínima
prometheus fix-types --target src --confidence 90 --verbose
```

---

## licencas

Ferramentas relacionadas a licenças e avisos de terceiros.

```bash
prometheus licencas [subcomando]
```

### Subcomandos

| Subcomando | Descrição |
|---|---|
| `scan` | Escaneia licenças de dependências |
| `notices` | Gera arquivo THIRD-PARTY-NOTICES |
| `disclaimer` | Gerencia disclaimers de proveniência |

### Exemplos

```bash
# Scan de licenças
prometheus licencas scan

# Gerar notices
prometheus licencas notices generate

# Adicionar disclaimer
prometheus licencas disclaimer add

# Verificar disclaimers
prometheus licencas disclaimer verify
```

---

## names

Gera arquivos de mapeamento de nomes no diretório `names/`.

```bash
prometheus names [opções]
```

### Flags

| Flag | Tipo | Descrição |
|---|---|---|
| `--legacy` | boolean | Usa formato legado |

---

## rename

Aplica renomeações baseadas nos mapeamentos gerados por `names`.

```bash
prometheus rename
```

---

## reverter

Gerencia o mapa de reversão de moves estruturais.

```bash
prometheus reverter [subcomando]
```

### Subcomandos

| Subcomando | Descrição |
|---|---|
| `listar` | Lista entradas no mapa de reversão |
| `arquivo <arquivo>` | Reverte arquivo específico |
| `move <id>` | Reverte move por ID |
| `limpar` | Limpa mapa de reversão |
| `status` | Mostra status do mapa |

---

## perf

Baseline e comparação de performance sintética.

```bash
prometheus perf [subcomando] [opções]
```

### Subcomandos

| Subcomando | Descrição |
|---|---|
| `baseline` | Cria snapshot de baseline |
| `compare` | Compara com baseline |

### Flags

| Flag | Tipo | Descrição |
|---|---|---|
| `--dir <dir>` | string | Diretório de snapshots |
| `--json` | boolean | Saída em formato JSON |
| `--limite <n>` | number | Número máximo de comparações |

### Exemplos

```bash
# Criar baseline
prometheus perf baseline

# Comparar com baseline
prometheus perf compare

# Comparação em JSON
prometheus perf --json compare
```

---

## Ajuda Geral

```bash
# Listar todos os comandos
prometheus --help

# Ajuda de comando específico
prometheus diagnosticar --help
prometheus guardian --help
prometheus fix-types --help
```

---

**Versão:** v0.11.0
**Última atualização:** 2026-05-28
