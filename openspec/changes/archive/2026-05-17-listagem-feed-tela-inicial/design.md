## Context

A tela inicial (`src/views/index.ejs`) renderiza posts via template EJS usando a classe `.posts-list__grid` (CSS Grid, 3 colunas no desktop). O estilo dos cards está em `src/static/styles/admin.css`. A mudança é puramente de apresentação — sem alteração de dados, rotas ou backend.

## Goals / Non-Goals

**Goals:**
- Substituir o layout de grade por lista feed horizontal compacta
- Garantir responsividade: linha horizontal no desktop, coluna empilhada no mobile (< 600px)
- Manter o mesmo conjunto de informações exibidas (data, título, resumo, botão)

**Non-Goals:**
- Alterar rotas, modelos ou lógica de backend
- Remover as classes CSS de grade do arquivo (evitar regressão desnecessária)
- Paginação ou lazy loading

## Decisions

**Novas classes em vez de reusar `.posts-list__grid`**
Criar `.posts-list__feed` e `.posts-list__feed__item` evita conflito com o layout de grade existente e mantém a mudança isolada. Custo: duas classes novas no CSS.

**Breakpoint em 600px**
Alinhado com o breakpoint `540px` já usado no projeto para o header. Usa `min-width: 600px` para o layout horizontal — abaixo disso empilha verticalmente.

**Resumo truncado com `line-clamp`**
No layout horizontal o resumo recebe `overflow: hidden` e `-webkit-line-clamp: 2` para não quebrar a linha do item. Alternativa (esconder o resumo no desktop) foi descartada — mantém informação visível.

**Botão à direita com `flex-shrink: 0`**
O botão "Saiba Mais" fica fixo à direita com largura mínima definida, sem encolher quando o título/resumo forem longos.

## Risks / Trade-offs

- **Resumo longo pode quebrar o alinhamento visual** → Mitigação: `line-clamp: 2` + altura máxima no item
- **Classes antigas ficam no CSS sem uso** → Risco baixo; são classes internas sem API pública
