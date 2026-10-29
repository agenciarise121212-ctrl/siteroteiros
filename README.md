# roteiro.

Landing page com criador de roteiros de vídeo: escreva o tema, edite o texto e baixe em PDF ou imprima.

## Publicar no GitHub Pages

Este projeto usa React + Vite e precisa ser construído (`npm run build`) antes de ir para o ar.
Se o GitHub Pages publicar os arquivos-fonte direto do branch, a página fica toda branca.
O fluxo automático incluído em `.github/workflows/deploy.yml` faz a construção e a publicação para você.

1. No repositório, abra **Settings → Pages**.
2. Em **Build and deployment → Source**, escolha **GitHub Actions**.
3. Envie todos os arquivos do projeto, incluindo a pasta `.github` e a pasta `public` (não envie `node_modules` nem `dist`).
4. Abra a aba **Actions** e aguarde o fluxo **Publicar no GitHub Pages** terminar com o ícone verde (cerca de 1 a 2 minutos).
5. Acesse `https://SEU-USUARIO.github.io/NOME-DO-REPOSITORIO/`.

A cada novo envio para o branch `main`, o site é atualizado sozinho.
Para publicar de novo manualmente: **Actions → Publicar no GitHub Pages → Run workflow**.

### Se a pasta `.github` não foi enviada

No GitHub, clique em **Add file → Create new file**, digite o nome `.github/workflows/deploy.yml` e cole o conteúdo abaixo:

```yaml
# Gera a versão final do site (npm run build) e publica a pasta dist no GitHub Pages.
# Requisito: Settings > Pages > Build and deployment > Source = "GitHub Actions".
name: Publicar no GitHub Pages

on:
  push:
    branches: ['main', 'master']
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: 'pages'
  cancel-in-progress: true

jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@3d3c42e5aac5ba805825da76410c181273ba90b1 # v7
      - name: Set up Node
        uses: actions/setup-node@820762786026740c76f36085b0efc47a31fe5020 # v7
        with:
          node-version: 24
      - name: Install dependencies
        run: npm ci || npm install
      - name: Build
        run: npm run build
      - name: Setup Pages
        uses: actions/configure-pages@45bfe0192ca1faeb007ade9deae92b16b8254a0d # v6
      - name: Upload artifact
        uses: actions/upload-pages-artifact@fc324d3547104276b827a68afc52ff2a11cc49c9 # v5
        with:
          path: './dist'
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@368f82528645a54fb793d4d04e342629a3f51346 # v5
```

## Rodar no computador

```bash
npm install
npm run dev     # ambiente de desenvolvimento
npm run build   # gera a versão final na pasta dist
```
