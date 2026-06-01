---
Proveniência e Autoria: Documentação completa do Prometheus (licença MIT-0).
---

# 🔌 Sistema de Plugins e Analista GitHub Actions (v0.7.0)

Este documento detalha as funcionalidades de extensibilidade e análise avançada introduzidas no Prometheus para o ecossistema de GitHub Actions.

---

## 🔌 Sistema de Plugins (GitHub Actions)

A partir da v0.5.0, o analista de GitHub Actions tornou-se extensível, permitindo que você adicione suas próprias regras de detecção.

### Criando um Plugin

Para inicializar um novo plugin:
```bash
prometheus plugins init meu-detector-seguranca
```

### Exemplo de Implementação

```typescript
import { registrarDetectorGithubActions } from '@analistas/plugins/analista-github-actions.js';

registrarDetectorGithubActions({
  nome: 'no-sudo-allowed',
  descricao: 'Detecta uso proibido de sudo em runners',
  severidade: 'alta',
  testar: (workflow, context) => {
    const problemas = [];
    // O parâmetro 'workflow' é o YAML parseado como objeto JS
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

---

## [BUSCA] Analista Avançado (Engine v0.7.0)

O motor de análise agora combina **Análise Estruturada (YAML AST)** com **Análise Sintática** permitindo detecções muito mais precisas:

- **Segurança** Script Injection, Hardcoded Secrets, Permissões excessivas, Pinning por SHA.
- **Performance** Falta de Caching, Jobs redundantes, Matrix strategy sem fail-fast.
- **Governança** Falta de CODEOWNERS, README sem Código de Conduta, Ausência de automação de stale/release.

---

## [FERRAMENTAS] Tecnologias Utilizadas

- **Parsing** Biblioteca `yaml` para suporte a AST completo.
- **Plugins** Sistema extensível com registro de detectores customizados.

---

## [BIBLIOTECA] Documentação Completa

Para documentação detalhada do GitHub App incluindo:
- Configuração passo-a-passo
- Todos os eventos webhook suportados
- Comandos CLI completos
- Deploy em produção
- Solução de problemas

Consulte o [Guia Completo do GitHub App](./GITHUB-APP.md).
