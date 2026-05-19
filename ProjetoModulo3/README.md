# 🛒 Mercadinho do Irmão — README

> Sistema de ponto de venda (PDV) e gestão de estoque para pequeno varejo,
> construído integralmente em [Bubble.io](https://bubble.io) e com ajuda do GEMINI.
https://mercado-71927.bubbleapps.io/version-test?debug_mode=true
---

## 📋 Índice

- [Visão Geral](#visão-geral)
- [Funcionalidades](#funcionalidades)
- [Arquitetura do App](#arquitetura-do-app)
- [Modelo de Dados](#modelo-de-dados)
- [Páginas e Navegação](#páginas-e-navegação)
- [Fluxos de Trabalho (Workflows)](#fluxos-de-trabalho-workflows)
- [Estado Global (Custom States)](#estado-global-custom-states)
- [Problemas Conhecidos](#problemas-conhecidos)
- [Roadmap / Próximos Passos](#roadmap--próximos-passos)

---

## Visão Geral

**Mercadinho do Irmão** é um sistema web de PDV (Ponto de Venda) voltado para
pequenos mercados e mercearias. Permite ao caixa registrar vendas por leitura
de código de barras, gerenciar o catálogo de produtos e visualizar o ranking de
produtos mais vendidos — tudo em uma única interface consolidada.

O sistema conta ainda com uma **tela de cliente** (customer display) que exibe
em tempo real os itens adicionados ao carrinho e o total a pagar.

---

## Funcionalidades

### PDV (Ponto de Venda)
- Leitura de código de barras via scanner físico ou digitação manual
- Adição automática de produtos ao carrinho ao pressionar Enter
- Exibição do último item escaneado com nome e preço
- Histórico de escaneamentos recentes com re-adição rápida
- Ajuste de quantidade antes de confirmar o item
- Limpeza do carrinho com um clique
- Remoção individual de itens do carrinho
- Seleção de método de pagamento: **Pix**, **Cartão** ou **Dinheiro**
- Confirmação da venda cria um registro `Sale` e seus `SaleItem`s

### Estoque (Catálogo de Produtos)
- Listagem de todos os produtos com nome, código de barras, preço, estoque e nível de alerta
- Filtros rápidos: **Todos**, **Estoque Baixo**, **Sem Estoque**
- Busca por nome ou código de barras
- Cadastro de novo produto (nome, barcode, preço, estoque, alerta)
- Edição de produto existente
- Exclusão de produto com confirmação

### Ranking de Vendas
- Visualização dos produtos mais vendidos por período: **Dia**, **Semana**, **Mês**, **Ano**, **Todo o Período**
- Cards de destaque para 1º, 2º e 3º lugar
- Grade completa com posição, nome, barcode, unidades vendidas e receita gerada
- Métricas de cabeçalho: total de unidades vendidas e SKUs ativos

### Tela de Cliente
- Display dedicado para o cliente acompanhar o pedido em tempo real
- Exibe o último item escaneado, quantidade, preço unitário, subtotal e total
- Auto-refresh contínuo
- Aceita Pix, Cartão e Dinheiro
- Bilíngue: Português / Inglês

---

## Arquitetura do App



> Sistema de ponto de venda (PDV) e gestão de estoque para pequeno varejo,
> construído integralmente em [Bubble.io](https://bubble.io).

---

---


> Sistema de ponto de venda (PDV) e gestão de estoque para pequeno varejo,
> construído integralmente em [Bubble.io](https://bubble.io).

---

## 📋 Índice

- [Visão Geral](#visão-geral)
- [Funcionalidades](#funcionalidades)
- [Arquitetura do App](#arquitetura-do-app)
- [Modelo de Dados](#modelo-de-dados)
- [Páginas e Navegação](#páginas-e-navegação)
- [Fluxos de Trabalho (Workflows)](#fluxos-de-trabalho-workflows)
- [Estado Global (Custom States)](#estado-global-custom-states)
- [Problemas Conhecidos](#problemas-conhecidos)
- [Roadmap / Próximos Passos](#roadmap--próximos-passos)

---

## Visão Geral

**Mercadinho do Irmão** é um sistema web de PDV (Ponto de Venda) voltado para
pequenos mercados e mercearias. Permite ao caixa registrar vendas por leitura
de código de barras, gerenciar o catálogo de produtos e visualizar o ranking de
produtos mais vendidos — tudo em uma única interface consolidada.

O sistema conta ainda com uma **tela de cliente** (customer display) que exibe
em tempo real os itens adicionados ao carrinho e o total a pagar.

---

## Funcionalidades

### PDV (Ponto de Venda)
- Leitura de código de barras via scanner físico ou digitação manual
- Adição automática de produtos ao carrinho ao pressionar Enter
- Exibição do último item escaneado com nome e preço
- Histó
Bubble.io Web App │ ├── Sidebar Reusável (navegação global) │ ├── Link → PDV (?tab=pdv) │ ├── Link → Estoque (?tab=estoque) │ └── Link → Ranking (?tab=ranking) │ ├── índice (página principal consolidada) │ ├── Grupo pos-main [visível quando current_tab = "pdv"] │ ├── Group catalog-main [visível quando current_tab = "estoque"] │ ├── Group ranking-main [visível quando current_tab = "ranking"] │ ├── Popup add-product-popup │ ├── Popup edit-product-popup │ └── Popup delete-product-popup │ ├── cliente (tela de cliente — display secundário) ├── classificação (página legada — pode ser removida) ├── catalog (página legada — pode ser removida) ├── reset_pw (redefinição de senha) └── 404 (página não encontrada)

### Navegação por Abas
A página `index` utiliza um **Custom State** `current_tab` (tipo: text,
padrão: `"pdv"`) para controlar qual seção está visível. A navegação ocorre via
parâmetro de URL `?tab=`, lido no evento **Page is loaded**, que define o estado
adequado. Cada grupo de conteúdo possui uma condicional de visibilidade e
**Collapse when hidden** ativado.

---

## Modelo de Dados

### `User`
| Campo   | Tipo | Descrição                  |
|---------|------|----------------------------|
| `name`  | text | Nome do operador/caixa     |
| `theme` | text | Preferência de tema visual |
| `email` | text | E-mail de acesso           |

**Privacidade:** Cada usuário só pode ver e editar os próprios dados.

---

### `Product`
| Campo         | Tipo   | Descrição                        |
|---------------|--------|----------------------------------|
| `name`        | text   | Nome do produto                  |
| `barcode`     | text   | Código de barras (EAN/UPC)       |
| `price`       | number | Preço unitário (R$)              |
| `stock`       | number | Quantidade em estoque            |
| `alert_level` | number | Nível mínimo para alerta de baixo estoque |

---

### `Sale`
| Campo            | Tipo   | Descrição                         |
|------------------|--------|-----------------------------------|
| `status`         | text   | Status da venda (`open`, `closed`)|
| `cashier`        | User   | Operador que realizou a venda     |
| `total`          | number | Valor total da venda (R$)         |
| `closed_at`      | date   | Data/hora de fechamento           |
| `payment_method` | text   | `Pix`, `Card` ou `Cash`           |

---

### `SaleItem`
| Campo        | Tipo    | Descrição                         |
|--------------|---------|-----------------------------------|
| `sale`       | Sale    | Venda à qual o item pertence      |
| `product`    | Product | Produto vendido                   |
| `quantity`   | number  | Quantidade vendida                |
| `unit_price` | number  | Preço unitário no momento da venda|
| `subtotal`   | number  | `quantity × unit_price`           |

---

## Páginas e Navegação

| Página       | Slug        | Descrição                                              |
|--------------|-------------|--------------------------------------------------------|
| **Index**    | `/`         | App principal: PDV, Estoque e Ranking em abas          |
| **Customer** | `/customer` | Tela do cliente — display em tempo real                |
| **Catalog**  | `/catalog`  | ⚠️ Legada — conteúdo migrado para Index               |
| **Ranking**  | `/ranking`  | ⚠️ Legada — conteúdo migrado para Index               |
| **Reset PW** | `/reset_pw` | Redefinição de senha via e-mail                        |
| **404**      | `/404`      | Página de erro com botão "Go Home"                     |

---

## Fluxos de Trabalho (Workflows)

### PDV — `index`
| Trigger                        | Ações                                              |
|--------------------------------|----------------------------------------------------|
| Clique em **Scan / Add Item**  | ShowElement (barcode popup), DisplayGroupData      |
| Clique em **Clear**            | ResetInputs (limpa carrinho)                       |
| Clique em **Delete item**      | DeleteThing (remove SaleItem)                      |
| Clique em **Confirm Sale**     | ChangeThing (atualiza Sale), NewThing (cria Sale)  |
| Clique em **Add to Cart**      | NewThing (cria SaleItem), HideElement              |
| Clique em **Cancel / Close**   | HideElement                                        |

### Estoque — `index` (migrado do catalog)
| Trigger                        | Ações                                                    |
|--------------------------------|----------------------------------------------------------|
| Clique em **Add Product**      | ShowElement (popup add)                                  |
| Filtro **All / Low / Out**     | DisplayListData (filtra repeating group)                 |
| Clique em **Edit** (linha)     | DisplayGroupData + ShowElement (popup edit)              |
| Clique em **Delete** (linha)   | DisplayGroupData + ShowElement (popup delete)            |
| Clique em **Save Product**     | NewThing (Product), HideElement, ResetInputs             |
| Clique em **Save Changes**     | ChangeThing (Product), HideElement                       |
| Clique em **Delete Product**   | DeleteThing, HideElement                                 |

### Ranking — `index` (migrado do ranking)
| Trigger                        | Ações                                   |
|--------------------------------|-----------------------------------------|
| Filtro **Day/Week/Month/Year/All** | DisplayListData (filtra ranking grid)|

### Navegação global
| Trigger                   | Ações                                              |
|---------------------------|----------------------------------------------------|
| **Page is loaded**        | Lê `?tab=` da URL, define `current_tab`            |
| Sidebar → PDV             | Navega para `/?tab=pdv`                            |
| Sidebar → Estoque         | Navega para `/?tab=estoque`                        |
| Sidebar → Ranking         | Navega para `/?tab=ranking`                        |

---

## Estado Global (Custom States)

| Elemento | Estado        | Tipo | Padrão | Descrição                              |
|----------|---------------|------|--------|----------------------------------------|
| `index`  | `current_tab` | text | `pdv`  | Controla qual aba está visível na tela |

---

## Problemas Conhecidos

Os itens abaixo foram identificados pelo validador do Bubble e ainda precisam de
correção:

### Página `index`
- **Group pos-stat-items:** Data source recebe `List of Sales` em vez de `Sale`
- **Search for SaleItems (x2):** `value` deveria ser `Sale`, não `List of Sales`
- **Button pos-barcode-trigger-btn:** Tenta ler dados do grupo pai, que não tem
  tipo de dado configurado
- **Button confirm-sale-btn:** Mesmo problema — grupo pai sem tipo de dado

### Página `catalog`
- **Search for Products (x2):** `value` deveria ser `text`, está recebendo `Product`
- **Button filter-low:** Grupo pai `filter-buttons` sem tipo de dado configurado

### Página `customer`
- **Group last-scanned-card:** Data source deveria ser `SaleItem`, está recebendo `text`
- **Group customer-total-card:** Data source deveria ser `Sale`, está recebendo `number`

### Página `ranking`
- **RepeatingGroup ranking-grid:** Data source vazio (deveria ser `List of SaleItems`)
- **Search for Sales:** `value` deveria ser `date` ou `List of dates`, está
  recebendo `text` ou vazio
- **Display list (x5):** Data sources vazios em múltiplos gráficos/listas

---

## Roadmap / Próximos Passos

- [ ] Corrigir todos os erros de validação listados acima
- [ ] Adicionar estado visual ativo na sidebar (highlight do tab atual)
- [ ] Implementar desconto por venda ou por item
- [ ] Relatório de vendas por período com exportação
- [ ] Controle de acesso por papel (caixa vs. gerente)
- [ ] Suporte a múltiplos operadores simultâneos
- [ ] Notificações automáticas de estoque baixo
- [ ] Remover páginas legadas (`/catalog`, `/ranking`) após validação completa

---

> **Plataforma:** Bubble.io (no-code) · **Versão:** test
> **Moeda:** BRL (R$) · **Idioma principal:** Português

### Navegação por Abas
A página `index` utiliza um **Custom State** `current_tab` (tipo: text,
padrão: `"pdv"`) para controlar qual seção está visível. A navegação ocorre via
parâmetro de URL `?tab=`, lido no evento **Page is loaded**, que define o estado
adequado. Cada grupo de conteúdo possui uma condicional de visibilidade e
**Collapse when hidden** ativado.

---

## Modelo de Dados

### `User`
| Campo   | Tipo | Descrição                  |
|---------|------|----------------------------|
| `name`  | text | Nome do operador/caixa     |
| `theme` | text | Preferência de tema visual |
| `email` | text | E-mail de acesso           |

**Privacidade:** Cada usuário só pode ver e editar os próprios dados.

---

### `Product`
| Campo         | Tipo   | Descrição                        |
|---------------|--------|----------------------------------|
| `name`        | text   | Nome do produto                  |
| `barcode`     | text   | Código de barras (EAN/UPC)       |
| `price`       | number | Preço unitário (R$)              |
| `stock`       | number | Quantidade em estoque            |
| `alert_level` | number | Nível mínimo para alerta de baixo estoque |

---

### `Sale`
| Campo            | Tipo   | Descrição                         |
|------------------|--------|-----------------------------------|
| `status`         | text   | Status da venda (`open`, `closed`)|
| `cashier`        | User   | Operador que realizou a venda     |
| `total`          | number | Valor total da venda (R$)         |
| `closed_at`      | date   | Data/hora de fechamento           |
| `payment_method` | text   | `Pix`, `Card` ou `Cash`           |

---

### `SaleItem`
| Campo        | Tipo    | Descrição                         |
|--------------|---------|-----------------------------------|
| `sale`       | Sale    | Venda à qual o item pertence      |
| `product`    | Product | Produto vendido                   |
| `quantity`   | number  | Quantidade vendida                |
| `unit_price` | number  | Preço unitário no momento da venda|
| `subtotal`   | number  | `quantity × unit_price`           |

---

## Páginas e Navegação

| Página       | Slug        | Descrição                                              |
|--------------|-------------|--------------------------------------------------------|
| **Index**    | `/`         | App principal: PDV, Estoque e Ranking em abas          |
| **Customer** | `/customer` | Tela do cliente — display em tempo real                |
| **Catalog**  | `/catalog`  | ⚠️ Legada — conteúdo migrado para Index               |
| **Ranking**  | `/ranking`  | ⚠️ Legada — conteúdo migrado para Index               |
| **Reset PW** | `/reset_pw` | Redefinição de senha via e-mail                        |
| **404**      | `/404`      | Página de erro com botão "Go Home"                     |

---

## Fluxos de Trabalho (Workflows)

### PDV — `index`
| Trigger                        | Ações                                              |
|--------------------------------|----------------------------------------------------|
| Clique em **Scan / Add Item**  | ShowElement (barcode popup),

### Navegação por Abas
A página `index` utiliza um **Custom State** `current_tab` (tipo: text,
padrão: `"pdv"`) para controlar qual seção está visível. A navegação ocorre via
parâmetro de URL `?tab=`, lido no evento **Page is loaded**, que define o estado
adequado. Cada grupo de conteúdo possui uma condicional de visibilidade e
**Collapse when hidden** ativado.

---

## Modelo de Dados

### `User`
| Campo   | Tipo | Descrição                  |
|---------|------|----------------------------|
| `name`  | text | Nome do operador/caixa     |
| `theme` | text | Preferência de tema visual |
| `email` | text | E-mail de acesso           |


Limitaçoes: Não consigo realizar vendas, o darkmode não funcionou, não consigo adicionar produtos corretamente.
**Privacidade:** Cada usuário só pode ver e editar os próprios dados.

---

### `Product`
| Campo         | Tipo   | Descrição                        |
|---------------|--------|----------------------------------|
| `name`        | text   | Nome do produto                  |
| `bar
