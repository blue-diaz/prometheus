<!-- @prometheus-disable pontuacao-repetida espacamento-incorreto -->
---
Proveniência e Autoria: Este documento integra o projeto Prometheus (licença MIT-0).
---

# Prometheus REST API

Documentação completa da API REST do Prometheus para análise estática de código.

## Versão

**v1** — Todos os endpoints estão sob o prefixo `/api/v1/`.

## Base URL

```
http://localhost:3100/api/v1
```

Porta configurável via variável de ambiente `PROMETHEUS_API_PORT`.

## Documentação Interativa

Acesse o Swagger UI em:

```
http://localhost:3100/api/docs
```

Especificação OpenAPI em JSON:

```
http://localhost:3100/api/docs.json
```

---

## Middlewares

A API utiliza os seguintes middlewares configurados automaticamente:

| Middleware       | Descrição                                               |
| ---------------- | ------------------------------------------------------- |
| CORS             | Origens configuráveis via `PROMETHEUS_API_CORS_ORIGINS` |
| Rate Limiting    | 100 req / 15 min (padrão), 10 req / min (estrito)       |
| Validação (Zod)  | Validação de body, query e params                       |
| Logging (Morgan) | Log detalhado de requisições                            |
| Error Handler    | Tratamento global de erros                              |
| Cache/ETag       | Headers de cache e ETags automáticos                    |

---

## Endpoints

### Health Check

Verifica se a API está operacional com métricas do sistema.

```
GET /api/health
```

**Resposta 200:**
```json
{
  "sucesso": true,
  "dados": {
    "status": "ok",
    "timestamp": 1716076800000,
    "versao": "0.7.0",
    "uptime": 3600.5,
    "memoria": {
      "heapUsedMB": 120,
      "heapTotalMB": 256,
      "externalMB": 15,
      "rssMB": 180,
      "usoPct": "46.9%"
    },
    "sistema": {
      "totalMemMB": 16384,
      "freeMemMB": 8192,
      "cpus": 8,
      "plataforma": "linux",
      "arquitetura": "x64"
    },
    "disco": {
      "acessoDiretorio": true
    }
  }
}
```

---

### Analistas

#### Listar todos os analistas (paginado)

```
GET /api/v1/analistas
```

**Parâmetros de query:**

| Parâmetro   | Tipo     | Obrigatório | Descrição                                  |
| ----------- | -------- | ----------- | ------------------------------------------ |
| `pagina`    | `number` | Não         | Número da página (padrão: 1)               |
| `porPagina` | `number` | Não         | Itens por página (padrão: 20)              |
| `categoria` | `string` | Não         | Filtrar por categoria                      |
| `busca`     | `string` | Não         | Busca por nome, descrição ou categoria     |

**Headers de resposta:**

| Header             | Descrição                |
| ------------------ | ------------------------ |
| `Total-Count`      | Total de itens           |
| `Page`             | Página atual             |
| `Per-Page`         | Itens por página         |

**Resposta 200:**
```json
{
  "sucesso": true,
  "dados": {
    "total": 30,
    "pagina": 1,
    "porPagina": 20,
    "totalPaginas": 2,
    "itens": [
      {
        "nome": "github-actions",
        "categoria": "CI/CD",
        "descricao": "Análise de workflows do GitHub Actions",
        "sempreAtivo": false
      }
    ]
  }
}
```

#### Estatísticas dos analistas

```
GET /api/v1/analistas/stats
```

**Resposta 200:**
```json
{
  "sucesso": true,
  "dados": {
    "total": 30,
    "sempreAtivos": 5,
    "categorias": 8,
    "porCategoria": {
      "CI/CD": 6,
      "JavaScript/TypeScript": 12,
      "Python": 3,
      "CSS": 4,
      "Outros": 5
    }
  }
}
```

#### Listar analistas por categoria

```
GET /api/v1/analistas/categorias
```

**Resposta 200:**
```json
{
  "sucesso": true,
  "dados": {
    "CI/CD": [
      {
        "nome": "github-actions",
        "categoria": "CI/CD",
        "descricao": "Análise de workflows do GitHub Actions",
        "sempreAtivo": false
      }
    ],
    "JavaScript/TypeScript": [
      {
        "nome": "funcoes-longas",
        "categoria": "JavaScript/TypeScript",
        "descricao": "Detecção de funções excessivamente longas",
        "sempreAtivo": true
      }
    ]
  }
}
```

#### Buscar analista por nome

```
GET /api/v1/analistas/:nome
```

**Resposta 200:**
```json
{
  "sucesso": true,
  "dados": {
    "nome": "github-actions",
    "categoria": "CI/CD",
    "descricao": "Análise de workflows do GitHub Actions",
    "sempreAtivo": false
  }
}
```

