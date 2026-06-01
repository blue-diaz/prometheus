---
Proveniência e Autoria: Este documento integra o projeto Prometheus (licença MIT-0).
---


# Guia de Comandos

Este guia documenta os comandos efetivamente expostos pela CLI atual do Prometheus.

## Visão Geral

```bash
prometheus --help
```

Comandos disponíveis:

- `diagnosticar|diag`
- `guardian`
- `formatar`
- `otimizar-svg`
- `podar`
- `reestruturar`
- `atualizar`
- `analistas`
- `metricas`
- `fix-types`
- `licencas`
- `names`
- `rename`
- `reverter`
- `perf`
- `marketplace`
- `compliance`
- `formatters`

## diagnosticar

Análise completa do repositório.

```bash
prometheus diagnosticar [opções]
```

Flags principais:

- `--listar-analistas`
- `--guardian-check`
- `--json`
- `--json-ascii`
- `--fast`
- `--trust-compiler`
- `--verify-cycles`
- `--criar-arquetipo`
- `--salvar-arquetipo`
- `--include <padrao>`
- `--exclude <padrao>`
- `--exclude-tests`
- `--full`
- `--compact`
- `--log-level <nivel>`
- `--executive`
- `--auto-fix`
- `--auto-fix-mode <modo>`
- `--auto-fix-conservative`
- `--fix`
- `--fix-safe`
- `--show-fixes`
- `--export`
- `--export-full`
- `--export-to <dir>`
- `--path <diretorio>`

Exemplos:

```bash
prometheus diagnosticar --compact
prometheus diagnosticar --full --guardian-check
prometheus diagnosticar --json --export --export-to relatorios
prometheus diagnosticar --include "src/**" --exclude "*****/****.test.ts"
prometheus diagnosticar --fix-safe
prometheus diagnosticar --path ./projeto-a --path ./projeto-b --json
```

## guardian

Baseline e verificação de integridade.

```bash
prometheus guardian [opções]
```

Flags:

- `--accept-baseline`
- `--diff`
- `--full-scan`
- `--json`

Exemplos:

```bash
prometheus guardian --accept-baseline
prometheus guardian --diff
prometheus guardian --diff --json
```

## formatar

Formatação interna do projeto.

```bash
prometheus formatar [opções]
```

Flags:

- `--check`
- `--write`
- `--engine <engine>`
- `--include <padrao>`
- `--exclude <padrao>`

Exemplos:

```bash
prometheus formatar --check
prometheus formatar --write --engine auto
```

## otimizar-svg

Otimização de SVGs usando o motor interno.

```bash
prometheus otimizar-svg [opções]
```

Flags:

- `--dir <caminho>`
- `--write`
- `--dry`
- `--include <padrao>`
- `--exclude <padrao>`

Exemplos:

```bash
prometheus otimizar-svg --dry
prometheus otimizar-svg --dir assets/icons --write
```

## podar

Remoção de arquivos órfãos e lixo do repositório.

```bash
prometheus podar [opções]
```

Flags:

- `--force`
- `--include <padrao>`
- `--exclude <padrao>`

Exemplos:

```bash
prometheus podar
prometheus podar --force
```

## reestruturar

Aplica plano de reorganização estrutural.

```bash
prometheus reestruturar [opções]
```

Flags:

- `--auto`
- `--aplicar`
- `--somente-plano`
- `--domains`
- `--flat`
- `--prefer-estrategista`
- `--preset <nome>`
- `--categoria <pair>`
- `--include <padrao>`
- `--exclude <padrao>`

Exemplos:

```bash
prometheus reestruturar --somente-plano --preset prometheus
prometheus reestruturar --auto --flat
prometheus reestruturar --preset node-community --categoria controller=handlers
```

## atualizar

Atualização segura do Prometheus.

```bash
prometheus atualizar [opções]
```

Flags:

- `--global`

## analistas

Lista analistas registrados e seus metadados.

```bash
prometheus analistas [opções]
```

Flags:

- `--json`
- `--output <arquivo>`
- `--doc <arquivo>`

## metricas

Consulta histórico de métricas de execução.

```bash
prometheus metricas [opções]
```

Flags:

- `--json`
- `--limite <n>`
- `--export <arquivo>`
- `--analistas`

Exemplos:

```bash
prometheus metricas
prometheus metricas --analistas
prometheus metricas --json --limite 20
```

## fix-types

Detecta e corrige `any` e `unknown`.

```bash
prometheus fix-types [opções]
```

Flags:

- `--dry-run`
- `--target <path>`
- `--confidence <number>`
- `--verbose`
- `--interactive`
- `--export`
- `--include <padrao>`
- `--exclude <padrao>`

Exemplos:

```bash
prometheus fix-types --dry-run
prometheus fix-types --target src --confidence 90 --verbose
```

## licencas

Ferramentas relacionadas a licenças e avisos de terceiros.

```bash
prometheus licencas [subcomando]
```

Subcomandos confirmados:

- `scan`
- `notices`
- `disclaimer`

## names

Gera arquivos de mapeamento de nomes no diretório `names/`.

```bash
prometheus names [opções]
```

Flags:

- `--legacy`

## rename

Aplica renomeações baseadas nos mapeamentos gerados por `names`.

```bash
prometheus rename
```

## reverter

Gerencia o mapa de reversão de moves estruturais.

```bash
prometheus reverter [subcomando]
```

Subcomandos:

- `listar`
- `arquivo <arquivo>`
- `move <id>`
- `limpar`
- `status`

## compliance

Relatórios de conformidade com frameworks de segurança (ISO 27001, SOC 2).

```bash
prometheus compliance <subcomando> [opções]
```

Subcomandos:

- `list` — Lista frameworks de compliance disponíveis
- `report <framework>` — Gera relatório de compliance para o framework

Opções do `report`:

- `--json` — Saída em formato JSON
- `-o, --output <arquivo>` — Salvar em arquivo

Frameworks suportados:

- `iso-27001` — ISO/IEC 27001:2022 (15 controles mapeados)
- `soc2` — SOC 2 AICPA (12 princípios de confiança mapeados)

Exemplos:

```bash
prometheus compliance list
prometheus compliance report iso-27001
prometheus compliance report soc2 --json
prometheus compliance report iso-27001 -o relatorio-compliance.md
```

## formatters

Gerencia formatadores de relatório (plugins de saída).

```bash
prometheus formatters <subcomando> [opções]
```

Subcomandos:

- `list` — Lista formatadores registrados
- `register <nome> <caminho>` — Registra um novo formatador via caminho de módulo
- `remove <nome>` — Remove um formatador registrado

Opções do `list`:

- `--json` — Saída em formato JSON

Exemplos:

```bash
prometheus formatters list
prometheus formatters list --json
prometheus formatters register meu-exporter ./caminho/modulo.js
prometheus formatters remove meu-exporter
```

## perf

Baseline e comparação de performance sintética.

```bash
prometheus perf [opções] [subcomando]
```

Flags:

- `--dir <dir>`
- `--json`
- `--limite <n>`

Subcomandos:

- `baseline`
- `compare`

Exemplos:

```bash
prometheus perf baseline
prometheus perf compare
prometheus perf --json compare
```
