# Joins e Subqueries em Banco de Dados
## Tutorial de Construção Progressiva do Conhecimento (PBL)

**Curso:** Análise e Desenvolvimento de Sistemas  
**Disciplina:** Banco de Dados  
**Contexto:** E-commerce

---

## Como estudar este material

Neste tutorial, você não recebe a resposta pronta de imediato.
Em cada etapa, o fluxo é:

1. Ler o problema de negócio.
2. Tentar escrever a consulta.
3. Comparar com a solução guiada.
4. Registrar o que aprendeu no checkpoint.

Se você pular a etapa de tentativa, perde a parte mais importante da aprendizagem.

---

## Cenário da aula

Você trabalha no time de dados da loja virtual **LX**.
Financeiro, Marketing e CRM pedem respostas rápidas usando SQL.

Objetivo final: chegar ao ponto de combinar **joins** e **subqueries** para resolver perguntas reais de negócio.

---

## Base mínima para praticar

### 1) Estrutura das tabelas

```sql
CREATE TABLE customers (
  customer_id SERIAL PRIMARY KEY,
  full_name VARCHAR(100),
  email VARCHAR(120),
  city VARCHAR(80)
);

CREATE TABLE categories (
  category_id SERIAL PRIMARY KEY,
  category_name VARCHAR(80)
);

CREATE TABLE products (
  product_id SERIAL PRIMARY KEY,
  product_name VARCHAR(120),
  category_id INT,
  price DECIMAL(10,2),
  FOREIGN KEY (category_id) REFERENCES categories(category_id)
);

CREATE TABLE orders (
  order_id SERIAL PRIMARY KEY,
  customer_id INT,
  order_date DATE,
  status VARCHAR(30),
  FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

CREATE TABLE order_items (
  order_id INT,
  product_id INT,
  quantity INT,
  unit_price DECIMAL(10,2),
  PRIMARY KEY (order_id, product_id),
  FOREIGN KEY (order_id) REFERENCES orders(order_id),
  FOREIGN KEY (product_id) REFERENCES products(product_id)
);

CREATE TABLE payments (
  payment_id SERIAL PRIMARY KEY,
  order_id INT,
  paid_amount DECIMAL(10,2),
  payment_date DATE,
  payment_method VARCHAR(30),
  FOREIGN KEY (order_id) REFERENCES orders(order_id)
);
```

### 2) Dados de exemplo

```sql
INSERT INTO customers (full_name, email, city) VALUES
('Ana Lima', 'ana@email.com', 'Recife'),
('Bruno Melo', 'bruno@email.com', 'Salvador'),
('Carla Souza', 'carla@email.com', 'Recife'),
('Diego Reis', 'diego@email.com', 'Fortaleza');

INSERT INTO categories (category_name) VALUES
('Electronics'),
('Home'),
('Books');

INSERT INTO products (product_name, category_id, price) VALUES
('Wireless Mouse', 1, 120.00),
('Mechanical Keyboard', 1, 350.00),
('Desk Lamp', 2, 90.00),
('SQL Fundamentals', 3, 150.00);

INSERT INTO orders (customer_id, order_date, status) VALUES
(1, CURRENT_DATE - INTERVAL '10 days', 'PAID'),
(1, CURRENT_DATE - INTERVAL '95 days', 'PAID'),
(2, CURRENT_DATE - INTERVAL '5 days', 'CANCELLED'),
(3, CURRENT_DATE - INTERVAL '2 days', 'PENDING');

INSERT INTO order_items (order_id, product_id, quantity, unit_price) VALUES
(1, 1, 1, 120.00),
(1, 4, 1, 150.00),
(2, 2, 1, 350.00),
(3, 3, 2, 90.00),
(4, 4, 1, 150.00);

INSERT INTO payments (order_id, paid_amount, payment_date, payment_method) VALUES
(1, 270.00, CURRENT_DATE - INTERVAL '9 days', 'CREDIT_CARD'),
(2, 350.00, CURRENT_DATE - INTERVAL '94 days', 'PIX');
```

---

## Missão 1: Descobrir por que JOIN existe

**Problema:** o Financeiro quer `order_id`, `order_date` e nome do cliente na mesma consulta.

### Tente primeiro (3 min)

Escreva sua SQL antes de olhar a solução.

Perguntas-guia:
- Em qual tabela está o nome do cliente?
- Em qual tabela está a data do pedido?
- Qual coluna conecta as duas?

### Solução guiada

```sql
SELECT
  o.order_id,
  o.order_date,
  c.full_name AS customer_name,
  o.status
FROM orders o
INNER JOIN customers c
  ON o.customer_id = c.customer_id;
```

### Checkpoint de aprendizagem

Complete:
- Usei `INNER JOIN` porque __________________________________.
- A chave de relacionamento foi ____________________________.

---

## Missão 2: Entender diferença entre INNER e LEFT

**Problema:** o Marketing quer todos os clientes, inclusive quem nunca comprou.

### Tente primeiro (4 min)

Sem executar, responda:
- `INNER JOIN` mostraria clientes sem pedido?
- Qual join preserva todas as linhas da tabela de clientes?

