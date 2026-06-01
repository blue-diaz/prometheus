<!-- @prometheus-disable pontuacao-repetida -->
---
Proveniência e Autoria: Documentação completa do Prometheus (licença MIT-0).
---

# GitHub App - Prometheus

Guia completo para configurar, usar e estender o Prometheus como GitHub App.

---

## Visão Geral

O Prometheus pode operar como um **GitHub App** que analisa automaticamente workflows do GitHub Actions e fornece feedback direto nos repositórios. Ele responde a eventos como pull requests, pushes, reviews e issues, entregando análises de qualidade, segurança e performance.

### Funcionalidades

- **Análise automática** de workflows em pull requests e pushes
- **Score de qualidade** para cada workflow (0-100)
- **Detecção de problemas** de segurança, performance e governança
- **Comentários automáticos** em PRs e issues
- **Check runs** com annotations no código
- **Análise sob demanda** via comandos em reviews e issues
- **Sistema de plugins** para detectores customizados

---

## Configuração

### 1. Criar o GitHub App

1. Acesse [GitHub Settings > Developer Settings > GitHub Apps](https://github.com/settings/apps)
2. Clique em **New GitHub App**
3. Configure:
   - **GitHub App name**: `Prometheus Analyzer`
   - **Homepage URL**: URL do seu projeto
   - **Webhook URL**: `https://seu-dominio.com/api/github/webhooks`
   - **Setup URL** (pós-instalação): `https://seu-dominio.com/api/github/setup` — redireciona de volta ao GitHub (o usuário **não** volta ao dashboard)
   - **Webhook secret**: Gere uma string aleatória segura

4. **Permissions** necessárias:

   | Recurso               | Permissão        |
   |-----------------------|------------------|
   | `Contents`            | Read & Write     |
   | `Checks`              | Read & Write     |
   | `Pull requests`       | Read & Write     |
   | `Issues`              | Read & Write     |
   | `Commit statuses`     | Read & Write     |

5. **Subscribe to events**:
   - `Check suite`
   - `Pull request`
   - `Pull request review`
   - `Push`
   - `Issues`
   - `Installation`
   - `Installation repositories`

6. Gere uma **Private key** (RSA) e salve o arquivo `.pem`

### 2. Instalar o App

1. No painel do GitHub App, vá em **Install App**
2. Selecione os repositórios (todos ou específicos)

### 3. Configurar Variáveis de Ambiente

Crie um arquivo `.env` na raiz do projeto:

```env
# GitHub App Configuration
GITHUB_APP_ID=123456
GITHUB_PRIVATE_KEY="-----BEGIN RSA PRIVATE KEY-----\nMIIEpAIBAAKCAQEA...\n-----END RSA PRIVATE KEY-----"
GITHUB_WEBHOOK_SECRET=seu-secret-aqui

# Server Configuration
PROMETHEUS_API_PORT=3000
PROMETHEUS_PUBLIC_URL=https://seu-dominio.com

# Optional
NODE_ENV=production
```

> **Setup URL**: use `https://seu-dominio.com/api/github/setup` no campo *Setup URL* do GitHub App. Após instalar, o GitHub chama essa rota, a configuração é salva em disco e o usuário é redirecionado para `https://github.com/settings/installations/{id}`.

> **Nota**: A chave privada deve estar em uma única linha com `\n` representando quebras de linha, ou em múltiplas linhas dentro de aspas.

---

## Iniciar o Servidor

### Via CLI

```bash
prometheus github-actions app
# ou
prometheus github-actions app --port 3000
```

### Via npm

```bash
npm run start
```

### Logs de inicialização

```
[LANCAMENTO] Servidor rodando na porta 3000
[APONTAR-DIREITA] Webhooks: http://localhost:3000/api/github/webhooks
[APONTAR-DIREITA] REST API: http://localhost:3000/api
[APONTAR-DIREITA] Health: http://localhost:3000/api/health
```

---

## Eventos Webhook Suportados

### 1. Check Suite

**Eventos**: `check_suite.requested`, `check_suite.rerequested`

**Comportamento**:
- Cria um check run com status `in_progress`
- Analisa todos os workflows do repositório
- Publica resultado com score, ocorrências e annotations

**Quando dispara**:
- Quando um novo check suite é criado (ex: após um push)
- Quando um check é re-requested

**Exemplo de output**:
```
## Análise de Workflows do GitHub Actions

**Score Geral:** 85/100

### Ocorrências (3)

- **AVISO**: `.github/workflows/ci.yml:15` - Falta de pinning por SHA na action actions/checkout
- **ERRO**: `.github/workflows/ci.yml:23` - Possível script injection via github.event.issue.title
- **INFO**: `.github/workflows/ci.yml:45` - Considere habilitar cache para dependências
```

---

### 2. Pull Request

**Eventos**: `pull_request.opened`, `pull_request.synchronize`, `pull_request.reopened`

**Comportamento** (análise completa, sem workflows no repo):
- Baixa o código no commit do PR e executa todos os analistas
- Escopo incremental: arquivos alterados entre `base` e `head` do PR
- Comenta no PR com resumo + relatório completo (seção recolhível)
- Publica comentários inline nos trechos relevantes
- Ignora PRs cuja branch começa com `prometheus/` (relatórios automáticos)

**Quando dispara**:
- PR aberto, novos commits (synchronize) ou PR reaberto

**Configuração**: `pullRequestAnalysisEnabled` em `PATCH /api/github/instalacoes/:id/config`

---

### 3. Push

**Evento**: `push`

**Comportamento** (sem GitHub Actions no repositório alvo):
- Baixa o código no commit do push e executa a **análise completa** (todos os analistas)
- Com análise incremental ativa, prioriza arquivos alterados no push (`compare` API)
- Grava o relatório em `.prometheus/reports/analise-{branch}.md`
- Abre ou atualiza um PR `prometheus/analise-{branch}` → `{branch}`
- Publica comentários inline nos arquivos (review comments) com os principais achados

**Quando dispara**:
- Push em branches (`refs/heads/*`), não em tags

**Configuração por instalação**: `GET/PATCH /api/github/instalacoes/:id/config` (`pushAnalysisEnabled`, `pullRequestAnalysisEnabled`, `fast`, `incrementalOnPush`)

---

### 4. Pull Request Review

**Evento**: `pull_request_review.submitted`

**Comportamento**:
- Responde a menções `@prometheus analyze` em reviews
- Executa análise sob demanda e posta resultado

**Quando dispara**:
- Quando um review é submetido contendo `@prometheus analyze`

**Como usar**:
1. Abra um review no PR
2. Inclua `@prometheus analyze` no comentário
3. Submeta o review
4. O Prometheus responderá com a análise

**Exemplo**:
```
@prometheus analyze

---

## Prometheus - Análise Sob Demanda

**Score Geral:** 90/100

- [OK] ci.yml (Score: 95/100)
- [OK] deploy.yml (Score: 85/100)

Nenhuma ocorrência encontrada.
```

---

### 5. Issues

**Evento**: `issues.opened`

**Comportamento**:
- Responde a menções `@prometheus scan` em issues
- Executa scan completo dos workflows do repositório

**Quando dispara**:
- Quando uma issue é criada contendo `@prometheus scan`

**Como usar**:
1. Crie uma nova issue
2. Inclua `@prometheus scan` no corpo
3. O Prometheus responderá com o scan

**Exemplo**:
```
@prometheus scan

---

## Prometheus - Scan Sob Demanda

**Score Geral:** 78/100

- [OK] ci.yml (Score: 85/100, 2 ocorrências)
- [AVISO] deploy.yml (Score: 70/100, 5 ocorrências)

### Principais Ocorrências

#### Erros (1)

- `.github/workflows/deploy.yml:12` - Hardcoded secret detectado

#### Avisos (6)

- `.github/workflows/ci.yml:15` - Falta de pinning por SHA
...
```

---

## Comandos CLI

### Scan Local

Analisa workflows no diretório local:

```bash
prometheus github-actions scan
prometheus github-actions scan --path /caminho/do/repo
prometheus github-actions scan --workflow ci.yml
prometheus github-actions scan --json
prometheus github-actions scan --compact
```

**Opções**:

| Flag               | Descrição                              |
|--------------------|----------------------------------------|
| `-p, --path`       | Caminho para o repositório (padrão: `.`) |
| `-w, --workflow`   | Analisar workflow específico           |
| `-j, --json`       | Saída em formato JSON                  |
| `--compact`        | Saída compacta (uma linha)             |
| `--fix`            | Aplica correções automaticamente       |
| `--dry-run`        | Visualiza correções sem aplicar        |

### Gerar Relatório

```bash
prometheus github-actions report
prometheus github-actions report --format json
prometheus github-actions report --format html --output relatorio.html
prometheus github-actions report --titulo "Análise Semanal"
prometheus github-actions report --no-detalhes
```

**Opções**:

| Flag                 | Descrição                                    |
|----------------------|----------------------------------------------|
| `-p, --path`         | Caminho para o repositório                   |
| `-f, --format`       | Formato: `json`, `markdown`, `html`, `csv`   |
| `-o, --output`       | Arquivo de saída                             |
| `--titulo`           | Título do relatório                          |
| `--no-detalhes`      | Excluir detalhes das ocorrências             |

### Gate de Qualidade

Para uso em CI/CD - falha se o score estiver abaixo do threshold:

```bash
prometheus github-actions gate
prometheus github-actions gate --threshold 80
prometheus github-actions gate --fail-on aviso
prometheus github-actions gate --json
```

**Opções**:

| Flag                | Descrição                                      |
|---------------------|------------------------------------------------|
| `-p, --path`        | Caminho para o repositório                     |
| `-t, --threshold`   | Score mínimo aceitável (0-100, padrão: 70)     |
| `-w, --workflow`    | Verificar workflow específico                  |
| `-j, --json`        | Saída JSON para consumo externo                |
| `--fail-on`         | Nível mínimo para falhar: `erro`, `aviso`, `info` |

**Exemplo de uso em CI**:

```yaml
- name: Quality Gate
  run: prometheus github-actions gate --threshold 80 --fail-on erro
```

### Servidor GitHub App

```bash
prometheus github-actions app
prometheus github-actions app --port 3000
```

---

## Sistema de Plugins

### Criando um Plugin Customizado

Plugins permitem adicionar detectores personalizados:

```typescript
import { registrarDetectorGithubActions } from 'prometheus';

registrarDetectorGithubActions({
  nome: 'no-sudo-allowed',
  descricao: 'Detecta uso proibido de sudo em runners',
  severidade: 'alta',
  testar: (workflow, context) => {
    const problemas = [];
    if (context.conteudo.includes('sudo ')) {
      problemas.push({
        tipo: 'security-policy',
        descricao: 'Uso de sudo detectado. Runners devem ser não-root.',
        severidade: 'alta',
        sugestao: 'Remover sudo ou usar containers com usuários específicos.'
      });
    }
    return problemas;
  }
});
```

### Interface do Plugin

```typescript
interface DeteccaoCustom {
  nome: string;
  descricao: string;
  severidade: 'baixa' | 'media' | 'alta' | 'critica';
  testar: (
    workflow: unknown,
    context: { conteudo: string; caminhos: string[] }
  ) => ProblemaWorkflow[] | Promise<ProblemaWorkflow[]>;
  corrigir?: (doc: Document) => void;
}

interface GitHubActionsPlugin {
  nome: string;
  versao: string;
  descricao: string;
  autor: string;
  detecoes: DeteccaoCustom[];
}
```

### Detectores Incluídos

| Detector                          | Categoria    | Descrição                                  |
|-----------------------------------|--------------|--------------------------------------------|
| `detector-workflow-security`      | Segurança    | Script injection, secrets, permissões      |
| `detector-workflow-performance`   | Performance  | Caching, jobs redundantes, matrix strategy |
| `detector-workflow-estrutura`     | Estrutura    | Nomenclatura, organização, jobs órfãos     |
| `detector-workflow-compliance`    | Compliance   | Políticas organizacionais, CODEOWNERS      |
| `detector-workflow-acessibilidade`| Acessibilidade| Labels, documentação, issue templates      |
| `detector-workflow-trigger-inseguro`| Segurança | Triggers perigosos, pull_request_target    |
| `detector-dependencias-vulneraveis`| Segurança  | Dependências com vulnerabilidades conhecidas|

---

## SDK para Desenvolvedores

### Uso Programático

```typescript
import { PrometheusSDK } from 'prometheus';

const sdk = new PrometheusSDK();

// Analisar conteúdo de workflow
const ocorrencias = await sdk.analisarGithubActions(conteudoYAML, '.github/workflows/ci.yml');

console.log(ocorrencias);
// [
//   {
//     relPath: '.github/workflows/ci.yml',
//     linha: 15,
//     nivel: 'aviso',
//     mensagem: 'Falta de pinning por SHA...',
//     sugestao: 'Use hash SHA ao invés de versão...'
//   }
// ]
```

### Registrar Detector Customizado

```typescript
import { registrarDetectorGithubActions } from 'prometheus';

registrarDetectorGithubActions({
  nome: 'meu-detector',
  descricao: 'Minha regra customizada',
  severidade: 'media',
  testar: (workflow, context) => {
    // Sua lógica aqui
    return [];
  }
});
```

---

## Deploy em Produção

### Requisitos

- Node.js ≥ 24.15.0
- HTTPS obrigatório para webhooks
- Domínio público acessível pelo GitHub

### Opções de Deploy

#### 1. Node.js Direto

```bash
NODE_ENV=production node dist/bin/index.js github-actions app --port 3000
```

#### 2. PM2

```bash
pm2 start dist/bin/index.js --name "prometheus-github-app" -- \
  github-actions app --port 3000
```

#### 3. Docker

```dockerfile
FROM node:24-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --production
COPY dist ./dist
EXPOSE 3000
CMD ["node", "dist/bin/index.js", "github-actions", "app", "--port", "3000"]
```

```bash
docker build -t prometheus-github-app .
docker run -d -p 3000:3000 \
  --env-file .env \
  prometheus-github-app
```

#### 4. Docker Compose

```yaml
version: '3.8'
services:
  prometheus:
    build: .
    ports:
      - "3000:3000"
    env_file:
      - .env
    restart: unless-stopped
```

### Proxy Reverso (Nginx)

```nginx
server {
    listen 443 ssl;
    server_name prometheus.seu-dominio.com;

    ssl_certificate /etc/letsencrypt/live/seu-dominio.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/seu-dominio.com/privkey.pem;

    location /api/github/webhooks {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### Webhook Proxy (Desenvolvimento)

Para desenvolvimento local, use o [smee.io](https://smee.io):

```bash
# Instalar cliente smee
npm install -g smee-client

# Criar canal
smee -u https://smee.io/abc123

# Encaminhar webhooks
smee --url https://smee.io/abc123 --target http://localhost:3000/api/github/webhooks
```

Configure a URL do smee.io como webhook no GitHub App.

---

## Solução de Problemas

### Webhook não está chegando

1. Verifique se a URL do webhook está acessível externamente
2. Verifique os logs do servidor
3. Use `smee.io` para testar localmente
4. Verifique se o `webhook_secret` está correto

### Erro de autenticação

1. Verifique se `GITHUB_APP_ID` está correto
2. Verifique se a private key está no formato correto
3. Verifique se o App está instalado no repositório alvo

### Score sempre 100

1. Verifique se os workflows estão sendo encontrados
2. Execute `prometheus github-actions scan --json` para debug
3. Verifique se os detectores estão registrados

### Logs de erro

```bash
# Ver logs do servidor
tail -f /var/log/prometheus.log

# Ver webhooks recebidos
curl http://localhost:3000/api/health
```

---

## Arquitetura

```
┌─────────────────┐     Webhook      ┌──────────────────┐
│   GitHub        │ ────────────────> │  Prometheus      │
│   Repositório   │                  │  Servidor        │
└─────────────────┘                  │  (Express)       │
                                     └────────┬─────────┘
                                              │
                               ┌──────────────┼──────────────┐
                               │              │              │
                    ┌──────────▼───┐  ┌──────▼──────┐  ┌────▼─────┐
                    │ Check Suite  │  │ Pull Request│  │ Push     │
                    │ Handler      │  │ Handler     │  │ Handler  │
                    └──────────┬───┘  └──────┬──────┘  └────┬─────┘
                               │              │              │
                               └──────────────┼──────────────┘
                                              │
                               ┌──────────────▼──────────────┐
                               │   Analista GitHub Actions   │
                               │   + Detectores + Plugins    │
                               └─────────────────────────────┘
```

---

## Referências

- [Documentação GitHub Apps](https://docs.github.com/en/apps)
- [Octokit.js Documentation](https://octokit.github.io/octokit.js/)
- [Webhooks Guide](https://docs.github.com/en/webhooks)
- [Checks API](https://docs.github.com/en/rest/checks)
- [REST API do Prometheus](./REST-API.md)
- [Sistema de Plugins](./dashboard-e-plugins-github.md)
