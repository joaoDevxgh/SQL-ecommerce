🏗️ Estrutura do Banco de Dados
O banco de dados ecommerce é composto por 10 tabelas principais:

🧍‍♂️ clients – Clientes

📦 products – Produtos

🛒 orders – Pedidos

💳 payments – Pagamentos

🏢 suppliers – Fornecedores

🧑‍💼 sellers – Vendedores

🏬 productStorage – Estoques

🔗 productSeller – Relacionamento Produto x Vendedor

🔗 productOrder – Relacionamento Produto x Pedido

🔗 productSupplier – Relacionamento Produto x Fornecedor

🚀 Como Executar
Clone este repositório:
git clone https://github.com/seu-usuario/ecommerce-sql.git
cd ecommerce-sql

🗺️ Modelo Relacional (Resumido)
clients ───────── orders ─────── productOrder ─────── products
    │                     │                              │
payments                  │                              │
                          └────── productSeller ─────────┘
suppliers ─── productSupplier
products ─── productStorageLocation ─── productStorage

📘 Conceitos Envolvidos
Modelagem Relacional: Criação de tabelas com relações 1:N e N:N.

Integridade Referencial: Uso de FOREIGN KEY.

SQL DDL: Criação e estruturação de dados (CREATE, DROP).

SQL DML: Consultas e manipulações (SELECT, JOIN, WHERE, GROUP BY, ORDER BY).

Consultas Relacionais: Cruzamento de dados entre tabelas.
