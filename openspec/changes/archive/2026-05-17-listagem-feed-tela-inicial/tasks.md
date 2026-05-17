## 1. CSS — Classes de lista feed

- [x] 1.1 Adicionar classe `.posts-list__feed` em `admin.css` com `display: flex; flex-direction: column; gap: 12px`
- [x] 1.2 Adicionar classe `.posts-list__feed__item` com layout `flex-direction: row; align-items: center; gap: 16px` para desktop (>= 600px)
- [x] 1.3 Definir estilo da data no item (`.feed__item__date`): cor `#69d6ff`, fonte 13px, `flex-shrink: 0`
- [x] 1.4 Definir estilo do bloco de texto (`.feed__item__body`): `flex: 1; overflow: hidden` com título e resumo
- [x] 1.5 Aplicar `overflow: hidden; display: -webkit-box; -webkit-line-clamp: 2` no resumo para truncar
- [x] 1.6 Definir estilo do botão (`.feed__item__button`): `flex-shrink: 0`, largura fixa, estilo visual igual ao atual
- [x] 1.7 Adicionar media query `@media (max-width: 599px)` com `.posts-list__feed__item { flex-direction: column; align-items: flex-start }`

## 2. HTML — Template EJS

- [x] 2.1 Em `src/views/index.ejs`, substituir `<div class="posts-list__grid">` por `<div class="posts-list__feed">`
- [x] 2.2 Substituir `<div class="posts-list__item">` e `<div class="post__card">` pelo novo markup de item de lista com as classes `feed__item__date`, `feed__item__body`, `feed__item__button`

## 3. Verificação

- [x] 3.1 Iniciar a aplicação e verificar layout de lista no desktop (>= 600px)
- [x] 3.2 Verificar layout empilhado no mobile (< 600px) via DevTools
- [x] 3.3 Verificar que o botão "Saiba Mais" navega corretamente para `/post/:id`
- [x] 3.4 Verificar que o estado vazio (sem posts) continua funcionando sem alterações
