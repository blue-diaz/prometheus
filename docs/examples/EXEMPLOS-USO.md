---
Proveniência e Autoria: Este documento integra o projeto Prometheus (licença MIT-0).
---

# Exemplos de Uso - Prometheus

Exemplos práticos e casos de uso reais do Prometheus.

---

## Exemplo 1: Análise Básica de Projeto

Analisar um projeto JavaScript/TypeScript simples:

```bash
# Análise padrão
prometheus diagnosticar

# Análise completa com detalhes
prometheus diagnosticar --full

# Análise e gerar relatório JSON
prometheus diagnosticar --json --export --export-to relatorios

# Filtrar escopo de análise
prometheus diagnosticar --include "src/**" --exclude "**/*.test.ts"
```

### Resultado Esperado

```json
{
  "totalArquivos": 45,
  "totalOcorrencias": 47,
  "duracaoMs": 2300,
  "ocorrencias": [
    {
      "relPath": "src/utils/parser.ts",
      "linha": 45,
      "nivel": "erro",
      "tipo": "funcoes-longas",
      "mensagem": "Função excede limite de linhas"
    }
  ]
}
```

---

## Exemplo 2: Auto-fix de Problemas

Corrigir automaticamente problemas encontrados:

```bash
# Mostrar correções disponíveis sem aplicar
prometheus diagnosticar --show-fixes

# Aplicar apenas correções seguras
prometheus diagnosticar --fix-safe

# Auto-fix conservador durante diagnóstico
prometheus diagnosticar --auto-fix --auto-fix-mode conservative

# Corrigir tipos inseguros (any, unknown)
prometheus fix-types --dry-run
prometheus fix-types --target src --confidence 90
```

---

## Exemplo 3: Guardian - Monitorar Saúde

Configurar Guardian para monitorar continuamente:

```bash
# Estabelecer baseline de saúde
prometheus guardian --accept-baseline

# Verificar estado atual vs baseline
prometheus guardian --diff

# Verificação completa em JSON
prometheus guardian --diff --json --full-scan
```

---

## Exemplo 4: Relatórios Detalhados

Gerar diferentes tipos de relatório:

```bash
# Relatório JSON exportado
prometheus diagnosticar --json --export --export-to relatorios

# Relatório completo com sharding
prometheus diagnosticar --json --export-full

# Modo executivo (resumo para gestão)
prometheus diagnosticar --executive

# Ver métricas históricas
prometheus metricas
prometheus metricas --analistas
prometheus metricas --json --limite 20
```

### Exemplo de Saída Markdown (via --full)

```markdown
# Relatório de Análise - meu-projeto

**Data**: 2026-05-19
**Tempo de Análise**: 2.3s

## Resumo Executivo

- **Arquivos**: 45
- **Linhas**: 12,500
- **Problemas**: 47 (3 críticos, 15 maiores)

## Problemas por Tipo

### Código Frágil (12 ocorrências)
- `src/utils/parser.ts:45` - Função muito longa
- `src/api/handler.ts:12` - Complexidade cognitiva alta
```

---

## Exemplo 5: Criar Analista Customizado

Criar novo detector para suas necessidades:

```typescript
// src/analistas/plugins/detector-async-sem-await.ts

import type { Analista, OcorrenciaAnalista } from '@/types/analistas/index.js';
import type { ArquivoEntrada } from '@/types/core/index.js';

export class DetectorAsyncSemAwait implements Analista {
  nome = 'AsyncSemAwait';
  descricao = 'Detecta funções async sem await';
  categoria = 'code-quality';
  linguagens = ['typescript', 'javascript'];

  async analisar(arquivo: ArquivoEntrada, conteudo: string): Promise<OcorrenciaAnalista[]> {
    const ocorrencias: OcorrenciaAnalista[] = [];
    const regex = /async\s+function\s+(\w+)\s*\([^)]*\)\s*{/g;

    let match;
    while ((match = regex.exec(conteudo))) {
      const funcao = match[1];
      const inicio = match.index;
      // Encontrar fim da função (simplificado)
      const fim = conteudo.indexOf('}', inicio);
      const corpo = conteudo.substring(inicio, fim);

      if (!corpo.includes('await')) {
        const linhas = conteudo.substring(0, inicio).split('\n').length;

        ocorrencias.push({
          relPath: arquivo.relPath,
          linha: linhas,
          tipo: 'async-sem-await',
          nivel: 'aviso',
          mensagem: `Função '${funcao}' é async mas não usa await`,
          sugestao: `Remover 'async' de '${funcao}' ou adicionar await`,
        });
      }
    }

    return ocorrencias;
  }
}

export default DetectorAsyncSemAwait;
```

