# 📚 Livraria - Sistema de Gestão de Vendas e Estoque

Este repositório contém a documentação técnica e o modelo lógico de banco de dados para o **Sistema de Gestão da Livraria Saber**, uma loja especializada na venda de livros e produtos de papelaria.

## 1. Visão Geral do Projeto (Minimundo)

O principal objetivo do sistema é **gerenciar todas as operações de venda, estoque e cadastro** da loja, garantindo a integridade e a rastreabilidade das informações sobre o vasto catálogo de produtos.

### 1.1. Escopo (O que o Sistema Faz)

* **Cadastro:** Gerencia `Clientes`, `Vendedores`, `Autores`, `Editoras`, `Fornecedores` e `Produtos` (Livros e Papelaria).
* **Vendas:** Registra transações detalhadas, controlando a data, valor total e forma de pagamento.
* **Estoque:** Otimiza o controle de estoque para Livros (por ISBN) e Papelaria (por SKU/Categoria).
* **Relatórios:** Permite a geração de relatórios básicos de desempenho (e.g., vendas por período, comissões de vendedores).

### 1.2. Foco da Modelagem

O modelo foi desenvolvido para resolver o desafio de diferenciar os dois tipos de produtos principais (`Livro` e `Papelaria`), mantendo a rastreabilidade em uma única transação de venda.

---

## 2. Modelo Lógico Normalizado (3FN)

O Diagrama Entidade-Relacionamento (DER) foi convertido para um modelo lógico que atende à **Terceira Forma Normal (3FN)**, eliminando redundâncias e garantindo a integridade dos dados.

### 2.1. Definição das Tabelas e Chaves

| Tabela | Chave Primária (PK) | Chaves Estrangeiras (FK) | Propósito Principal |
| :--- | :--- | :--- | :--- |
| **CLIENTE** | `id_cliente` | N/A | Dados de fidelidade do cliente. |
| **VENDEDOR** | `id_vendedor` | N/A | Dados do vendedor responsável pela venda. |
| **FORNECEDOR** | `id_fornecedor` | N/A | Fornecedores de Papelaria. |
| **EDITORA** | `id_editora` | N/A | Editoras de Livros. |
| **AUTOR** | `id_autor` | N/A | Autores de Livros. |
| **LIVRO** | `id_livro` | `id_editora` | Produtos literários no estoque. |
| **PAPELARIA** | `id_papelaria` | `id_fornecedor` | Produtos de papelaria no estoque. |
| **VENDA** | `id_venda` | `id_cliente`, `id_vendedor` | Cabeçalho da transação de venda. |
| **ITEM\_VENDA** | `id_venda`, `seq_item` (Composta) | `id_livro`, `id_papelaria` | Detalhamento da transação (resolve N:N Venda-Produto). |
| **LIVRO\_AUTOR** | `id_livro`, `id_autor` (Composta) | `id_livro`, `id_autor` | Resolve a relação N:N entre Livro e Autor. |

### 2.2. Regras de Integridade Chave

* **Integridade Referencial:** Todas as Chaves Estrangeiras (`FK`) garantem que os dados só possam ser inseridos se fizerem referência a um registro existente nas tabelas primárias.
* **Normalização:** A estrutura está em **3FN**, garantindo que todas as colunas não-chave dependam exclusivamente da chave primária (e nada mais que a chave).
* **Restrição Lógica (XOR):** Na tabela **ITEM\_VENDA**, deve ser aplicada uma restrição lógica que garanta que **apenas um** dos campos (`id_livro` OU `id_papelaria`) seja preenchido por linha, mas nunca ambos.
