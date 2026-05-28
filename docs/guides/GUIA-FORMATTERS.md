<!-- @prometheus-disable pontuacao-repetida espacamento-incorreto -->
---
Proveniência e Autoria: Este documento integra o projeto Prometheus (licença MIT-0).
---

# Guia de Formatadores (Plugins de Saída)

O sistema de formatadores permite estender os formatos de saída dos relatórios do Prometheus. Além dos formatos nativos (JSON, Markdown), você pode criar plugins para gerar HTML, PDF, ou qualquer outro formato.

## Comandos

```bash
# Listar formatadores registrados
prometheus formatters list

# Listar em JSON (para processamento)
prometheus formatters list --json

# Registrar formatador externo
prometheus formatters register meu-formatter ./caminho/modulo.js

# Remover formatador
prometheus formatters remove meu-formatter
```

## Formatadores Built-in

| Nome                  | Formato    | Descrição                              |
| --------------------- | ---------- | -------------------------------------- |
| `json-builtin`        | json       | Exportador JSON nativo (.json)         |
| `markdown-builtin`    | markdown   | Exportador Markdown nativo (.md)       |
| `compliance-json`     | json       | Relatório de compliance JSON           |
| `compliance-markdown` | markdown   | Relatório de compliance Markdown       |

## Criando um Formatador Customizado

Um formatador é um módulo que exporta um objeto implementando a interface `Formatter`:

```typescript
// types/cli/formatters.ts
interface Formatter {
  nome: string;
  descricao: string;
  formato: string;     // 'json' | 'markdown' | 'html' | 'pdf' | 'custom'
  versao?: string;
  extensoes?: string[]; // extensões de arquivo associadas (ex: ['.html'])
  gerar(dados: DadosFormatacao, opcoes?: Partial<FormatterOpcoes>): string | Promise<string>;
}
```

### Exemplo: Formatador HTML

```javascript
// html-formatter.js
export default {
  nome: 'html-report',
  descricao: 'Gera relatório em HTML',
  formato: 'html',
  versao: '1.0.0',
  extensoes: ['.html', '.htm'],
  gerar(dados) {
    const { metadata, stats, ocorrencias } = dados;
    let html = `<!DOCTYPE html>
<html><head><title>Relatório Prometheus</title></head><body>`;
    html += `<h1>${metadata.projectNome || 'Relatório'}</h1>`;
    html += `<p>Total de ocorrências: ${stats.totalOcorrencias}</p>`;
    html += `<ul>${ocorrencias.map(o =>
      `<li>${o.relPath}:${o.linha} - ${o.mensagem}</li>`
    ).join('')}</ul>`;
    html += '</body></html>';
    return html;
  }
};
```

### Registrar e Usar

```bash
prometheus formatters register html-report ./html-formatter.js
prometheus formatters list
# html-report (html): Gera relatório em HTML
```

## Dados Disponíveis no Formatador

O parâmetro `dados` recebe a interface `DadosFormatacao`:

```typescript
interface DadosFormatacao {
  metadata: {
    timestamp: string;
    schemaVersion: string;
    modo: string;
    flags: string[];
    prometheusVersion?: string;
    projectNome?: string;
  };
  stats: {
    arquivosAnalisados: number;
    arquivosComProblemas: number;
    totalOcorrencias: number;
    porNivel: { erro: number; aviso: number; info: number };
    porCategoria: Record<string, number>;
    tempoExecucao?: number;
  };
  ocorrencias: Ocorrencia[];
  linguagens?: { total: number; extensoes: Record<string, number> };
  sugestoes?: string[];
}
```
