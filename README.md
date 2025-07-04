# Análise de Dados de Vendas - MVP de Boas Práticas

**Autor:** Wellington Sales Chaves  
**Matrícula:** 2025000  
**Dataset original:** [Relatórios de Vendas - Kaggle](https://www.kaggle.com/datasets/dienert/vendas)

---

## Descrição do Problema

O conjunto de dados **Vendas** é um dataset transacional que contém registros detalhados sobre transações comerciais de uma empresa, incluindo:

- Data da venda
- Nome do produto
- Categoria do serviço
- Quantidade vendida
- Preço unitário
- Receita gerada

O objetivo é realizar uma análise descritiva e exploratória para entender o desempenho financeiro e comercial da empresa ao longo do tempo.

---

## Hipóteses da Análise

1. Produtos com maior preço unitário resultam em maior receita total?
2. Existe correlação entre a quantidade vendida e o valor total da venda?
3. Determinados produtos geram lucro de forma mais consistente ao longo do tempo?
4. As vendas aumentam em determinadas épocas do ano?
5. Vendedores específicos geram mais lucro do que outros?
6. Existem padrões nas vendas de acordo com a data?

---

## Tipo de Problema

Trata-se de uma **análise exploratória de dados (EDA)** não supervisionada, com foco na identificação de padrões, tendências e anomalias.

---

## Fonte dos Dados

Os dados foram obtidos do Kaggle:  
🔗 [https://www.kaggle.com/datasets/dienert/vendas](https://www.kaggle.com/datasets/dienert/vendas)

O conjunto contém 44.500 amostras com os seguintes campos principais:

- `name`: Vendedor
- `sale_id`: ID da venda
- `product_id`: ID do produto
- `product`: Nome do produto
- `price_y`: Preço unitário
- `price_x`: Custo do produto
- `quantity`: Quantidade vendida
- `created_at`: Data e hora da venda
- `updated_at`: Última atualização
- `email`: Contato associado

---

## Etapas de Processamento e Pré-Processamento

```python
# Leitura do arquivo CSV
df = pd.read_csv('/content/Vendas.csv')

# Renomeando colunas para nomes mais intuitivos
df.rename(columns={
    'price_y': 'preco_unitario',
    'price_x': 'custo',
    'quantity': 'quantidade',
    'name': 'vendedor',
    'product': 'produtos',
    'created_at': 'data_venda',
    'updated_at': 'data_atualizacao'
}, inplace=True)

# Cálculo de métricas
df['receita'] = df['quantidade'] * df['preco_unitario']
df['lucro'] = df['receita'] - df['custo']

# Conversão e manipulação de datas
df['data_venda'] = pd.to_datetime(df['data_venda'], dayfirst=True)
df['created_at'] = pd.to_datetime(df['data_venda'], format='%d/%m/%Y')
df['mes'] = df['created_at'].dt.month
df['ano_mes'] = df['created_at'].dt.to_period('M').dt.to_timestamp()
df['dia_semana'] = df['created_at'].dt.dayofweek
df['decada'] = (df['created_at'].dt.year // 10) * 10
