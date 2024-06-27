# SQL Scripts for Product Table Operations

Este repositório contém scripts SQL para várias operações na tabela `produtos`. As operações incluem inserções, atualizações, consultas específicas e junções com outras tabelas.

## Tabela de Produtos

### Estrutura da Tabela
| Campo         | Tipo de Campo    | Chave |
|---------------|------------------|-------|
| cod_prod      | Integer (8)      | X     |
| loj_prod      | Integer (8)      | X     |
| desc_prod     | Char (40)        |       |
| dt_inclu_prod | Data (dd/mm/yyyy)|       |
| preco_prod    | decimal (8,3)    |       |

## Operações SQL

### Inserir um registro na tabela `produtos`

```sql
INSERT INTO produtos (cod_prod, loj_prod, desc_prod, dt_inclu_prod, preco_prod)
VALUES (170, 2, 'LEITE CONDENSADO MOCOCA', '30/12/2010', 45.40);
---------------------------------------------
Alterar o preço do produto para R$95,40
UPDATE produtos
SET preco_prod = 95.40
WHERE cod_prod = 170 AND loj_prod = 2;

---------------------------------------------
Selecionar todos os registros das lojas 1 e 2

SELECT * FROM produtos
WHERE loj_prod IN (1, 2);
----------------------------------------------
Selecionar a maior e a menor data de inclusão do produto dt_inclu_prod
SELECT MAX(dt_inclu_prod) AS data_mais_recente, MIN(dt_inclu_prod) AS data_mais_antiga
FROM produtos;
----------------------------------------------
Selecionar a quantidade total de registros na tabela de produtos
SELECT COUNT(*) AS quantidade_total
FROM produtos;
----------------------------------------------
Selecionar todos os produtos que começam com a letra "L"
SELECT * FROM produtos
WHERE desc_prod LIKE 'L%';
----------------------------------------------
Selecionar a soma de todos os preços dos produtos totalizados por loja
SELECT loj_prod, SUM(preco_prod) AS soma_precos
FROM produtos
GROUP BY loj_prod;
----------------------------------------------
Selecionar a soma de todos os preços dos produtos totalizados por loja que seja maior que R$100.000
SELECT loj_prod, SUM(preco_prod) AS soma_precos
FROM produtos
GROUP BY loj_prod
HAVING SUM(preco_prod) > 100000;
----------------------------------------------
Consultas Avançadas
Selecionar os detalhes dos produtos para a loja 1
SELECT l.loj_prod, l.desc_loj, p.cod_prod, p.desc_prod, p.preco_prod, e.qtd_prod
FROM produtos p
JOIN estoque e ON p.cod_prod = e.cod_prod AND p.loj_prod = e.loj_prod
JOIN lojas l ON p.loj_prod = l.loj_prod
WHERE p.loj_prod = 1;
----------------------------------------------
Selecionar produtos que existem na tabela de produtos e não na tabela de estoque
SELECT * 
FROM produtos p
WHERE NOT EXISTS (
    SELECT 1
    FROM estoque e
    WHERE p.cod_prod = e.cod_prod AND p.loj_prod = e.loj_prod
);
----------------------------------------------
Selecionar produtos que existem na tabela de produtos e não na tabela de estoque
SELECT * 
FROM produtos p
WHERE NOT EXISTS (
    SELECT 1
    FROM estoque e
    WHERE p.cod_prod = e.cod_prod AND p.loj_prod = e.loj_prod
);
----------------------------------------------
Selecionar produtos que existem na tabela de estoque e não na tabela de produtos
SELECT * 
FROM estoque e
WHERE NOT EXISTS (
    SELECT 1
    FROM produtos p
    WHERE e.cod_prod = p.cod_prod AND e.loj_prod = p.loj_prod
);
----------------------------------------------



