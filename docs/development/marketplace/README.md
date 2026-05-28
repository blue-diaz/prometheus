---
Proveniência e Autoria: Este documento integra o projeto Prometheus (licença MIT-0).
---

# [CHAVE-INGLESA] Marketplace de Analistas — Guia de Desenvolvimento

Guia completo para entender a arquitetura, estender e contribuir com o sistema de Marketplace de Analistas do Prometheus.

---

## Visão Geral

O Marketplace é um sistema que conecta o Prometheus a um ecossistema de analistas comunitários. Ele gerencia o ciclo de vida completo: descoberta, instalação, versionamento, remoção e publicação de analistas externos.

### Arquitetura

```
┌─────────────────────────────────────────────────────────┐
│                     CLI (comando-marketplace)            │
│  search  install  remove  list  info  publish           │
└────────────────────┬────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────┐
│                  API REST (routes/marketplace)           │
│  GET /search  GET /packages  GET /installed             │
└────────────────────┬────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────┐
│              Core (src/shared/marketplace/)              │
│  registry.ts    remote.ts    versionamento.ts            │
└───────┬────────────────────┬───────────────────────────-┘
        │                    │
        ▼                    ▼
  .prometheus/marketplace/   Registries Remotos
  (pacotes-instalados.json)  (URLs / GitHub)
```

---

## Componentes

### Tipos — `src/types/comum/marketplace.ts`

```typescript
// Informação de versão de um analista
interface MarketplacePackageVersion {
  version: string;        // ex: "1.2.0"
  semver: string;         // ex: "^1.0.0"
  description: string;
  author: string;
  license: string;
  minPrometheusVersion: string;
  url?: string;
  checksum?: string;
  publicadoEm?: number;
}

// Analista completo no marketplace
interface MarketplaceAnalista {
  nome: string;
  descricao: string;
  categoria: string;        // ex: "segurança", "qualidade"
  versoes: MarketplacePackageVersion[];
  tags: string[];
  homepage?: string;
  repository?: string;
  instalado?: boolean;
  versaoInstalada?: string;
  dataInstalacao?: number;
}

// Configuração de registry remoto
interface MarketplaceRemoteRegistry {
  nome: string;
  url: string;
  tipo: 'github' | 'npm' | 'url';
  habilitado: boolean;
}

// Pacote instalado localmente
interface MarketplacePackageLocal {
  nome: string;
  versao: string;
  descricao: string;
  caminho: string;
  dataInstalacao: number;
  autor: string;
  licenca: string;
  categoria: string;
  tags: string[];
}
```

### Registry Local — `src/shared/marketplace/registry.ts`

Gerencia os analistas instalados no projeto. Persiste o estado em `.prometheus/marketplace/pacotes-instalados.json`.

| Função                           | Descrição                                     |
| -------------------------------- | --------------------------------------------- |
| `listarPacotesInstalados()`      | Retorna todos os pacotes instalados           |
| `pacoteEstaInstalado(nome)`      | Verifica se um pacote específico existe       |
| `instalarPacote(analista, v)`    | Instala e persiste um novo pacote             |
| `removerPacote(nome)`            | Remove um pacote instalado                    |
| `obterInfoPacote(nome)`          | Obtém detalhes de um pacote instalado         |
| `buscarPacotesInstalados(termo)` | Busca por nome/descrição/tags                 |
| `listarCategoriasInstaladas()`   | Lista categorias únicas dos instalados        |

### Registry Remoto — `src/shared/marketplace/remote.ts`

Conecta-se a registries remotos para buscar e descobrir analistas.

| Função                          | Descrição                                      |
| ------------------------------- | ---------------------------------------------- |
| `listarRegistries()`            | Lista registries configurados                  |
| `buscarNoRegistry(termo)`       | Busca analistas em todos os registries ativos  |
| `obterAnalistaDoRegistry(nome)` | Obtém analista específico do registry          |
| `search(termo)`                 | Interface de busca (retorna `SearchResult`)    |
| `info(nome)`                    | Interface de informação de analista            |

### Versionamento — `src/shared/marketplace/versionamento.ts`

Resolução de versões usando semver.

| Função                               | Descrição                                         |
| ------------------------------------ | ------------------------------------------------- |
| `resolverMelhorVersao(versoes, req)` | Escolhe a melhor versão que satisfaz o req        |
| `ordenarVersoes(versoes)`            | Ordena versões da mais recente para a mais antiga |
| `versaoMaisRecente(versoes)`         | Retorna a versão mais recente                     |

