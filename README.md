# Controle financeiro — macOS

Aplicativo de controle financeiro da casa para **macOS**: **Electron** com os dados num banco
**SQLite local** (`node:sqlite`, embutido no Electron). Funciona offline; nada sai do computador.

O app nasce sem nenhum valor e com a estrutura base:

- **Mês a mês** — Entradas: Salário pessoa 1, 13º pessoa 1, Férias pessoa 1, Outros · Saídas: Cartão de crédito, Financiamento, Luz, Internet, Plano de saúde
- **Reservas e patrimônio** — Reservas e investimentos: Poupança banco 1 · Bitcoin oculto (volta pelo lápis *Editar módulos*) · FGTS: Pessoa 1
- **Metas a comprar** — vazio
- **Categorias** e **Normalização de nomes** — conjunto inicial (`desktop/esquema.json`)

## Estrutura

```
index.html            esqueleto: só <?!= include('...') ?> na ordem certa (resolvidos no build)
frontend/             css, partials e js (scripts clássicos, escopo global; a ordem dos includes importa)
  js/data/edicao.html rótulos, categorias padrão e textos desta edição
  js/data/seed.html   ano inicial: nenhum valor, só a estrutura base
desktop/
  esquema.json        tabelas do banco, categorias e regras de normalização iniciais
  db.js               getData / applyChanges / saveNormalizacoes sobre SQLite
  main.js, preload.js janela, menu (exportar/importar banco) e a ponte google.script.run -> processo principal
  build.js            gera desktop/app/ (HTML único, xlsx, pdf.js e fontes locais)
tests/                node --test: banco (DDB-*) e app no jsdom (DSK-*)
```

## Uso

```
npm install                       # se não baixar o Electron: node node_modules/electron/install.js
npm test
npm run desktop:dev               # abre o app
npm run desktop:dist:mac  # instalador em dist/
```

Pelo GitHub: *Actions > Instalador > Run workflow* (ou crie uma tag `v*`); o instalador fica em *Artifacts*.

Os dados ficam em `~/Library/Application Support/Controle financeiro/financeiro.db`. O menu *Arquivo* exporta e importa o banco (backup / outro computador).

O app não é assinado nem notarizado pela Apple: na primeira vez, clique com o botão direito no app > *Abrir* (ou em *Ajustes do Sistema > Privacidade e Segurança > Abrir mesmo assim*). O .dmg é universal (Intel e Apple Silicon) e só pode ser gerado num Mac — use o workflow do GitHub.
