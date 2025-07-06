## Análise de Dados de Vendas - MVP de Boas Práticas
Autor: Wellington Sales Chaves
Matrícula: 2025000
Dataset: Relatórios de Vendas - Kaggle

## Descrição do Projeto
Este projeto tem como objetivo realizar uma Análise Exploratória de Dados (EDA) com foco na compreensão do desempenho financeiro e comercial de uma empresa ao longo do tempo.

O conjunto de dados transacional contém informações sobre:
Data da venda
Nome do produto
Categoria do serviço
Quantidade vendida
Preço unitário
Receita gerada
Vendedor
Custo do produto

## Hipóteses Investigadas
Produtos com maior preço unitário resultam em maior receita total?
Existe correlação entre a quantidade vendida e o valor total da venda?
Determinados produtos geram lucro de forma mais consistente ao longo do tempo?
As vendas aumentam em determinadas épocas do ano?
Vendedores específicos geram mais lucro do que outros?
Existem padrões nas vendas de acordo com a data?

## Tipo de Problema
Análise de dados não supervisionada, com foco na identificação de padrões, tendências, anomalias e respostas às hipóteses de negócio.

## Fonte dos Dados
O dataset foi obtido na plataforma Kaggle e contém 44.500 registros.
Campos principais:

sale_id, product_id: IDs únicos
produtos, categoria_servico, vendedor: categóricas
preco_unitario, custo, quantidade, lucro, receita: numéricas
data_venda, updated_at, created_at: datas

## Pré-Processamento
python
Copiar
Editar
# Leitura
df = pd.read_csv('/content/Vendas.csv')

# Renomeando colunas
df.rename(columns={
    'price_y': 'preco_unitario',
    'price_x': 'custo',
    'quantity': 'quantidade',
    'name': 'vendedor',
    'product': 'produtos',
    'created_at': 'data_venda',
    'updated_at': 'data_atualizacao'
}, inplace=True)

# Métricas derivadas
df['receita'] = df['quantidade'] * df['preco_unitario']
df['lucro'] = df['receita'] - df['custo']

# Datas
df['data_venda'] = pd.to_datetime(df['data_venda'], dayfirst=True)
df['mes'] = df['data_venda'].dt.month
df['ano_mes'] = df['data_venda'].dt.to_period('M').dt.to_timestamp()
df['dia_semana'] = df['data_venda'].dt.dayofweek
df['decada'] = (df['data_venda'].dt.year // 10) * 10

## Análise Exploratória (EDA)
Durante a EDA, foram investigadas as seguintes dimensões:
Volume de vendas por década, produto e vendedor
Distribuição da receita e custos
Padrões temporais: mês, dia da semana e década
Correlação entre variáveis quantitativas
Análise de outliers e inconsistências

## Estatísticas Descritivas
Média por década (gráfico de barras)
Desvio padrão das métricas financeiras
Boxplots por década para identificar dispersões e outliers
Histogramas com curvas KDE para:
Quantidade
Preço unitário
Receita
Custo

## Matriz de Correlação
Correlação entre:
quantidade
receita
preco_unitario
custo
lucro

## Normalização
Aplicada para preparar os dados para futuras aplicações de machine learning.

## Resultados das Hipóteses
Hipótese	Resultado
Produtos com maior preço unitário geram mais receita?	❌ Nem sempre. Produtos caros vendem menos.
Quantidade vendida está correlacionada com receita?	✅ Sim, forte correlação positiva.
Alguns produtos geram lucro de forma mais constante?	✅ Sim, há produtos com lucro estável ao longo do tempo.
Há sazonalidade nas vendas?	✅ Sim, certos meses têm maior volume.
Alguns vendedores geram mais lucro?	✅ Sim, existem vendedores com desempenho muito superior.
Existem padrões temporais nas vendas?	✅ Sim, certos dias da semana concentram maior volume.

## Conclusão
A análise demonstrou como a compreensão profunda dos dados pode revelar padrões ocultos e orientar decisões estratégicas de vendas, marketing e estoque. A exploração dos dados financeiros e temporais permitiu validar hipóteses importantes sobre comportamento de compra, desempenho de produtos e vendedores, e sazonalidade nas vendas.
