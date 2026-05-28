---
Proveniência e Autoria: Este documento integra o projeto Prometheus (licença MIT-0).
---

# Guia de Compliance

Relatórios de conformidade com frameworks de segurança mapeiam automaticamente os achados do Prometheus para controles de normas como ISO 27001 e SOC 2.

## Frameworks Suportados

| Framework        | Versão             | Controles | Categorias                              |
| ---------------- | ------------------ | --------- | --------------------------------------- |
| `iso-27001`      | ISO/IEC 27001:2022 | 15        | Políticas, Acesso, Criptografia ...     |
| `soc2`           | SOC 2 (AICPA 2023) | 12        | Segurança, Disponibilidade, Privacidade |

## Como Funciona

1. O Prometheus analisa o código-fonte do projeto
2. Cada ocorrência encontrada (vulnerabilidade, tipo inseguro, senha hardcoded, etc.) é mapeada para controles dos frameworks
3. O relatório de compliance é gerado com:
   - **Pontuação geral** de conformidade (0-100%)
   - **Status por controle**: conforme, parcial, não-conforme
   - **Riscos identificados** com severidade (crítico, alto, médio, baixo)
   - **Recomendações** acionáveis

## Uso

```bash
# Listar frameworks disponíveis
prometheus compliance list

# Relatório ISO 27001 em Markdown
prometheus compliance report iso-27001

# Relatório SOC 2 em JSON
prometheus compliance report soc2 --json

# Salvar relatório em arquivo
prometheus compliance report iso-27001 -o relatorio-compliance.md
prometheus compliance report soc2 --json -o relatorio-soc2.json
```

## Exemplo de Saída (Markdown)

```
# Relatório de Compliance: ISO 27001

**Projeto**: meu-projeto
**Framework**: ISO/IEC 27001:2022
**Pontuação Geral**: 73%

## Resumo
| Métrica               | Valor |
|-----------------------|-------|
| Pontuação Geral       | 73%   |
| Conformes             | 11    |
| Parciais              | 3     |
| Não Conformes         | 1     |
| Riscos Críticos       | 1     |
| Riscos Altos          | 2     |

## Recomendações
- Corrigir 1 risco(s) crítico(s) de segurança imediatamente
- 1 controle(s) não conforme(s) precisam de ação corretiva
```

## Mapeamento ISO 27001

| Controle              | Descrição                          | Padrões Correspondentes                |
| --------------------- | ---------------------------------- | -------------------------------------- |
| A.9.4.2               | Autenticação Segura                | `senha-fraca`, `weak-crypto`           |
| A.10.1.1              | Criptografia                       | `MD5`, `SHA1`, `encryption`            |
| A.12.1.2              | Proteção contra Malware            | `XSS`, `SQL_INJECTION`                 |
| A.12.6.1              | Gestão de Vulnerabilidades         | `dependencia-vulneravel`               |
| A.14.2.1              | Desenvolvimento Seguro             | `tipo-inseguro`, `unhandled-async`     |
| A.18.1.4              | Privacidade e Proteção de Dados    | `LGPD`, `GDPR`, `dados-sensiveis`      |

## Mapeamento SOC 2

| Princípio             | Categoria         | Padrões Correspondentes                |
| --------------------- | ----------------- | -------------------------------------- |
| CC1.1                 | Segurança         | `controle-acesso`, `CORS`              |
| CC2.1                 | Segurança         | `SQL_INJECTION`, `COMMAND_INJECTION`   |
| CC4.1                 | Integridade       | `hash`, `checksum`, `criptografia`     |
| CC5.1                 | Confidencialidade | `weak-crypto`, `dados-sensiveis`       |
| CC5.2                 | Confidencialidade | `PASSWORD_HARDCODED`, `TOKEN_EXPOSTO`  |
| CC6.1                 | Privacidade       | `LGPD`, `GDPR`, `PII`                  |