Plugins com prefixo `analista-` ou `detector-` são descobertos automaticamente via autodiscovery.

---

## Exemplo 6: Debug e Diagnóstico

Quando algo não funciona:

```bash
# Aumentar verbosidade
prometheus diagnosticar --full

# Log level detalhado
prometheus diagnosticar --log-level debug

# Listar analistas que serão executados
prometheus diagnosticar --listar-analistas

# Verificar dependências circulares
prometheus diagnosticar --verify-cycles

# Criar issue com dados de debug
prometheus diagnosticar --json --full > dados-debug.json
```

### Checklist de Troubleshooting

```bash
# 1. Verificar versão
prometheus --version

# 2. Listar analistas disponíveis
prometheus analistas

# 3. Testar um arquivo específico
prometheus diagnosticar --include "src/test.ts"

# 4. Verificar configuração
cat prometheus.config.json
```

---

## Exemplo 7: Integração com CI/CD

### GitHub Actions

```yaml
name: Prometheus Analysis
on: [push, pull_request]

jobs:
  analyze:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '24'

      - name: Install Prometheus
        run: npm install -g prometheus

      - name: Run Analysis
        run: prometheus diagnosticar --json --export --export-to reports

      - name: Upload Report
        if: always()
        uses: actions/upload-artifact@v4
        with:
          name: prometheus-report
          path: reports/
```

### GitLab CI

```yaml
prometheus-analysis:
  image: node:24
  script:
    - npm install -g prometheus
    - prometheus diagnosticar --json --export --export-to reports
  artifacts:
    paths:
      - reports/
```

---

## Exemplo 8: Pré-commit Hook

Rodar Prometheus automaticamente em commit via lint-staged:

```json
// package.json
{
  "lint-staged": {
    "*.{ts,tsx,js,jsx}": ["eslint --fix"]
  }
}
```

Ou via husky + script customizado:

```bash
#!/bin/bash
#.husky/pre-commit

echo "Executando Prometheus ..."
prometheus diagnosticar --include "src/**" --compact

if [ $? -ne 0 ]; then
  echo "Problemas críticos encontrados. Execute 'prometheus diagnosticar --full' para detalhes."
  exit 1
fi

echo "Análise passou"
exit 0
```

---

## Exemplo 9: Workflow de Aprendizado

Para aprender Prometheus gradualmente:

### Dia 1: Configuração Básica

```bash
# 1. Rodar análise padrão
prometheus diagnosticar

# 2. Revisar resultado
```

### Dia 2: Entender Problemas

```bash
# 1. Análise completa
prometheus diagnosticar --full

# 2. Analisar com verbose
prometheus diagnosticar --full --log-level debug
```

### Dia 3: Auto-fix

```bash
# 1. Criar backup
git commit -m "Backup antes de prometheus auto-fix"

# 2. Revisar mudanças propostas
prometheus diagnosticar --show-fixes

# 3. Aplicar correções seguras
prometheus diagnosticar --fix-safe
```

### Dia 4: Integração

```bash
# 1. Configurar CI/CD (ver Exemplo 7)
# 2. Estabelecer baseline
prometheus guardian --accept-baseline
# 3. Rodar regularmente
```

---

## Exemplo 10: Projeto Grande (Monorepo)

Para projetos com múltiplos pacotes:

### Estrutura

```
meu-mono-repo/
├── packages/
│   ├── core/
│   ├── api/
│   └── cli/
└── prometheus.config.json
```

### Configuração Raiz

```json
{
  "INCLUDE_EXCLUDE_RULES": {
    "globalExcludeGlob": [
      "node_modules/**",
      "dist/**",
      "packages/*/node_modules/**"
    ]
  },
  "languageSupport": {
    "typescript": { "enabled": true },
    "javascript": { "enabled": true }
  }
}
```

### Análise

```bash
# Todos os pacotes
prometheus diagnosticar

# Apenas um pacote
prometheus diagnosticar --include "packages/core/**"

# Comparar com baseline
prometheus guardian --diff
```

---

## Próximos Passos

1. **Explore** Rode `prometheus --help` para ver todos comandos
2. **Configure** Ajuste `prometheus.config.json` para seu projeto
3. **Integre** Adicione a CI/CD e pre-commit hooks
4. **Monitore** Use Guardian para acompanhar saúde
5. **Estenda** Crie analistas customizados conforme necessário

---

## Suporte

- [Documentação Completa](../INDICE.md)
- [Arquitetura](../arquitetura/ARVORE-ARQUITETURAL.md)
- [Criar Analista](../desenvolvimento/analistas/CRIAR-ANALISTA.md)
- [Referência de Comandos](../referencia/comandos/README.md)
