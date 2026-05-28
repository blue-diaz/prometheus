<!-- @prometheus-disable pontuacao-repetida -->
---
Proveniência e Autoria: Documentação completa do Prometheus (licença MIT-0).
---

# [BIBLIOTECA] Documentação - Prometheus

**Bem-vindo** à documentação completa do Prometheus!

Prometheus é uma ferramenta CLI poderosa para análise, diagnóstico e manutenção de projetos JavaScript/TypeScript com suporte para múltiplas linguagens.

---

## [LANCAMENTO] Comece Aqui

**Procurando** algo específico?

| Na                             | Consulte                                                            |
| ------------------------------ | ------------------------------------------------------------------- |
| ⏱️ Começar rápido              | [Guia de Início Rápido](guias/GUIA-INICIO-RAPIDO.md)                |
| [LIVRO] Todos os comandos      | [Referência de Comandos](referencia/comandos/README.md)             |
| [CONFIG] Configurar            | [Guia de Configuração](guias/GUIA-CONFIGURACAO.md)                  |
| 👨‍[COMPUTADOR] Criar analista | [Como Criar Analistas](desenvolvimento/analistas/CRIAR-ANALISTA.md) |
| [COMPUTADOR] Extensão VSCode   | [Guia da Extensão VSCode](guias/GUIA-VSCODE.md)                     |
| 🏗️ Entender arquitetura       | [Árvore Arquitetural](arquitetura/ARVORE-ARQUITETURAL.md)           |
| [IDEIA] Exemplos práticos      | [Exemplos de Uso](exemplos/EXEMPLOS-USO.md)                         |
| 🔌 Sistema de plugins          | [Sistema de Plugins](desenvolvimento/SISTEMA-PLUGINS.md)            |
| [LOJA] Marketplace             | [Guia do Marketplace](guias/GUIA-MARKETPLACE.md)                    |
| 📋 Tudo (índice completo)      | [INDICE.md](INDICE.md)                                              |

---

## 📂 Estrutura de Documentação

```
docs/
├── INDICE.md                      ← Índice COMPLETO (comece aqui!)
├── README.md                      ← Este arquivo
│
├── guias/                         ← Para usuários
│   ├── GUIA-INICIO-RAPIDO.md
│   ├── GUIA-CONFIGURACAO.md
│   ├── GUIA-COMANDOS.md
│   ├── GUIA-MARKETPLACE.md
│   └── GUIA-VSCODE.md              ← Extensão VSCode
│
├── referencia/                    ← Referência técnica
│   └── comandos/
│       └── README.md              ← Todos os comandos
│
├── arquitetura/                   ← Design e internals
│   ├── README.md
│   ├── ARVORE-ARQUITETURAL.md    ← Estrutura completa
│   ├── TYPE-SAFETY.md
│   ├── SISTEMA-ERROS.md
│   ├── MENSAGENS-LOGS.md
│   └── SEGURANCA.md
│
├── desenvolvimento/               ← Para desenvolvedores
│   ├── PADROES.md                ← Padrões de código
│   ├── SISTEMA-PLUGINS.md        ← Sistema de plugins
│   ├── analistas/
│   │   └── CRIAR-ANALISTA.md     ← Criar novos analisadores
│   ├── marketplace/
│   │   └── README.md             ← Marketplace de analistas
│   └── estrutura-codigo/
│       └── README.md             ← Organização do src/
│
├── exemplos/
│   └── EXEMPLOS-USO.md           ← Casos de uso práticos
│
├── roadmap/
│   └── ROADMAP.md                ← Futura roadmap
│
└── ...
```

---

## [ALVO] Caminhos de Aprendizado

### Para Usuários 👤

```
1. Guia Início Rápido
        ↓
2. Guia Configuração
        ↓
3. Referência de Comandos
        ↓
4. Exemplos de Uso
```

### Para Desenvolvedores 👨‍[COMPUTADOR]

```
1. Árvore Arquitetural
        ↓
2. Estrutura de Código
        ↓
3. Como Criar Analistas
        ↓
4. Padrões de Desenvolvimento
        ↓
5. Sistema de Plugins
6. Marketplace de Analistas
```

### Por Tópico específico [BUSCA]

Consulte o [INDICE.md completo](INDICE.md)

---

## [BIBLIOTECA] Seções Principais

### [LANCAMENTO] Guias

- Instalação e setup
- Configuração de projeto
- Guia de todos os comandos
- Marketplace de analistas comunitários

### 🏗️ Arquitetura

- Estrutura de diretórios
- Fluxo de execução
- Design de camadas
- Type safety garantido
- Sistema de erros
- i18n (português, inglês, chinês, japonês)
- Segurança
- **Análise Inteligente** Diagnóstico profundo de segurança, performance e arquitetura.
- **Ecossistema GitHub:** Analista avançado de Actions com suporte a plugins e análise estruturada (v0.7.0).
- **Extensível** Sistema de plugins robusto para adicionar novas detecções sob medida.

### 👨‍[COMPUTADOR] Desenvolvimento

- Como criar novos analistas/detectores
- Estrutura detalhada do código
- Padrões de desenvolvimento (nomenclatura, imports, tipos)
- Sistema de plugins e registry
- Marketplace de analistas (arquitetura e extensão)

### [LIVRO] Referência

- Documentação de TODOS os comandos
- Flags e opções
- Exemplos de uso
- Troubleshooting

### [IDEIA] Exemplos

- Análise básica
- Auto-fix
- Guardian (monitoramento)
- CI/CD integration
- Criar analista customizado

### 🗺️ Roadmap

- v0.6 - v0.8 (Marketplace implementado)
- Visão de longo termo

---

## [BRILHO] Destaques Recentes

