
# ![Apple Logo]([https://github.com/najirh/Apple-Retail-Sales-SQL-Project---Analyzing-Millions-of-Sales-Rows/blob/main/Apple_Changsha_RetailTeamMembers_09012021_big.jpg.slideshow-xlarge_2x.jpg](https://www.google.com/url?sa=i&url=https%3A%2F%2Fwww.istockphoto.com%2Fbr%2Ffotos%2Floja-da-apple&psig=AOvVaw3-mqcJyUb7lLca-b4zBLyM&ust=1761957276458000&source=images&cd=vfe&opi=89978449&ved=0CBUQjRxqFwoTCJDF_b2YzZADFQAAAAAdAAAAABAE)) Projeto SQL de Vendas no Varejo da Apple - Analisando Milhões de Linhas de Vendas

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
2. Calcule o total de unidades vendidas por cada loja.
3. Identifique quantas vendas ocorreram em dezembro de 2023.
4. Determine quantas lojas nunca tiveram uma solicitação de garantia registrada.
5. Calcule a porcentagem de solicitações de garantia marcadas como "Warranty Void".
6. Identifique qual loja teve o maior total de unidades vendidas no último ano.
7. Conte o número de produtos únicos vendidos no último ano.
8. Encontre o preço médio dos produtos em cada categoria.
9. Quantas solicitações de garantia foram registradas em 2020?
10. Para cada loja, identifique o dia com maior volume de vendas, com base na quantidade total de unidades vendidas.

### Nível Intermediário a Avançado (5 Perguntas)

11. Identifique o produto com **menor número de vendas** em cada país para cada ano, com base no total de unidades vendidas.
12. Calcule quantas **solicitações de garantia** foram registradas dentro de **180 dias** após a venda de um produto.
13. Determine quantas solicitações de garantia foram feitas para **produtos lançados nos últimos dois anos**.
14. Liste os **meses dos últimos três anos** em que as vendas **excederam 5.000 unidades nos EUA**.
15. Identifique a **categoria de produto com mais solicitações de garantia** registradas nos últimos dois anos.

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