**Resposta 404:**
```json
{
  "sucesso": false,
  "erro": {
    "mensagem": "Analista 'inexistente' não encontrado"
  }
}
```

---

### Diagnóstico

Executa análise completa do projeto.

#### Executar diagnóstico (síncrono)

```
POST /api/v1/diagnosticar
```

**Body:**
```json
{
  "baseDir": "/caminho/do/projeto",
  "fast": false,
  "include": ["src/**/*.ts"],
  "exclude": ["**/*.test.ts", "**/node_modules/**"],
  "analistas": ["github-actions", "funcoes-longas"],
  "async": false
}
```

**Parâmetros do body:**

| Parâmetro   | Tipo       | Obrigatório | Descrição                                                           |
| ----------- | ---------- | ----------- | ------------------------------------------------------------------- |
| `baseDir`   | `string`   | Não         | Diretório base para análise (padrão: `process.cwd()`)               |
| `fast`      | `boolean`  | Não         | Modo rápido: analisa apenas `src/` e exclui CI/CD (padrão: `false`) |
| `include`   | `string[]` | Não         | Padrões de inclusão (glob)                                          |
| `exclude`   | `string[]` | Não         | Padrões de exclusão (glob)                                          |
| `analistas` | `string[]` | Não         | Lista de analistas específicos para executar                        |
| `async`     | `boolean`  | Não         | Executar em background como job (padrão: `false`)                   |

**Resposta 200 (síncrono):**
```json
{
  "sucesso": true,
  "dados": {
    "totalArquivos": 150,
    "totalOcorrencias": 42,
    "duracaoMs": 3500,
    "timestamp": 1716076800000,
    "ocorrencias": [
      {
        "relPath": "src/utils.ts",
        "linha": 45,
        "coluna": 10,
        "tipo": "tipo-inseguro-any",
        "nivel": "erro",
        "mensagem": "Uso de 'any' desabilita verificação de tipos",
        "sugestao": "Use tipo específico ou unknown com validação"
      }
    ]
  }
}
```

**Resposta 202 (assíncrono):**
```json
{
  "sucesso": true,
  "dados": {
    "jobId": "job_1716076800000_abc123",
    "status": "pendente",
    "mensagem": "Diagnóstico enfileirado. Consulte o status em GET /api/v1/diagnosticar/jobs/:jobId"
  }
}
```

#### Executar diagnóstico (assíncrono)

Para análises longas, use `async: true` no body. O diagnóstico é executado em background e você pode consultar o status.

```
POST /api/v1/diagnosticar
```
```json
{ "async": true, "baseDir": "/caminho/do/projeto" }
```

#### Listar jobs de diagnóstico

```
GET /api/v1/diagnosticar/jobs
```

**Resposta 200:**
```json
{
  "sucesso": true,
  "dados": {
    "total": 3,
    "jobs": [
      {
        "id": "job_1716076800000_abc123",
        "status": "completo",
        "criadoEm": 1716076800000,
        "finalizadoEm": 1716076805000,
        "resultado": {
          "totalArquivos": 150,
          "totalOcorrencias": 42,
          "duracaoMs": 3500
        }
      }
    ]
  }
}
```

#### Consultar status de um job

```
GET /api/v1/diagnosticar/jobs/:jobId
```

**Resposta 200:**
```json
{
  "sucesso": true,
  "dados": {
    "id": "job_1716076800000_abc123",
    "status": "executando",
    "criadoEm": 1716076800000
  }
}
```

**Estados possíveis do job:**

| Estado       | Descrição                            |
| ------------ | ------------------------------------ |
| `pendente`   | Aguardando execução                  |
| `executando` | Em progresso                         |
| `completo`   | Finalizado com sucesso               |
| `erro`       | Finalizado com erro (campo `erro`)   |

**Resposta 404:**
```json
{
  "sucesso": false,
  "erro": {
    "mensagem": "Job 'job_invalido' não encontrado"
  }
}
```

---

### Guardian

#### Status do Guardian

```
GET /api/v1/guardian
```

**Resposta 200:**
```json
{
  "sucesso": true,
  "dados": {
    "habilitado": true,
    "status": "baseline_existente",
    "baselineExiste": true,
    "ultimaVerificacao": 1716076800000,
    "alteracoes": []
  }
}
```

#### Criar baseline

```
POST /api/v1/guardian/baseline
```

**Body:**
```json
{
  "baseDir": "/caminho/do/projeto"
}
```