**Documentação** atualizada em 2026-05-24:

- [ESTRELA] [GUIA-VSCODE.md](guias/GUIA-VSCODE.md) - Extensão VSCode com análise em tempo real
- [ESTRELA] `src/sdk/` — SDK programático (`analyzeFile`, `analyzeProject`) exposto via `prometheus/sdk`
- [ESTRELA] [GUIA-MARKETPLACE.md](guias/GUIA-MARKETPLACE.md) - Marketplace de analistas comunitários
- [ESTRELA] [marketplace/README.md](desenvolvimento/marketplace/README.md) - Arquitetura e extensão do marketplace

**Documentação** atualizada em 2026-02-15:

- [ESTRELA] [INDICE.md](INDICE.md) - Índice completo com navegação por tópico
- [ESTRELA] [ARVORE-ARQUITETURAL.md](arquitetura/ARVORE-ARQUITETURAL.md) - Mapa completo da estrutura
- [ESTRELA] [CRIAR-ANALISTA.md](desenvolvimento/analistas/CRIAR-ANALISTA.md) - Guia prático com exemplos
- [ESTRELA] [PADROES.md](desenvolvimento/PADROES.md) - Convenções e boas práticas
- [ESTRELA] [SISTEMA-PLUGINS.md](desenvolvimento/SISTEMA-PLUGINS.md) - Como estender com plugins
- [ESTRELA] [estrutura-codigo/](desenvolvimento/estrutura-codigo/README.md) - src/ mapeado e explicado
- [ESTRELA] [EXEMPLOS-USO.md](exemplos/EXEMPLOS-USO.md) - Casos práticos e CI/CD
- [ESTRELA] [referencia/comandos/](referencia/comandos/README.md) - Cada comando documentado

---

## [COMENTARIO] Procurando algo?

| Pergunta                | Resposta                                                         |
| ----------------------- | ---------------------------------------------------------------- |
| "Como instalo? "        | [Guia de Início Rápido](guias/GUIA-INICIO-RAPIDO.md)             |
| "Como configuro? "      | [GUIA-CONFIGURACAO.md](guias/GUIA-CONFIGURACAO.md)               |
| "Qual comando usar? "   | [Referência de Comandos](referencia/comandos/README.md)          |
| "Como criar analista? " | [CRIAR-ANALISTA.md](desenvolvimento/analistas/CRIAR-ANALISTA.md) |
| "Como funciona? "       | [ARVORE-ARQUITETURAL.md](arquitetura/ARVORE-ARQUITETURAL.md)     |
| "Exemplo prático"       | [EXEMPLOS-USO.md](exemplos/EXEMPLOS-USO.md)                      |
| "Qual padrão usar? "    | [PADROES.md](desenvolvimento/PADROES.md)                         |
| "Como estender? "       | [SISTEMA-PLUGINS.md](desenvolvimento/SISTEMA-PLUGINS.md)         |
| "Marketplace? "         | [GUIA-MARKETPLACE.md](guias/GUIA-MARKETPLACE.md)                 |
| "Extensão VSCode? "     | [GUIA-VSCODE.md](guias/GUIA-VSCODE.md)                           |
| "SDK programático? "    | `src/sdk/` — `analyzeFile`, `analyzeProject` em `prometheus/sdk` |
| "Organização do src/"   | [estrutura-codigo/](desenvolvimento/estrutura-codigo/README.md)  |
| "Tudo! "                | [INDICE.md](INDICE.md)                                           |

---

## 📋 Checklist de Documentação

Documentação completa e atualizada [OK]

- [OK] Instalação e início rápido
- [OK] Configuração e customização
- [OK] Referência de TODOS os comandos
- [OK] Arquitetura e design
- [OK] Estrutura completa do código
- [OK] Como criar analistas
- [OK] Padrões de desenvolvimento
- [OK] Sistema de plugins/extensão
- [OK] Exemplos práticos
- [OK] Integração CI/CD
- [OK] Marketplace de analistas
- [OK] Type safety
- [OK] Sistema de erros
- [OK] i18n (4 idiomas)
- [OK] Segurança
- [OK] Extensão VSCode
- [OK] SDK Programático (`prometheus/sdk`)

---

## 🔗 Links Importantes

- **Repositório:** PROTECTED_2 [GitHub](https://github.com/blue-diaz/prometheus)
- **Issues:** PROTECTED_2 [GitHub Issues](https://github.com/blue-diaz/prometheus/issues)
- **Releases:** PROTECTED_2 [Releases](https://github.com/blue-diaz/prometheus/releases)
- **Licença** MIT-0

---

## [DOCUMENTO] Manutenção da Documentação

Se você atualizar a documentação:

1. Atualize este README se mudar estrutura
2. Atualize [INDICE.md](INDICE.md) com novos arquivos
3. Inclua um resumo clara do que mudou
4. Teste os links em um PR
5. Documente patterns com exemplos reais

---

## [GRADUACAO] Próximos Passos

[APONTAR-DIREITA] **Novo** com Prometheus? Comece com [Guia de Início Rápido](guias/GUIA-INICIO-RAPIDO.md)

[APONTAR-DIREITA] **Já** conhece? Veja [INDICE.md](INDICE.md) completo com navegação por tópico

[APONTAR-DIREITA] **Developer**? Vá direto para [Como Criar Analistas](desenvolvimento/analistas/CRIAR-ANALISTA.md)

[APONTAR-DIREITA] **Marketplace**? Veja o [Guia do Marketplace](guias/GUIA-MARKETPLACE.md) ou a [Arquitetura](desenvolvimento/marketplace/README.md)

---

**Versão:** v0.8.9+
**Última atualização** 2026-05-24
**Status** [OK] Documentação completa
