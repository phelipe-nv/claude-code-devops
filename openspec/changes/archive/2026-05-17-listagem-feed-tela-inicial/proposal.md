## Why

A tela inicial exibe posts em grade de 3 colunas, o que limita a densidade de informação visível por tela e não favorece leitura sequencial de notícias. A mudança para lista horizontal compacta (feed) permite visualizar mais posts simultaneamente e aproxima a UX de portais de notícias.

## What Changes

- Substituir o layout em grade (`posts-list__grid`) por layout em lista feed (`posts-list__feed`) na tela inicial
- Cada item de lista exibe em linha horizontal: data | título | resumo | botão "Saiba Mais"
- No mobile (< 600px) o item empilha verticalmente: data, título, resumo e botão em coluna
- Novas classes CSS para o item de lista; classes de grade existentes permanecem no arquivo mas saem do uso

## Capabilities

### New Capabilities

- `feed-listagem`: Layout de lista horizontal compacta para exibição dos posts na tela inicial, com responsividade mobile

### Modified Capabilities

## Impact

- `src/views/index.ejs` — substituição do bloco de grade pelo novo markup de lista
- `src/static/styles/admin.css` — adição das classes `.posts-list__feed` e `.posts-list__feed__item`