### Solução guiada

```sql
SELECT
  c.customer_id,
  c.full_name,
  o.order_id
FROM customers c
LEFT JOIN orders o
  ON c.customer_id = o.customer_id
ORDER BY c.customer_id;
```

Para listar apenas quem nunca comprou:

```sql
SELECT
  c.customer_id,
  c.full_name
FROM customers c
LEFT JOIN orders o
  ON c.customer_id = o.customer_id
WHERE o.order_id IS NULL;
```

### Checkpoint de aprendizagem

Explique com suas palavras a regra: `LEFT JOIN + IS NULL`.

---

## Missão 3: Identificar ausência de pagamento

**Problema:** a Operação quer pedidos sem pagamento registrado.

### Tente primeiro (3 min)

Dica progressiva:
1. Comece por `orders`.
2. Junte com `payments`.
3. Filtre os casos sem correspondência.

### Solução guiada

```sql
SELECT
  o.order_id,
  o.order_date,
  o.status
FROM orders o
LEFT JOIN payments p
  ON o.order_id = p.order_id
WHERE p.payment_id IS NULL;
```

### Checkpoint de aprendizagem

Se você trocar por `INNER JOIN`, o que muda no resultado?

---

## Missão 4: Construir a primeira subquery

**Problema:** listar produtos com preço acima da média.

### Etapa A (isolada)

Antes da subquery, calcule só a média:

```sql
SELECT AVG(price) AS avg_price
FROM products;
```

### Etapa B (composição)

Agora use o valor da média dentro do `WHERE`:

```sql
SELECT
  product_id,
  product_name,
  price
FROM products
WHERE price > (
  SELECT AVG(price)
  FROM products
);
```

### Checkpoint de aprendizagem

Esta subquery retorna:
- ( ) uma linha e uma coluna
- ( ) várias linhas

---

## Missão 5: Subquery com IN

**Problema:** quais clientes já tiveram pedido cancelado?

### Tente primeiro (4 min)

Passos esperados:
1. Descobrir `customer_id` dos pedidos cancelados.
2. Buscar os clientes desse conjunto.

### Solução guiada

```sql
SELECT
  customer_id,
  full_name
FROM customers
WHERE customer_id IN (
  SELECT customer_id
  FROM orders
  WHERE status = 'CANCELLED'
);
```

### Checkpoint de aprendizagem

Quando usar `IN`?
- ( ) Quando a subquery retorna conjunto de valores
- ( ) Quando a subquery retorna apenas um valor

---

## Missão 6: Subquery com EXISTS

**Problema:** quais categorias têm pelo menos um produto vendido?

### Tente primeiro (5 min)

Pense na lógica:
- Para cada categoria, existe ao menos uma linha relacionada em vendas?

### Solução guiada

```sql
SELECT
  c.category_id,
  c.category_name
FROM categories c
WHERE EXISTS (
  SELECT 1
  FROM products pr
  JOIN order_items oi
    ON oi.product_id = pr.product_id
  WHERE pr.category_id = c.category_id
);
```

### Checkpoint de aprendizagem

Por que a condição `pr.category_id = c.category_id` é essencial?

---

## Missão 7: Join + subquery em problema real

**Problema:** CRM quer clientes cujo último pedido foi há mais de 90 dias.

### Tente primeiro (6 min)

Estratégia sugerida:
1. Descobrir a última compra por cliente (`MAX(order_date)`).
2. Juntar com `customers`.
3. Filtrar datas antigas.

### Solução guiada

```sql
SELECT
  c.customer_id,
  c.full_name,
  last_order.last_order_date
FROM customers c
JOIN (
  SELECT
    customer_id,
    MAX(order_date) AS last_order_date
  FROM orders
  GROUP BY customer_id
) last_order
  ON last_order.customer_id = c.customer_id
WHERE last_order.last_order_date < CURRENT_DATE - INTERVAL '90 days';
```

### Checkpoint de aprendizagem

Esta solução usa:
- Join entre consulta externa e subquery no `FROM`
- Agregação com `MAX`
- Filtro de data

---

## Síntese construída pelo aluno

Complete ao final da leitura:

1. Para trazer apenas correspondências, uso __________________.
2. Para preservar tudo da tabela da esquerda, uso __________________.
3. Para filtrar por ausência de relação, combino __________________.
4. Para comparar com média geral, uso __________________.
5. Para verificar existência de linhas relacionadas, uso __________________.

---

## Desafio integrador final (sem solução pronta)

Resolva em dupla e valide com o professor:

1. Liste os 5 clientes com maior faturamento total.
2. Liste produtos nunca vendidos.
3. Liste pedidos cujo total foi maior que a média dos pedidos do próprio cliente.
4. Para cada categoria, mostre o produto mais caro (aceitando empate).

Critério de sucesso:
- SQL correta
- justificativa da estratégia
- leitura de negócio do resultado

---

## Fechamento

Se você chegou até aqui resolvendo as missões, construiu progressivamente:
- base sólida de joins;
- domínio de subqueries em cenários reais;
- capacidade de traduzir perguntas de negócio em SQL.