---

## Formato do Registry Remoto

Registries remotos devem expor um JSON com a seguinte estrutura:

```json
{
  "analistas": [
    {
      "nome": "analista-seguranca",
      "descricao": "Detecta vulnerabilidades comuns",
      "categoria": "seguranca",
      "tags": ["seguranca", "auditoria"],
      "versoes": [
        {
          "version": "1.2.0",
          "semver": "^1.0.0",
          "description": "Versao estavel",
          "author": "comunidade",
          "license": "MIT",
          "minPrometheusVersion": "0.8.0"
        }
      ]
    }
  ]
}
```

---

## Fluxo de Instalação

```
Usuário executa: prometheus marketplace install analista-seguranca

1. CLI chama remote.info('analista-seguranca')
2. Remote percorre registries configurados
3. Registry responde com MarketplaceAnalista
4. CLI chama versionamento.resolverMelhorVersao(versoes, 'latest')
5. CLI chama registry.instalarPacote(analista, versao)
6. Registry persiste em .prometheus/marketplace/pacotes-instalados.json
7. CLI exibe confirmação
```

---

## Adicionar um Novo Registry

1. Crie/edite `.prometheus/marketplace/registries.json`
2. Adicione a entrada no formato `MarketplaceRemoteRegistry`
3. O registry será consultado em todas as buscas

```json
[
  {
    "nome": "meu-registry",
    "url": "https://exemplo.com/analistas.json",
    "tipo": "url",
    "habilitado": true
  }
]
```

---

## Publicar um Analista

O comando `marketplace publish` valida o arquivo do analista e orienta o usuário:

```bash
prometheus marketplace publish ./src/analistas/plugins/meu-analista.ts
```

Para disponibilizar para a comunidade:

1. Crie um fork do repositório `blue-diaz/prometheus-marketplace`
2. Adicione seu analista ao diretório `analistas/`
3. Atualize o `index.json` com as informações do seu analista
4. Envie um Pull Request

### Estrutura esperada do arquivo de analista

O arquivo deve exportar um objeto compatível com a interface `Analista`:

```typescript
import { criarAnalista } from '@';

export const meuAnalista = criarAnalista({
  nome: 'meu-analista',
  categoria: 'qualidade',
  descricao: 'Meu analista customizado',
  tags: ['custom', 'qualidade'],
  aplicar: (src, relPath) => {
    // lógica de análise
    return [];
  },
});
```

---

## Endpoints da API

| Método | Rota                                    | Descrição                        |
| ------ | --------------------------------------- | -------------------------------- |
| GET    | `/api/v1/marketplace/search?q=termo`    | Busca analistas no registry      |
| GET    | `/api/v1/marketplace/packages`          | Lista pacotes conhecidos         |
| GET    | `/api/v1/marketplace/packages/:nome`    | Info de um pacote (local+remoto) |
| GET    | `/api/v1/marketplace/installed`         | Lista analistas instalados       |
| GET    | `/api/v1/marketplace/installed?filter=` | Filtra instalados por termo      |

---

## Boas Práticas

- **Analistas** devem ter nomes únicos e descritivos (hífen separador: `analista-seguranca`)
- **Versões** seguem semver estrito (`MAJOR.MINOR.PATCH`)
- **Tags** ajudam na busca use categorias como `seguranca`, `desempenho`, `qualidade`, `react`, `python`
- **Categoria** deve ser uma das existentes ou uma nova bem definida
- **Descrição** curta (< 120 caracteres) para exibição em listas

---

## Solução de Problemas

| Problema                          | Causa                                     | Solução                                      |
| --------------------------------- | ----------------------------------------- | -------------------------------------------- |
| `Module '"@"' has no ...`         | Tipos do marketplace não exportados       | Verificar `src/types/comum/index.ts`         |
| Registry retorna 404              | URL do registry incorreta                 | Verificar `registries.json`                  |
| Pacote não persiste               | Permissão de escrita no `.prometheus/`    | Verificar permissões do diretório            |
| Conflito de versão                | Semver mal especificado                   | Usar `info` para ver versões disponíveis     |

---

> **Versão:** v0.8.0+ | **Última atualização:** 2026-05-22