**Resposta 200:**
```json
{
  "sucesso": true,
  "dados": {
    "mensagem": "Baseline criado com sucesso",
    "totalArquivos": 150,
    "timestamp": 1716076800000
  }
}
```

#### Comparar com baseline (diff)

```
GET /api/v1/guardian/diff
```

**Parâmetros de query:**

| Parâmetro | Tipo     | Obrigatório | Descrição                                |
| --------- | -------- | ----------- | ---------------------------------------- |
| `baseDir` | `string` | Não         | Diretório base (padrão: `process.cwd()`) |

**Resposta 200:**
```json
{
  "sucesso": true,
  "dados": {
    "baselineTimestamp": 1716076800000,
    "atualTimestamp": 1716080400000,
    "arquivosAdicionados": ["src/novo.ts"],
    "arquivosRemovidos": ["src/antigo.ts"],
    "arquivosModificados": ["src/utils.ts"],
    "integridade": "comprometida"
  }
}
```

**Resposta 400 (sem baseline):**
```json
{
  "sucesso": false,
  "erro": {
    "mensagem": "Nenhum baseline encontrado. Crie um primeiro com POST /baseline"
  }
}
```

#### Histórico de verificações

```
GET /api/v1/guardian/historico
```

**Parâmetros de query:**

| Parâmetro | Tipo     | Obrigatório | Descrição                                |
| --------- | -------- | ----------- | ---------------------------------------- |
| `limit`   | `number` | Não         | Número de registros (padrão: 20)         |

**Resposta 200:**
```json
{
  "sucesso": true,
  "dados": {
    "total": 5,
    "registros": [
      {
        "timestamp": 1716076800000,
        "status": "ok",
        "totalArquivos": 150,
        "alteracoes": 0,
        "duracaoMs": 2500
      }
    ]
  }
}
```

#### Executar verificação

```
POST /api/v1/guardian/verificar
```

**Body:**
```json
{
  "baseDir": "/caminho/do/projeto"
}
```

**Resposta 200:**
```json
{
  "sucesso": true,
  "dados": {
    "status": "ok",
    "timestamp": "2026-05-18T23:43:42.649Z",
    "detalhes": []
  }
}
```

---

### Métricas

#### Histórico de métricas

```
GET /api/v1/metricas
```

**Parâmetros de query:**

| Parâmetro | Tipo     | Obrigatório | Descrição                                |
| --------- | -------- | ----------- | ---------------------------------------- |
| `limit`   | `number` | Não         | Número de análises recentes (padrão: 20) |
| `de`      | `number` | Não         | Timestamp inicial (epoch ms)             |
| `ate`     | `number` | Não         | Timestamp final (epoch ms)               |

**Resposta 200:**
```json
{
  "sucesso": true,
  "dados": {
    "totalAnalises": 150,
    "ultimasAnalises": [
      {
        "timestamp": 1716076800000,
        "totalArquivos": 150,
        "totalOcorrencias": 42,
        "duracaoMs": 3500
      }
    ]
  }
}
```

#### Resumo de métricas

```
GET /api/v1/metricas/resumo
```

**Resposta 200:**
```json
{
  "sucesso": true,
  "dados": {
    "totalAnalises": 150,
    "mediaOcorrencias": 38,
    "mediaArquivos": 145,
    "mediaDuracaoMs": 3200,
    "tendencia": "descendo",
    "ultimaAnalise": {
      "timestamp": 1716076800000,
      "totalArquivos": 150,
      "totalOcorrencias": 42
    }
  }
}
```

**Estados de tendência:**

| Tendência  | Descrição                                       |
| ---------- | ----------------------------------------------- |
| `subindo`  | Ocorrências aumentando (>10% vs média anterior) |
| `descendo` | Ocorrências diminuindo (>10% vs média anterior) |
| `estavel`  | Variação dentro de ±10%                         |

---

## Formato de Resposta

Todas as respostas seguem o padrão:

**Sucesso:**
```json
{
  "sucesso": true,
  "dados": { ... }
}
```

**Erro:**
```json
{
  "sucesso": false,
  "erro": {
    "mensagem": "Descrição do erro",
    "detalhes": "Informações adicionais (opcional)"
  }
}
```

**Erro de validação:**
```json
{
  "sucesso": false,
  "erro": {
    "mensagem": "Dados inválidos",
    "detalhes": [
      { "campo": "baseDir", "mensagem": "String must contain at least 1 character(s)" }
    ]
  }
}
```

## Códigos de Status HTTP

