---
Proveniência e Autoria: Este documento integra o projeto Prometheus (licença MIT-0).
---

# Guia de Publicação — VSCode Marketplace

Passo a passo para publicar a extensão **Prometheus IntelliSense** no marketplace oficial do VSCode.

---

## Pré-requisitos

- Node.js ≥ 24.16.0
- Conta no [Azure DevOps](https://dev.azure.com) (gerencia o publisher do VSCode)
- Extensão `vsce` (`npm install -g @vscode/vsce`)
- A extensão deve estar compilando sem erros:
  ```bash
  cd extensions/vscode
  npm run compile
  ```

---

## 1. Criar um Publisher no Marketplace

Acesse [https://marketplace.visualstudio.com/manage](https://marketplace.visualstudio.com/manage) e faça login com sua conta Microsoft.

1. Clique em **New Publisher**
2. Defina:
   - **Publisher name**: `prometheus` (ou outro nome — se `prometheus` já existir, crie um novo)
   - **Display name**: `Prometheus`
   - **Description**: `Prometheus Code Analysis`
   - **E-mail**: seu e-mail
3. Aceite os termos e crie

> O `publisher` no `package.json` deve ser **exatamente** o nome do publisher que você criou. Atualmente está como `"prometheus"`.

---

## 2. Gerar um Token de Acesso Pessoal (PAT)

1. Vá para [https://dev.azure.com](https://dev.azure.com)
2. Clique em **User settings** (engrenagem no canto superior direito) → **Personal access tokens**
3. Clique em **New Token**
4. Configure:
   - **Name**: `vscode-marketplace-publish`
   - **Organization**: `All accessible organizations`
   - **Expiration**: escolha um prazo (recomendado: 1 ano)
   - **Scopes**: selecione **Custom defined** → marque **Marketplace > Manage**
5. Clique em **Create** e **copie o token** (não será mostrado novamente)

---

## 3. Atualizar o package.json

Verifique e atualize os campos obrigatórios em `extensions/vscode/package.json`:

```json
{
  "name": "prometheus-vscode",
  "displayName": "Prometheus IntelliSense",
  "version": "0.10.0",
  "publisher": "prometheus",
  "description": "Inteligência de código completa com Prometheus...",
  "license": "MIT-0",
  "repository": {
    "type": "git",
    "url": "https://github.com/blue-diaz/prometheus"
  },
  "icon": "icon.png",
  "galleryBanner": {
    "color": "#1a1a2e",
    "theme": "dark"
  },
  "engines": {
    "vscode": "^1.85.0"
  },
  "categories": [
    "Linters",
    "Programming Languages",
    "Formatters",
    "Visualization"
  ],
  "keywords": [ "prometheus", "intellisense", "code-analysis", "linter", ... ]
}
```

Campos obrigatórios que o marketplace valida:
- `name`, `displayName`, `version`, `publisher`, `description`, `engines.vscode`, `categories`
- `icon` — PNG de 128x128px ou 256x256px no diretório da extensão
- `license` — arquivo `LICENSE` no diretório da extensão ou campo no package.json
- `repository` — URL válida do repositório
- `bugs` — URL de issues

---

## 4. (Opcional) Licença

Copie a licença do projeto raiz para o diretório da extensão:

```bash
cp ../../LICENSE ./LICENSE
```

---

## 5. (Opcional) Ícone

Crie ou copie um ícone PNG 128x128:

```bash
# gere um ícone simples, por exemplo com ImageMagick:
convert -size 128x128 xc:'#1a1a2e' -font Helvetica -pointsize 48 \
  -fill white -gravity center -annotate 0 'P' icon.png
```

Ou crie manualmente e salve como `extensions/vscode/icon.png`.

---

## 6. Verificar se a Extensão Compila

```bash
cd extensions/vscode
npm run compile
```

Verifique se `out/extension.js` e todos os providers foram gerados.

---

## 7. Empacotar com vsce

```bash
npx @vscode/vsce package
```

Isso gera o arquivo `prometheus-vscode-0.10.0.vsix`.

Se houver erros, corrija e tente novamente:

- **Missing icon**: crie `icon.png`
- **Missing repository**: verifique o campo `repository` no package.json
- **Missing license**: copie o LICENSE ou adicione campo `"license": "MIT-0"`
- **ESLint warnings**: podem ser ignorados com `--noVerify` ou `--allowMissingRepository`

Para pular verificações (não recomendado para primeira publicação):

```bash
npx @vscode/vsce package --noVerify
```

---

## 8. Publicar no Marketplace

### 8.1. Via vsce (automático)

```bash
npx @vscode/vsce publish -p <TOKEN>
```

Substitua `<TOKEN>` pelo PAT gerado no passo 2.

Isso incrementa o patch da versão (0.10.0 → 0.10.1), empacota e publica.

### 8.2. Publicar versão específica

```bash
npx @vscode/vsce publish -p <TOKEN> 0.10.0
```

### 8.3. Publicar via VSIX (manual)

```bash
npx @vscode/vsce package          # gera .vsix
npx @vscode/vsce publish -p <TOKEN> --packagePath prometheus-vscode-0.10.0.vsix
```

### 8.4. Fazer upload manual pelo navegador

1. Acesse [https://marketplace.visualstudio.com/manage](https://marketplace.visualstudio.com/manage)
2. Selecione seu publisher
3. Clique em **New Extension** → **Visual Studio Code**
4. Faça upload do arquivo `.vsix` gerado

---

## 9. Verificar a Publicação

Após o upload, a extensão fica disponível em:

```
https://marketplace.visualstudio.com/items?itemName=prometheus.prometheus-vscode
```

A URL segue o padrão:
```
https://marketplace.visualstudio.com/items?itemName=<PUBLISHER>.<NAME>
```

Pode levar alguns minutos para aparecer nas buscas.

---

## 10. Atualizações Futuras

Para publicar uma nova versão:

```bash
cd extensions/vscode

# 1. Atualize manualmente a versão no package.json
#    ou use: npx vsce publish patch  (incrementa patch automaticamente)

# 2. Compile
npm run compile

# 3. Publique
npx @vscode/vsce publish -p <TOKEN>
```

Estratégias de versionamento:
- `patch` (0.10.0 → 0.10.1): bug fixes
- `minor` (0.10.0 → 0.11.0): novas funcionalidades
- `major` (1.0.0 → 2.0.0): breaking changes

```bash
npx @vscode/vsce publish patch -p <TOKEN>
npx @vscode/vsce publish minor -p <TOKEN>
npx @vscode/vsce publish major -p <TOKEN>
```

---

## 11. CI/CD com GitHub Actions (recomendado)

Crie `.github/workflows/publish-vscode.yml` no raiz do projeto:

```yaml
name: Publicar VSCode Extension

on:
  push:
    tags:
      - 'vscode-v*'

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: '24'

      - run: npm ci
        working-directory: extensions/vscode

      - run: npm run compile
        working-directory: extensions/vscode

      - name: Publish
        run: npx @vscode/vsce publish -p ${{ secrets.VSCE_PAT }}
        working-directory: extensions/vscode
```

> Configure o segredo `VSCE_PAT` no GitHub com o token gerado no passo 2.

---

## Checklist final antes de publicar

- [ ] `npm run compile` passa sem erros
- [ ] `package.json` tem publisher correto
- [ ] `package.json` tem versão correta
- [ ] `package.json` tem campos obrigatórios (name, displayName, description, engines, categories)
- [ ] `icon.png` existe no diretório da extensão
- [ ] `LICENSE` existe no diretório da extensão
- [ ] `README.md` atualizado com descrição e funcionalidades
- [ ] `.vscodeignore` lista arquivos que **não** devem ir no pacote (src/, node_modules/, tsconfig.json)
- [ ] Token PAT válido e com escopo `Marketplace > Manage`
- [ ] Publisher criado no marketplace

---

## Troubleshooting

| Erro | Solução |
|---|---|
| `Missing publisher name` | Crie o publisher no marketplace e atualize `package.json` |
| `This publisher is already in use` | Escolha outro nome de publisher |
| `The Personal Access Token verification failed` | Gere um novo PAT com escopo correto |
| `Extension not found after publish` | Aguarde 5-10 min para propagação |
| `Version X.X.X already exists` | Incremente a versão no `package.json` |
| `vsce: command not found` | Instale com `npm install -g @vscode/vsce` |

---

## Links Úteis

- [VSCode Extension API](https://code.visualstudio.com/api)
- [Publishing Extensions](https://code.visualstudio.com/api/working-with-extensions/publishing-extension)
- [Marketplace Portal](https://marketplace.visualstudio.com/manage)
- [vsce docs](https://github.com/microsoft/vscode-vsce)

