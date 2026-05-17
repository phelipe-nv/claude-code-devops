## ADDED Requirements

### Requirement: Exibição de posts em lista feed horizontal
A tela inicial SHALL exibir os posts em lista vertical de itens, onde cada item é uma linha horizontal contendo data, título, resumo e botão de ação.

#### Scenario: Posts exibidos em lista no desktop
- **WHEN** a tela inicial é acessada em viewport >= 600px e há posts cadastrados
- **THEN** cada post SHALL ser renderizado como uma linha horizontal com data à esquerda, título e resumo ao centro e botão "Saiba Mais" à direita

#### Scenario: Posts exibidos em coluna no mobile
- **WHEN** a tela inicial é acessada em viewport < 600px e há posts cadastrados
- **THEN** cada post SHALL ser renderizado em coluna vertical com data, título, resumo e botão empilhados

#### Scenario: Resumo truncado quando longo
- **WHEN** o resumo de um post excede duas linhas de texto no layout horizontal
- **THEN** o texto SHALL ser truncado com reticências e não SHALL quebrar a altura do item de lista

#### Scenario: Botão de ação navega para o post
- **WHEN** o usuário clica no botão "Saiba Mais" de um item da lista
- **THEN** o sistema SHALL navegar para a página do post correspondente (`/post/:id`)

#### Scenario: Estado vazio preservado
- **WHEN** não há posts cadastrados
- **THEN** a tela SHALL exibir a mensagem e botão de criação, sem alteração em relação ao comportamento atual
