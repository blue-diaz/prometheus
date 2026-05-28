---
Proveniência e Autoria: Este documento integra o projeto Prometheus (licença MIT-0).
---

# [LOJA] Marketplace de Analistas — Prometheus

O Marketplace de Analistas é o ecossistema comunitário de analistas e detectores para o Prometheus. Permite descobrir, instalar e gerenciar analistas criados pela comunidade, estendendo a capacidade de análise do Prometheus para além dos analistas embutidos.

---

## Para que serve

- **Descobrir** analistas prontos para problemas específicos (segurança, desempenho, boas práticas por linguagem)
- **Instalar** analistas de terceiros sem configurar plugins manualmente
- **Versionar** — cada analista segue semver, garantindo compatibilidade
- **Publicar** seus próprios analistas para a comunidade

---

## Pré-requisitos

- Prometheus v0.8.0+
- Acesso à internet para busca remota
- Comando executado dentro de um projeto Prometheus

---

## Comandos

### `marketplace search <termo>`

Busca analistas disponíveis nos registries remotos.

```bash
# Busca genérica
prometheus marketplace search qualidade

# Busca por tecnologia específica
prometheus marketplace search react

# Saída JSON (para pipes/scripts)
prometheus marketplace search seguranca --json
```

### `marketplace install <nome>`

Instala um analista do marketplace.

```bash
# Instala a versão mais recente
prometheus marketplace install analista-seguranca

# Instala uma versão específica
prometheus marketplace install analista-seguranca --version 1.2.0

# Instala de um registry específico
prometheus marketplace install analista-seguranca --registry https://meu-registry.com/index.json
```

### `marketplace list`

Lista todos os analistas instalados localmente.

```bash
# Lista completa
prometheus marketplace list

# Filtra por termo
prometheus marketplace list --filter seguranca

# Saída JSON
prometheus marketplace list --json
```

### `marketplace info <nome>`

Exibe informações detalhadas de um analista (remoto e local).

```bash
prometheus marketplace info analista-seguranca
```

Saída esperada:

```
Informações do Analista: analista-seguranca
  Versão instalada: 1.2.0
  Descrição: Detecta vulnerabilidades comuns em código
  Autor: comunidade
  Licença: MIT
  Categoria: segurança
  Instalado em: 22/05/2026 10:30:00
  Tags: segurança, vulnerabilidade, auditoria
```

### `marketplace remove <nome>`

Remove um analista instalado.

```bash
prometheus marketplace remove analista-seguranca
```

### `marketplace publish <caminho>`

Prepara um analista local para publicação no marketplace comunitário.

```bash
prometheus marketplace publish ./meu-analista.ts
```

Valida o arquivo e exibe as instruções para submissão ao repositório comunitário.

---

## API REST

O marketplace também expõe endpoints REST para integração programática.

### Listar instalados

```bash
GET /api/v1/marketplace/installed
GET /api/v1/marketplace/installed?filter=seguranca
```

### Buscar no registry remoto

```bash
GET /api/v1/marketplace/search?q=react
```

### Informações de um pacote

```bash
GET /api/v1/marketplace/packages/analista-seguranca
```

### Listar todos os pacotes conhecidos

```bash
GET /api/v1/marketplace/packages
```

---

## Registries Remotos

Por padrão, o marketplace consulta o registry oficial da comunidade:

```
https://raw.githubusercontent.com/blue-diaz/prometheus-marketplace/main/index.json
```

### Adicionar um registry customizado

Crie o arquivo `.prometheus/marketplace/registries.json`:

```json
[
  {
    "nome": "comunidade",
    "url": "https://raw.githubusercontent.com/blue-diaz/prometheus-marketplace/main/index.json",
    "tipo": "url",
    "habilitado": true
  },
  {
    "nome": "empresa",
    "url": "https://registry.internal.empresa.com/prometheus/analistas.json",
    "tipo": "url",
    "habilitado": true
  }
]
```

### Tipos de registry suportados

| Tipo     | Descrição                                         |
| -------- | ------------------------------------------------- |
| `url`    | URL HTTP/HTTPS para um JSON de índice             |
| `github` | Repositório GitHub (formato `owner/repo`)         |
| `npm`    | Pacote npm (futuro)                               |

---

## Resolução de Versões (Semver)

O marketplace usa semver para gerenciar versões de analistas.

| Especificador | Comportamento                           |
| ------------- | --------------------------------------- |
| `latest`      | Versão mais recente                     |
| `^1.2.0`      | Compatível com 1.x (major fixo)         |
| `~1.2.0`      | Compatível com 1.2.x (major+minor fixo) |
| `1.2.0`       | Versão exata                            |

---

## Solução de Problemas

| Problema                       | Causa provável                      | Solução                                         |
| ------------------------------ | ----------------------------------- | ----------------------------------------------- |
| `Registry indisponível`        | Registry remoto fora do ar          | Verifique a URL e a conectividade               |
| `Analista não encontrado`      | Nome incorreto ou registry vazio    | Use `search` para confirmar o nome              |
| `Versão não disponível`        | Versão solicitada não existe        | Use `info` para ver versões disponíveis         |
| `Pacote não está instalado`    | Nome errado no `remove`             | Use `list` para ver os instalados               |
| Erro de permissão              | Sem escrita no diretório do projeto | Verifique permissões da pasta `.prometheus`     |

---

## Próximos passos

- Explore o [Guia de Início Rápido](./GUIA-INICIO-RAPIDO.md) para configuração básica
- Crie seu próprio analista com o [Guia de Criação de Analistas](../desenvolvimento/analistas/CRIAR-ANALISTA.md)
- Publique no marketplace comunitário (instruções no `publish`)
- Consulte a [Documentação de Desenvolvimento do Marketplace](../desenvolvimento/marketplace/README.md)

---

> **Versão:** v0.8.0+ | **Última atualização:** 2026-05-22
