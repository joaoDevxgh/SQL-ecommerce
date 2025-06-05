# SQL-ecommerce
Criação das tabelas SQL, modelo de ecommerce
-- Limpar todos os dados e tabelas já criadas antes
DROP DATABASE IF EXISTS ecommerce;
CREATE DATABASE ecommerce;
USE ecommerce;

-- Criação do Banco de Dados
CREATE DATABASE IF NOT EXISTS ecommerce;
USE ecommerce;

-- Tabela Clientes
CREATE TABLE clients (
    idClient INT AUTO_INCREMENT PRIMARY KEY,
    Fname VARCHAR(15) NOT NULL,
    Minit CHAR(3),
    Lname VARCHAR(15) NOT NULL,
    CPF CHAR(11) NOT NULL UNIQUE,
    Address VARCHAR(100) NOT NULL
);

-- Tabela Produtos
CREATE TABLE products (
    idProduct INT AUTO_INCREMENT PRIMARY KEY,
    pname VARCHAR(100) NOT NULL,
    classification_kids BOOLEAN DEFAULT FALSE,
    category ENUM('eletronico','vestimenta','brinquedos','alimentos','moveis') NOT NULL,
    rating FLOAT DEFAULT 0,
    size VARCHAR(10)
);

-- Tabela Pedidos
CREATE TABLE orders (
    idOrder INT AUTO_INCREMENT PRIMARY KEY,
    idClient INT NOT NULL,
    orderStatus ENUM('confirmado','cancelado','em processamento') DEFAULT 'em processamento',
    orderDescription VARCHAR(255),
    shippingValue FLOAT DEFAULT 10,
    paymentCash BOOLEAN DEFAULT FALSE,
    FOREIGN KEY (idClient) REFERENCES clients(idClient)
);

-- Tabela Pagamentos
CREATE TABLE payments (
    idPayment INT AUTO_INCREMENT PRIMARY KEY,
    idClient INT NOT NULL,
    typePayment ENUM('cash','debit','credit','pix') NOT NULL,
    limitAvailable FLOAT,
    FOREIGN KEY (idClient) REFERENCES clients(idClient)
);


-- Tabela Estoque
CREATE TABLE productStorage (
    idProdStorage INT AUTO_INCREMENT PRIMARY KEY,
    storageLocation VARCHAR(255) NOT NULL,
    quantity INT DEFAULT 0
);

-- Tabela Fornecedores
CREATE TABLE suppliers (
    idSupplier INT AUTO_INCREMENT PRIMARY KEY,
    socialName VARCHAR(255) NOT NULL,
    CNPJ CHAR(14) NOT NULL UNIQUE,
    contact CHAR(11) NOT NULL
);

-- Tabela Vendedores
CREATE TABLE sellers (
    idSeller INT AUTO_INCREMENT PRIMARY KEY,
    socialName VARCHAR(255) NOT NULL,
    abstName VARCHAR(255),
    CNPJ CHAR(14) UNIQUE,
    CPF CHAR(11) UNIQUE,
    location VARCHAR(255),
    contact CHAR(11) NOT NULL
);

-- Tabela Produto x Vendedor
CREATE TABLE productSeller (
    idSeller INT NOT NULL,
    idProduct INT NOT NULL,
    prodQuantity INT DEFAULT 1,
    PRIMARY KEY (idSeller, idProduct),
    FOREIGN KEY (idSeller) REFERENCES sellers(idSeller),
    FOREIGN KEY (idProduct) REFERENCES products(idProduct)
);

-- Tabela Produto x Pedido
CREATE TABLE productOrder (
    idProduct INT NOT NULL,
    idOrder INT NOT NULL,
    poQuantity INT DEFAULT 1,
    poStatus ENUM('disponivel', 'sem estoque') DEFAULT 'disponivel',
    PRIMARY KEY (idProduct, idOrder),
    FOREIGN KEY (idProduct) REFERENCES products(idProduct),
    FOREIGN KEY (idOrder) REFERENCES orders(idOrder)
);

-- Tabela Produto x Estoque
CREATE TABLE productStorageLocation (
    idProduct INT NOT NULL,
    idStorage INT NOT NULL,
    location VARCHAR(255) NOT NULL,
    PRIMARY KEY (idProduct, idStorage),
    FOREIGN KEY (idProduct) REFERENCES products(idProduct),
    FOREIGN KEY (idStorage) REFERENCES productStorage(idProdStorage)
);


-- Tabela Produto x Fornecedor
CREATE TABLE productSupplier (
    idSupplier INT NOT NULL,
    idProduct INT NOT NULL,
    quantity INT NOT NULL,
    PRIMARY KEY (idSupplier, idProduct),
    FOREIGN KEY (idSupplier) REFERENCES suppliers(idSupplier),
    FOREIGN KEY (idProduct) REFERENCES products(idProduct)
);
SELECT * FROM clients;
SELECT * FROM products;
SELECT * FROM orders;

SELECT * FROM clients
WHERE Lname = 'Silva';

SELECT * FROM products
WHERE category = 'eletronico';

SELECT * FROM orders
WHERE orderStatus = 'confirmado';

SELECT 
    CONCAT(Fname, ' ', IFNULL(Minit, ''), ' ', Lname) AS fullName,
    CPF,
    Address
FROM clients;

SELECT 
    pname,
    rating,
    (rating + 1) AS adjustedRating,
    (shippingValue + 10) AS totalShipping
FROM products
JOIN productOrder ON products.idProduct = productOrder.idProduct
JOIN orders ON productOrder.idOrder = orders.idOrder;

SELECT * FROM products
ORDER BY rating DESC;

SELECT * FROM clients
ORDER BY Lname ASC;

SELECT 
    idClient,
    COUNT(idOrder) AS totalOrders
FROM orders
GROUP BY idClient
HAVING totalOrders > 1;

SELECT 
    idSeller,
    SUM(prodQuantity) AS totalSold
FROM productSeller
GROUP BY idSeller
HAVING totalSold > 10;

SELECT 
    orders.idOrder,
    CONCAT(c.Fname, ' ', IFNULL(c.Minit, ''), ' ', c.Lname) AS fullName,
    orders.orderStatus,
    orders.orderDescription
FROM orders
JOIN clients c ON orders.idClient = c.idClient;

SELECT 
    CONCAT(c.Fname, ' ', IFNULL(c.Minit, ''), ' ', c.Lname) AS fullName,
    o.idOrder,
    p.pname,
    po.poQuantity,
    o.orderStatus
FROM orders o
JOIN clients c ON o.idClient = c.idClient
JOIN productOrder po ON o.idOrder = po.idOrder
JOIN products p ON po.idProduct = p.idProduct
ORDER BY fullName, o.idOrder;


