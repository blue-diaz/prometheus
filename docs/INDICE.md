---
Proveniência e Autoria: Documentação completa do projeto Prometheus (licença MIT-0).
---

# 📋 INDICE - Documentação Completa do Prometheus

> Navegação completa por tópico. Use como ponto de partida para explorar a documentação.

---

## [LANCAMENTO] Início Rápido

| Documento                                                    | Descrição                                 |
| ------------------------------------------------------------ | ----------------------------------------- |
| [Guias/GUIA-INICIO-RAPIDO.md](./guias/GUIA-INICIO-RAPIDO.md) | Tutorial de 10 minutos para começar       |
| [Guias/GUIA-CONFIGURACAO.md](./guias/GUIA-CONFIGURACAO.md)   | Configuração personalizada do projeto     |
| [Guias/GUIA-COMANDOS.md](./guias/GUIA-COMANDOS.md)           | Referência completa de todos os comandos  |
| [Guias/GUIA-MARKETPLACE.md](./guias/GUIA-MARKETPLACE.md)     | Marketplace de analistas comunitários     |
| [Guias/GUIA-VSCODE.md](./guias/GUIA-VSCODE.md)               | Extensão VSCode com análise em tempo real |
| [Guias/GUIA-COMPLIANCE.md](./guias/GUIA-COMPLIANCE.md)       | Relatórios de compliance (ISO, SOC2)      |
| [Guias/GUIA-FORMATTERS.md](./guias/GUIA-FORMATTERS.md)       | Plugins de formatadores de saída          |

## 🏗️ Arquitetura e Design

| Documento                                                                  | Descrição                            |
| -------------------------------------------------------------------------- | ------------------------------------ |
| [Arquitetura/ARVORE-ARQUITETURAL.md](./arquitetura/ARVORE-ARQUITETURAL.md) | Estrutura visual completa do projeto |
| [Arquitetura/README.md](./arquitetura/README.md)                           | Visão geral da arquitetura           |
| [Arquitetura/TYPE-SAFETY.md](./arquitetura/TYPE-SAFETY.md)                 | Garantias de tipo no TypeScript      |
| [Arquitetura/SISTEMA-ERROS.md](./arquitetura/SISTEMA-ERROS.md)             | Tratamento de erros e códigos        |
| [Arquitetura/MENSAGENS-LOGS.md](./arquitetura/MENSAGENS-LOGS.md)           | Sistema de mensagens e logs          |
| [Arquitetura/SEGURANCA.md](./arquitetura/SEGURANCA.md)                     | Práticas de segurança                |

## [CHAVE-INGLESA] Desenvolvimento

| Documento                                                                                    | Descrição                               |
| -------------------------------------------------------------------------------------------- | --------------------------------------- |
| [Desenvolvimento/PADROES.md](./desenvolvimento/PADROES.md)                                   | Padrões de código e convenções          |
| [Desenvolvimento/SISTEMA-PLUGINS.md](./desenvolvimento/SISTEMA-PLUGINS.md)                   | Como criar e registrar plugins          |
| [Desenvolvimento/analistas/CRIAR-ANALISTA.md](./desenvolvimento/analistas/CRIAR-ANALISTA.md) | Guia passo-a-passo para criar analistas |
| [Desenvolvimento/estrutura-codigo/README.md](./desenvolvimento/estrutura-codigo/README.md)   | Organização do diretório src/           |
| [Desenvolvimento/marketplace/README.md](./desenvolvimento/marketplace/README.md)             | Arquitetura e extensão do marketplace   |

## 📡 API REST

| Documento                                    | Descrição                                |
| -------------------------------------------- | ---------------------------------------- |
| [Api/REST-API.md](./api/REST-API.md)         | Documentação completa da API REST v1     |
| [Swagger UI](http://localhost:3100/api/docs) | Documentação interativa (servidor ativo) |

## [IDEIA] Exemplos e Casos de Uso

| Documento                                                                | Descrição                                |
| ------------------------------------------------------------------------ | ---------------------------------------- |
| [Exemplos/EXEMPLOS-USO.md](./exemplos/EXEMPLOS-USO.md)                   | Casos práticos de uso e CI/CD            |
| [Guias/DASHBOARD-GITHUB-PLUGINS.md](./guias/DASHBOARD-GITHUB-PLUGINS.md) | Sistema de plugins e GitHub Actions      |

## 🤖 GitHub App

| Documento                                                            | Descrição                                                   |
| -------------------------------------------------------------------- | ----------------------------------------------------------- |
| [Guias/GITHUB-APP.md](./guias/GITHUB-APP.md)                         | Guia completo do GitHub App (configuração, eventos, deploy) |

## 📚 Referência Técnica

| Documento                                                  | Descrição                                          |
| ---------------------------------------------------------- | -------------------------------------------------- |
| [Referencia/Comandos](./referencia/comandos/README.md)     | Referência completa de comandos                    |
| SDK Programático (`prometheus/sdk`)                        | API: `analyzeFile`, `analyzeProject` em `src/sdk/` |

## 🗺️ Planejamento e Roadmap

| Documento                                                  | Descrição                      |
| ---------------------------------------------------------- | ------------------------------ |
| [Roadmap/ROADMAP.md](./roadmap/ROADMAP.md)                 | Roadmap estratégico por versão |

## 📜 Histórico e Migração

| Documento                                                            | Descrição                         |
| -------------------------------------------------------------------- | --------------------------------- |
| [Historico/MIGRACAO-MENSAGENS.md](./historico/MIGRACAO-MENSAGENS.md) | Migração de sistemas de mensagens |

## 📎 Partials (Componentes de Documentação)

| Documento                                                          | Descrição                       |
| ------------------------------------------------------------------ | ------------------------------- |
| [Partials/AVISO-PROVENIENCIA.md](./partials/AVISO-PROVENIENCIA.md) | Aviso de proveniência e licença |

---

## [GRAFICO] Resumo da Versão Atual

| Item           | Valor      |
| -------------- | ---------- |
| **Versão**     | v0.11.0    |
| **Status**     | Estável    |
| **Node.js**    | ≥24.16.0   |
| **TypeScript** | 6.0+       |
| **Testes**     | Vitest 4.x |
| **Coverage**   | Meta: 90%+ |

---

> [DOCUMENTO] **Manutenção** Ao adicionar ou alterar documentos, atualize este índice.
> Consulte o [README principal do projeto](./README.md) para navegação geral.