| Código | Descrição                           |
| ------ | ----------------------------------- |
| `200`  | Sucesso                             |
| `202`  | Aceito (job assíncrono enfileirado) |
| `400`  | Requisição inválida                 |
| `404`  | Recurso não encontrado              |
| `408`  | Requisição expirou                  |
| `429`  | Limite de requisições excedido      |
| `500`  | Erro interno do servidor            |

---

## Variáveis de Ambiente

| Variável                       | Descrição                                    | Padrão            |
| ------------------------------ | -------------------------------------------- | ----------------- |
| `PROMETHEUS_API_PORT`          | Porta do servidor API                        | `3100`            |
| `PROMETHEUS_API_CORS_ORIGINS`  | Origens permitidas (separadas por vírgula)   | `localhost: 000`  |
| `NODE_ENV`                     | Ambiente (`production` oculta stack traces)  | `development`     |

---

## Inicialização

### Servidor standalone

```typescript
import { iniciarServidorApi } from './api/server.js';

const servidor = await iniciarServidorApi(3100);

// Para graceful shutdown
await servidor.close();
```

### Via variável de ambiente

```bash
PROMETHEUS_API_PORT=3100 node dist/api/server.js
```

---

## Exemplos de Uso

### cURL

```bash
# Health check
curl http://localhost:3100/api/health

# Listar analistas (paginado)
curl "http://localhost:3100/api/v1/analistas?pagina=1&porPagina=10"

# Filtrar analistas por categoria
curl "http://localhost:3100/api/v1/analistas?categoria=js-ts"

# Buscar analistas por texto
curl "http://localhost:3100/api/v1/analistas?busca=funcoes"

# Estatísticas dos analistas
curl http://localhost:3100/api/v1/analistas/stats

# Executar diagnóstico (síncrono)
curl -X POST http://localhost:3100/api/v1/diagnosticar \
  -H "Content-Type: application/json" \
  -d '{"fast": true}'

# Executar diagnóstico (assíncrono)
curl -X POST http://localhost:3100/api/v1/diagnosticar \
  -H "Content-Type: application/json" \
  -d '{"async": true, "baseDir": "/meu/projeto"}'

# Consultar job
curl http://localhost:3100/api/v1/diagnosticar/jobs/job_1716076800000_abc123

# Status do Guardian
curl http://localhost:3100/api/v1/guardian

# Criar baseline
curl -X POST http://localhost:3100/api/v1/guardian/baseline \
  -H "Content-Type: application/json" \
  -d '{"baseDir": "/meu/projeto"}'

# Comparar com baseline
curl "http://localhost:3100/api/v1/guardian/diff"

# Histórico do Guardian
curl "http://localhost:3100/api/v1/guardian/historico?limit=10"

# Métricas com filtro de data
curl "http://localhost:3100/api/v1/metricas?de=1716000000000&ate=1716100000000"

# Resumo de métricas
curl http://localhost:3100/api/v1/metricas/resumo
```

### JavaScript/TypeScript

```typescript
const API_URL = 'http://localhost:3100/api/v1';

// Executar diagnóstico assíncrono
const response = await fetch(`${API_URL}/diagnosticar`, {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    async: true,
    fast: true,
    baseDir: '/meu/projeto'
  })
});

const { sucesso, dados } = await response.json();

if (sucesso) {
  const { jobId } = dados;
  console.log(`Job criado: ${jobId}`);

  // Polling até completar
  while (true) {
    const jobRes = await fetch(`${API_URL}/diagnosticar/jobs/${jobId}`);
    const { dados: job } = await jobRes.json();

    if (job.status === 'completo') {
      console.log(`Encontradas ${job.resultado.totalOcorrencias} ocorrências`);
      break;
    }
    if (job.status === 'erro') {
      console.error(`Erro: ${job.erro}`);
      break;
    }

    await new Promise(r => setTimeout(r, 1000));
  }
}

// Resumo de métricas
const resumo = await fetch(`${API_URL}/metricas/resumo`);
const { dados } = await resumo.json();
console.log(`Tendência: ${dados.tendencia}`);
```

### Python

```python
import requests
import time

API_URL = "http://localhost:3100/api/v1"

# Executar diagnóstico assíncrono
response = requests.post(
    f"{API_URL}/diagnosticar",
    json={"async": True, "fast": True}
)

data = response.json()
if data["sucesso"]:
    job_id = data["dados"]["jobId"]

    # Polling até completar
    while True:
        job_res = requests.get(f"{API_URL}/diagnosticar/jobs/{job_id}")
        job = job_res.json()["dados"]

        if job["status"] == "completo":
            print(f"Encontradas {job['resultado']['totalOcorrencias']} ocorrências")
            break
        if job["status"] == "erro":
            print(f"Erro: {job['erro']}")
            break

        time.sleep(1)
```
