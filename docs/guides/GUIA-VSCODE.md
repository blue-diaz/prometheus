---
Proveniência e Autoria: Documentação completa do Prometheus (licença MIT-0).
---

# [COMPUTADOR] Extensão VSCode - Prometheus

> Análise de código em tempo real diretamente no seu editor.

---

## [LANCAMENTO] Visão Geral

A extensão **Prometheus VSCode** traz o poder da análise de código do Prometheus
diretamente para o editor, com diagnósticos em tempo real, quick fixes e um
painel interativo de resultados.

### Funcionalidades

- **Análise onSave**: detecta problemas ao salvar arquivos
- **Análise onOpen**: analisa arquivos ao abri-los
- **Diagnósticos no Problems**: problemas exibidos no painel Problems do VSCode
- **Análise de projeto completo**: command para analisar todo o workspace
- **Painel de resultados**: webview com estatísticas e detalhes
- **Quick fixes**: ações rápidas com links para documentação
- **Multi-linguagem**: suporte a 12 linguagens
- **Configurável**: ative/desative, escolha severidade mínima

---

## Índice

- [Instalação](#instalação)
- [Comandos](#comandos)
- [Configuração](#configuração)
- [Linguagens Suportadas](#linguagens-suportadas)
- [Estrutura do Projeto](#estrutura-do-projeto)
- [SDK Programático](#sdk-programático)
- [Desenvolvimento](#desenvolvimento)

---

## Instalação

### Via VSIX (recomendado)

```bash
# 1. Package a extensão
npx vsce package -o prometheus-vscode.vsix

# 2. Instale no VSCode
code --install-extension prometheus-vscode.vsix
```

### Via symlink (desenvolvimento local)

```bash
# Do diretório raiz do Prometheus
ln -sf "$(pwd)/extensions/vscode" ~/.vscode/extensions/prometheus.prometheus-vscode-0.9.0
```

### Pré-requisitos

- **VSCode** >= 1.85.0
- **Node.js** >= 24.16.0 (para o SDK)
- **Prometheus** (como dependência, já incluída)

---

## Comandos

| Comando | Descrição | Atalho |
|---|---|---|
| `Prometheus: Analisar Arquivo Atual` | Analisa o arquivo aberto no editor | `Ctrl+Shift+P` |
| `Prometheus: Analisar Projeto Completo` | Analisa todo o workspace | `Ctrl+Shift+P` |
| `Prometheus: Mostrar Painel de Resultados` | Abre o painel webview | `Ctrl+Shift+P` |
| `Prometheus: Limpar Diagnósticos` | Limpa todos os diagnósticos ativos | `Ctrl+Shift+P` |

---

## Configuração

A extensão expõe as seguintes opções via `settings.json`:

```jsonc
{
  // Habilita/desabilita análise em tempo real
  "prometheus.enabled": true,

  // Executa análise ao salvar arquivo
  "prometheus.runOnSave": true,

  // Executa análise ao abrir arquivo
  "prometheus.runOnOpen": false,

  // Número máximo de resultados por arquivo
  "prometheus.maxResults": 100,

  // Nível mínimo de severidade para exibir
  // Opções: "erro" | "aviso" | "info"
  "prometheus.severityLevel": "info"
}
```

### Mapeamento de Severidade

| Prometheus | VSCode |
|---|---|
| `erro` | `Error` (linha vermelha) |
| `critico` | `Error` (linha vermelha) |
| `aviso` | `Warning` (linha amarela) |
| `risco` | `Warning` (linha amarela) |
| `info` | `Information` (linha azul) |
| `sucesso` | `Information` (linha azul) |

---

## Linguagens Suportadas

A extensão ativa automaticamente para:

- JavaScript (`.js`)
- JavaScript React (`.jsx`)
- TypeScript (`.ts`)
- TypeScript React (`.tsx`)
- CSS/SCSS (`.css`, `.scss`)
- HTML (`.html`)
- Python (`.py`)
- Go (`.go`)
- Rust (`.rs`)
- Java (`.java`)
- Shell Script (`.sh`)
- XML (`.xml`)
- YAML (`.yaml`, `.yml`)

---

## Estrutura do Projeto

```
extensions/vscode/
├── .vscode/
│   └── launch.json           ← Config para debug (F5)
├── src/
│   ├── extension.ts           ← Ponto de entrada
│   ├── diagnosticProvider.ts  ← Providers de diagnóstico
│   ├── codeActionProvider.ts  ← Quick fixes
│   └── prometheusPanel.ts     ← Webview panel
├── out/                       ← Compilado (TypeScript → JS)
├── package.json               ← Manifest da extensão
├── tsconfig.json
└── .vscodeignore
```

---

## SDK Programático

A extensão consome o **Prometheus SDK** via `prometheus/sdk`.

```typescript
import { analyzeFile, analyzeProject } from 'prometheus/sdk';

// Analisar arquivo individual
const result = await analyzeFile('/caminho/arquivo.ts', conteudo);
// → { ocorrencias: Ocorrencia[], arquivo: string, tempoMs: number }

// Analisar projeto completo
const result = await analyzeProject('/caminho/projeto', { fast: true });
// → { ocorrencias: Ocorrencia[], totalArquivos: number, tempoMs: number }
```

### Tipos

```typescript
interface Ocorrencia {
  mensagem: string;     // Descrição do problema
  relPath: string;      // Caminho relativo do arquivo
  tipo?: string;        // Código do detector
  nivel?: string;       // Severidade: erro | aviso | info
  linha?: number;       // Linha (1-indexed)
  coluna?: number;      // Coluna (1-indexed)
  origem?: string;      // Fonte do diagnóstico
  sugestao?: string;    // Sugestão de correção
  correcao?: {          // Correção automática
    original: string;
    novo: string;
    linha: number;
  };
}
```

---

## Desenvolvimento

### Build

```bash
# Do diretório raiz do Prometheus
npm run build

# Depois compile a extensão
cd extensions/vscode
npm run compile
```

### Debug

1. Abra o workspace raiz do Prometheus no VSCode
2. Vá na aba *Run and Debug* (Ctrl+Shift+D)
3. Selecione *Run Extension*
4. Pressione F5

Uma nova janela VSCode será aberta com a extensão carregada.

### Hot Reload

Para testar mudanças:

```bash
# Compile a extensão
cd extensions/vscode && npm run compile

# Recarregue a janela do VSCode de desenvolvimento
# Ctrl+Shift+P → "Developer: Reload Window"
```

---

## [COMENTARIO] FAQ

| Pergunta | Resposta |
|---|---|
| "A análise está lenta" | Ajuste `prometheus.maxResults` para limitar resultados |
| "Quero desativar temporariamente" | Defina `prometheus.enabled: false` |
| "Não aparecem resultados" | Verifique se o arquivo está em uma linguagem suportada |
| "Como vejo detalhes?  | Use `Prometheus: Mostrar Painel de Resultados` |

---

## 🔗 Links

- **Repositório:** [Prometheus](https://github.com/blue-diaz/prometheus)
- **Issues:** [GitHub Issues](https://github.com/blue-diaz/prometheus/issues)
- **Roadmap:** [Roadmap](../roadmap/ROADMAP.md)
- **SDK:** Referência do SDK no código: `src/sdk/`

---

> **Versão:** 0.9.0 | **Compatível com:** Prometheus >= 0.8.9
