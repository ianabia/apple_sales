
# Projeto SQL de Vendas no Varejo da Apple - Analisando Milhões de Linhas de Vendas

## Visão Geral do Projeto

Desenvolvi este projeto para aplicar e demonstrar técnicas avançadas de SQL na análise de um grande volume de dados — mais de 1 milhão de linhas de vendas da Apple. O conjunto de dados abrange informações sobre produtos, lojas, transações e solicitações de garantia de diversas unidades da Apple ao redor do mundo.

Ao responder a perguntas que vão desde as mais simples até as mais complexas, explorei diferentes abordagens para extrair insights valiosos de grandes bases de dados, mostrando minha capacidade de escrever consultas SQL otimizadas e analíticas.

Este projeto foi uma excelente oportunidade para aprofundar minhas habilidades em SQL e simular desafios reais enfrentados por analistas de dados em contextos corporativos.

## Diagrama de Entidade e Relacionamento (ERD)

![ERD](https://github.com/najirh/Apple-Retail-Sales-SQL-Project---Analyzing-Millions-of-Sales-Rows/blob/main/erd.png)

## Database

O projeto utiliza cinco tabelas principais:

1. **stores**: Contém informações sobre as lojas da Apple.
   - `store_id`: Identificador único de cada loja.
   - `store_name`: Nome da loja.
   - `city`: Cidade onde a loja está localizada.
   - `country`: País da loja.

2. **category**: Armazena informações sobre as categorias de produtos.
   - `category_id`: Identificador único de cada categoria de produto.
   - `category_name`: Nome da categoria.

3. **products**: Detalha os produtos da Apple.
   - `product_id`: Identificador único de cada produto.
   - `product_name`: Nome do produto.
   - `category_id`: Referência para a tabela de categorias.
   - `launch_date`: Data de lançamento do produto.
   - `price`: Preço do produto.

4. **sales**: Armazena as transações de vendas.
   - `sale_id`: Identificador único de cada venda.
   - `sale_date`: Data da venda.
   - `store_id`: Referência para a tabela de lojas.
   - `product_id`: Referência para a tabela de produtos.
   - `quantity`: Quantidade de unidades vendidas.

5. **warranty**: Contém informações sobre solicitações de garantia.
   - `claim_id`: Identificador único de cada solicitação de garantia.
   - `claim_date`: Data em que a solicitação foi feita.
   - `sale_id`: Referência para a tabela de vendas.
   - `repair_status`: Status da solicitação de garantia (ex.: Reparado Pago, Garantia Anulada).

## Objetivos

O projeto é dividido em três níveis de perguntas, criadas para testar habilidades em SQL com graus crescentes de complexidade:

### Nível Fácil a Intermediário (10 Perguntas)

1. Encontre o número de lojas em cada país.
 ```sql
SELECT
  r.last_name
FROM
  riders AS r
  INNER JOIN bikes AS b ON r.bike_vin_num = b.vin_num
  INNER JOIN crew AS c ON r.crew_chief_last_name = c.last_name
WHERE
  b.engine_tally > 2;
```

2. Calcule o total de unidades vendidas por cada loja.
```sql
SELECT 
	s.store_id,
	st.store_name,
	SUM(s.quantity) as total_unit_sold
FROM sales as s
JOIN
stores as st
ON st.store_id = s.store_id
GROUP BY 1, 2
ORDER BY 3 DESC
```

3. Identifique quantas vendas ocorreram em dezembro de 2023.
```sql
SELECT 
	COUNT(sale_id) as total_sale 
FROM sales
WHERE TO_CHAR(sale_date, 'MM-YYYY') = '12-2023'
```
4. Determine quantas lojas nunca tiveram uma solicitação de garantia registrada.
```sql
SELECT COUNT(*) FROM stores
WHERE store_id NOT IN (
						SELECT 
							DISTINCT store_id
						FROM sales as s
						RIGHT JOIN warranty as w
						ON s.sale_id = w.sale_id
						);
```

5. Calcule a porcentagem de solicitações de garantia marcadas como "Warranty Void".
```sql
no claim that as wv/total claim * 100

SELECT 
	ROUND
		(COUNT(claim_id)/
						(SELECT COUNT(*) FROM warranty)::numeric 
		* 100, 
	2)as warranty_void_percentage
FROM warranty
WHERE repair_status = 'Warranty Void'
```

6. Identifique qual loja teve o maior total de unidades vendidas no último ano.
```sql
SELECT 
	s.store_id,
	st.store_name,
	SUM(s.quantity)
FROM sales as s
JOIN stores as st
ON s.store_id = st.store_id
WHERE sale_date >= (CURRENT_DATE - INTERVAL '1 year')
GROUP BY 1, 2
ORDER BY 3 DESC
LIMIT 1
```
   
7. Conte o número de produtos únicos vendidos no último ano.
```sql
SELECT 
	COUNT(DISTINCT product_id)
FROM sales
WHERE sale_date >= (CURRENT_DATE - INTERVAL '1 year')
```

8. Encontre o preço médio dos produtos em cada categoria.
```sql
SELECT 
	p.category_id,
	c.category_name,
	AVG(p.price) as avg_price
FROM products as p
JOIN 
category as c
ON p.category_id = c.category_id
GROUP BY 1, 2
ORDER BY 3 DESC
```

9. Quantas solicitações de garantia foram registradas em 2020?
```sql
SELECT 
	COUNT(*) as warranty_claim
FROM warranty
WHERE EXTRACT(YEAR FROM claim_date) = 2020
```
 
10. Para cada loja, identifique o dia com maior volume de vendas, com base na quantidade total de unidades vendidas.
```sql
SELECT  * 
FROM
(
	SELECT 
		store_id,
		TO_CHAR(sale_date, 'Day') as day_name,
		SUM(quantity) as total_unit_sold,
		RANK() OVER(PARTITION BY store_id ORDER BY SUM(quantity) DESC) as rank
	FROM sales
	GROUP BY 1, 2
) as t1
WHERE rank = 1
```

### Nível Intermediário a Avançado (5 Perguntas)

11. Identifique o produto com **menor número de vendas** em cada país para cada ano, com base no total de unidades vendidas.
```sql
WITH product_rank
AS
(
SELECT 
	st.country,
	p.product_name,
	SUM(s.quantity) as total_qty_sold,
	RANK() OVER(PARTITION BY st.country ORDER BY SUM(s.quantity)) as rank
FROM sales as s
JOIN 
stores as st
ON s.store_id = st.store_id
JOIN
products as p
ON s.product_id = p.product_id
GROUP BY 1, 2
)
SELECT 
* 
FROM product_rank
WHERE rank = 1
```

12. Calcule quantas **solicitações de garantia** foram registradas dentro de **180 dias** após a venda de um produto.
```sql
SELECT 
	COUNT(*)
FROM warranty as w
LEFT JOIN 
sales as s
ON s.sale_id = w.sale_id
WHERE 
	w.claim_date - sale_date <= 180
```
13. Determine quantas solicitações de garantia foram feitas para **produtos lançados nos últimos dois anos**.
```sql
SELECT 
	p.product_name,
	COUNT(w.claim_id) as no_claim,
	COUNT(s.sale_id)
FROM warranty as w
RIGHT JOIN
sales as s 
ON s.sale_id = w.sale_id
JOIN products as p
ON p.product_id = s.product_id
WHERE p.launch_date >= CURRENT_DATE - INTERVAL '2 years'
GROUP BY 1
HAVING COUNT(w.claim_id) > 0
```

14. Liste os **meses dos últimos três anos** em que as vendas **excederam 5.000 unidades nos EUA**.
```sql
SELECT 
	TO_CHAR(sale_date, 'MM-YYYY') as month,
	SUM(s.quantity) as total_unit_sold
FROM sales as s
JOIN 
stores as st
ON s.store_id = st.store_id
WHERE 
	st.country = 'USA'
	AND
	s.sale_date >= CURRENT_DATE - INTERVAL '3 year'
GROUP BY 1
HAVING SUM(s.quantity) > 5000
```
15. Identifique a **categoria de produto com mais solicitações de garantia** registradas nos últimos dois anos.
```sql
SELECT 
	c.category_name,
	COUNT(w.claim_id) as total_claims
FROM warranty as w
LEFT JOIN
sales as s
ON w.sale_id = s.sale_id
JOIN products as p
ON p.product_id = s.product_id
JOIN 
category as c
ON c.category_id = p.category_id
WHERE 
	w.claim_date >= CURRENT_DATE - INTERVAL '2 year'
GROUP BY 1
```

### Nível Avançado (5 Perguntas)

16. Determine a **porcentagem de chance de receber solicitações de garantia** após cada compra para cada país.
17. Analise a **taxa de crescimento ano a ano** de cada loja.
18. Calcule a **correlação entre preço do produto e solicitações de garantia** para produtos vendidos nos últimos cinco anos, segmentados por faixa de preço.
19. Identifique a **loja com a maior porcentagem de solicitações "Paid Repaired"** em relação ao total de solicitações registradas.
20. Escreva uma consulta para calcular o **total acumulado mensal de vendas** de cada loja nos últimos quatro anos e comparar as tendências nesse período.

## Foco do Projeto

Este projeto foca principalmente no desenvolvimento e demonstração das seguintes habilidades em SQL:

- **Junções Complexas e Agregações**: Demonstrar a capacidade de realizar junções complexas em SQL e agregar dados de forma significativa.
- **Funções de Janela**: Utilizar funções de janela avançadas para totais acumulados, análise de crescimento e consultas baseadas em tempo.
- **Segmentação de Dados**: Analisar dados em diferentes períodos para obter insights sobre o desempenho de produtos.
- **Análise de Correlação**: Aplicar funções SQL para determinar relações entre variáveis, como preço do produto e solicitações de garantia.
- **Resolução de Problemas do Mundo Real**: Responder a perguntas relacionadas a negócios que refletem cenários reais enfrentados por analistas de dados.


## Dataset

- **Tamanho**: Mais de 1 milhão de registros de vendas.
- **Período Abrangido**: Os dados cobrem vários anos, permitindo análise de tendências de longo prazo.
- **Cobertura Geográfica**: Dados de vendas de lojas da Apple em diversos países.

## Conclusão

Ao concluir este projeto, eu desenvolvi habilidades avançadas em consultas SQL, melhorei minha capacidade de trabalhar com grandes volumes de dados e adquiri experiência prática na resolução de problemas complexos de análise de dados, que são fundamentais para a tomada de decisões de negócios. Esse projeto é uma ótima adição ao meu portfólio e demonstra minha expertise em SQL para potenciais empregadores.

---
