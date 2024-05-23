## 1. Importar Bibliotecas, Carregar Dados e Configurar Bibliotecas de Gráficos
**Objetivo**: Importar bibliotecas necessárias para a análise, carregar os dados de um arquivo CSV e configurar a aparência dos gráficos.

**Passos**:
1. **Importar Bibliotecas**:
    - `pandas` é usado para manipulação e análise de dados.
    - `matplotlib` e `seaborn` são usados para criar gráficos.

    ```python
    import pandas as pd
    import matplotlib.pyplot as plt
    import seaborn as sns
    ```

2. **Carregar Dados**:
    - Use `pandas` para carregar os dados de um arquivo CSV.

    ```python
    dados_vendas = pd.read_csv('dados_vendas.csv')
    print(dados_vendas.head())
    ```

3. **Configurar Gráficos**:
    - Configure o estilo dos gráficos para torná-los mais agradáveis.

    ```python
    sns.set(style="whitegrid")
    plt.figure(figsize=(12, 6))
    ```

## 2. Verificar e Limpar a Coluna de Data
**Objetivo**: Garantir que a coluna de data está limpa e no formato correto.

**Passos**:
1. **Remover Espaços em Branco**:
    - Remova espaços em branco ao redor dos valores na coluna `Data`.

    ```python
    dados_vendas['Data'] = dados_vendas['Data'].str.strip()
    ```

2. **Converter para Formato Datetime**:
    - Converta a coluna `Data` para o tipo datetime, o que facilita as operações de data.

    ```python
    dados_vendas['Data'] = pd.to_datetime(dados_vendas['Data'], errors='coerce')
    ```

3. **Remover Datas Inválidas (NaT)**:
    - Remova linhas com datas inválidas (NaT).

    ```python
    dados_vendas = dados_vendas.dropna(subset=['Data'])
    ```

## 3. Converter a Coluna de Data e Analisar Tendências de Vendas ao Longo do Tempo
**Objetivo**: Verificar como as vendas mudam ao longo do tempo.

**Passos**:
1. **Criar Coluna AnoMes**:
    - Extraia o ano e mês da coluna `Data`.

    ```python
    dados_vendas['AnoMes'] = dados_vendas['Data'].dt.to_period('M')
    ```

2. **Agrupar por AnoMes e Calcular Receita**:
    - Agrupe os dados por `AnoMes` e some a receita.

    ```python
    receita_mensal = dados_vendas.groupby('AnoMes')['Receita'].sum().reset_index()
    ```

3. **Converter AnoMes para String**:
    - Facilita a visualização ao converter `AnoMes` para string.

    ```python
    receita_mensal['AnoMes'] = receita_mensal['AnoMes'].astype(str)
    ```

4. **Plotar Receita Mensal**:
    - Crie um gráfico de linha mostrando a receita mensal ao longo do tempo.

    ```python
    plt.figure(figsize=(12, 6))
    sns.lineplot(data=receita_mensal, x='AnoMes', y='Receita', marker='o')
    plt.title('Receita Mensal ao Longo do Tempo')
    plt.xlabel('Ano-Mês')
    plt.ylabel('Receita')
    plt.xticks(rotation=45)
    plt.show()
    ```

## 4. Análise Sazonal
**Objetivo**: Identificar variações sazonais nas vendas.

**Passos**:
1. **Criar Coluna para o Mês**:
    - Extraia o mês da coluna `Data`.

    ```python
    dados_vendas['Mes'] = dados_vendas['Data'].dt.month
    ```

2. **Agrupar por Mês e Somar Receita**:
    - Agrupe os dados por mês e some a receita.

    ```python
    receita_mensal_sazonal = dados_vendas.groupby('Mes')['Receita'].sum().reset_index()
    ```

3. **Plotar Receita Mensal Sazonal**:
    - Crie um gráfico de barras mostrando a receita por mês.

    ```python
    plt.figure(figsize=(12, 6))
    sns.barplot(data=receita_mensal_sazonal, x='Mes', y='Receita', palette='coolwarm', dodge=False)
    plt.title('Receita Mensal ao Longo dos Anos')
    plt.xlabel('Mês')
    plt.ylabel('Receita')
    plt.legend([],[], frameon=False)
    plt.show()
    ```

## 5. Receita e Quantidade Vendida por Produto e Categoria
**Objetivo**: Identificar os produtos e categorias mais lucrativos.

**Passos**:
1. **Agrupar por Produto e Calcular Receita e Quantidade**:
    - Agrupe os dados por produto e some a receita e quantidade vendida.

    ```python
    produto_analise = dados_vendas.groupby('Produto').agg({'Receita': 'sum', 'Quantidade': 'sum'}).reset_index()
    ```

2. **Agrupar por Categoria e Calcular Receita e Quantidade**:
    - Agrupe os dados por categoria e some a receita e quantidade vendida.

    ```python
    categoria_analise = dados_vendas.groupby('Categoria').agg({'Receita': 'sum', 'Quantidade': 'sum'}).reset_index()
    ```

3. **Plotar Receita por Produto**:
    - Crie um gráfico de barras mostrando a receita por produto.

    ```python
    plt.figure(figsize=(12, 6))
    sns.barplot(data=produto_analise, x='Produto', y='Receita', palette='viridis')
    plt.title('Receita por Produto')
    plt.xlabel('Produto')
    plt.ylabel('Receita')
    plt.xticks(rotation=45)
    plt.show()
    ```

4. **Plotar Quantidade Vendida por Produto**:
    - Crie um gráfico de barras mostrando a quantidade vendida por produto.

    ```python
    plt.figure(figsize=(12, 6))
    sns.barplot(data=produto_analise, x='Produto', y='Quantidade', palette='viridis')
    plt.title('Quantidade Vendida por Produto')
    plt.xlabel('Produto')
    plt.ylabel('Quantidade Vendida')
    plt.xticks(rotation=45)
    plt.show()
    ```

5. **Plotar Receita por Categoria**:
    - Crie um gráfico de barras mostrando a receita por categoria.

    ```python
    plt.figure(figsize=(12, 6))
    sns.barplot(data=categoria_analise, x='Categoria', y='Receita', palette='viridis')
    plt.title('Receita por Categoria')
    plt.xlabel('Categoria')
    plt.ylabel('Receita')
    plt.xticks(rotation=45)
    plt.show()
    ```

6. **Plotar Quantidade Vendida por Categoria**:
    - Crie um gráfico de barras mostrando a quantidade vendida por categoria.

    ```python
    plt.figure(figsize=(12, 6))
    sns.barplot(data=categoria_analise, x='Categoria', y='Quantidade', palette='viridis')
    plt.title('Quantidade Vendida por Categoria')
    plt.xlabel('Categoria')
    plt.ylabel('Quantidade Vendida')
    plt.xticks(rotation=45)
    plt.show()
    ```

## 6. Estatísticas Descritivas dos Preços e Quantidades Vendidas
**Objetivo**: Obter uma visão geral das distribuições de preços e quantidades vendidas.

**Passos**:
1. **Calcular Estatísticas Descritivas para os Preços**:
    - Use o método `describe` para obter estatísticas como média, mediana, mínimo, máximo, etc.

    ```python
    estatisticas_precos = dados_vendas['Preço'].describe()
    print(estatisticas_precos)
    ```

2. **Calcular Estatísticas Descritivas para as Quantidades Vendidas**:
    - Use o método `describe` para obter estatísticas para a quantidade vendida.

    ```python
    estatisticas_quantidade = dados_vendas['Quantidade'].describe()
    print(estatisticas_quantidade)
    ```

3. **Plotar Distribuição dos Preços**:
    - Crie um histograma mostrando a distribuição dos preços.

    ```python
    plt.figure(figsize=(12, 6))
    sns.histplot(dados_vendas['Preço'], kde=True, bins=30, color='blue')
    plt.title('Distribuição dos Preços')
    plt.xlabel('Preço')
    plt.ylabel('Frequência')
    plt.show()
    ```

4. **Plotar Distribuição das Quantidades Vendidas**:
    - Crie um histograma mostrando a distribuição das quantidades vendidas.

    ```python
    plt.figure(figsize=(12, 6))
    sns.histplot(dados_vendas['Quantidade'], kde=True, bins=30, color='green')
    plt.title('Distribuição das Quantidades Vendidas')
    plt.xlabel('Quantidade Vendida')
    plt.ylabel('Frequência')
    plt.show()
    ```
## 7. Analisar Receita por Região
**Objetivo**: Entender a distribuição geográfica das vendas.

**Passos**:
1. **Agrupar por Região e Somar Receita**:
    - Agrupe os dados por região e some a receita.

    ```python
    regiao_analise = dados_vendas.groupby('Região')['Receita'].sum().reset_index()
    ```

2. **Plotar Receita por Região**:
    - Crie um gráfico de barras mostrando a receita por região.

    ```python
    plt.figure(figsize=(12, 6))
    sns.barplot(data=regiao_analise, x='Região', y='Receita', palette='magma', dodge=False)
    plt.title('Receita por Região')
    plt.xlabel('Região')
    plt.ylabel('Receita')
    plt.legend([],[], frameon=False)
    plt.show()
    ```

## 8. Análise de Produtos Mais Vendidos
**Objetivo**: Identificar os produtos mais vendidos em termos de receita e quantidade.

**Passos**:
1. **Calcular Receita Total e Quantidade Vendida para Cada Produto**:
    - Agrupe os dados por produto e some a receita e quantidade vendida.

    ```python
    top_produtos_receita = dados_vendas.groupby('Produto')['Receita'].sum().reset_index().sort_values(by='Receita', ascending=False).head(10)
    top_produtos_quantidade = dados_vendas.groupby('Produto')['Quantidade'].sum().reset_index().sort_values(by='Quantidade', ascending=False).head(10)
    ```

2. **Plotar Top 10 Produtos por Receita**:
    - Crie um gráfico de barras mostrando os 10 produtos que mais geraram receita.

    ```python
    plt.figure(figsize=(12, 6))
    sns.barplot(data=top_produtos_receita, x='Produto', y='Receita', palette='viridis', dodge=False)
    plt.title('Top 10 Produtos por Receita')
    plt.xlabel('Produto')
    plt.ylabel('Receita')
    plt.xticks(rotation=45)
    plt.legend([],[], frameon=False)
    plt.show()
    ```

3. **Plotar Top 10 Produtos por Quantidade Vendida**:
    - Crie um gráfico de barras mostrando os 10 produtos mais vendidos em termos de quantidade.

    ```python
    plt.figure(figsize=(12, 6))
    sns.barplot(data=top_produtos_quantidade, x='Produto', y='Quantidade', palette='viridis', dodge=False)
    plt.title('Top 10 Produtos por Quantidade Vendida')
    plt.xlabel('Produto')
    plt.ylabel('Quantidade Vendida')
    plt.xticks(rotation=45)
    plt.legend([],[], frameon=False)
    plt.show()
    ```

## 9. Análise de Correlação
**Objetivo**: Identificar a relação entre diferentes variáveis, como preço e quantidade vendida.

**Passos**:
1. **Calcular a Matriz de Correlação**:
    - Use o método `corr` para calcular a correlação entre variáveis numéricas.

    ```python
    correlacao = dados_vendas[['Preço', 'Quantidade', 'Receita']].corr()
    ```

2. **Plotar a Matriz de Correlação**:
    - Crie um mapa de calor para visualizar a matriz de correlação.

    ```python
    plt.figure(figsize=(10, 8))
    sns.heatmap(correlacao, annot=True, cmap='coolwarm', vmin=-1, vmax=1)
    plt.title('Matriz de Correlação')
    plt.show()
    ```

## 10. Verificar a Frequência de Produtos por Região
**Objetivo**: Entender a frequência dos produtos vendidos em cada região.

**Passos**:
1. **Filtrar Dados com Região Preenchida**:
    - Selecione apenas as linhas onde a coluna `Região` não está vazia.

    ```python
    dados_com_regiao = dados_vendas[dados_vendas['Região'].notna()]
    ```

2. **Contar a Frequência de Cada Produto por Região**:
    - Agrupe os dados por produto e região e conte as ocorrências.

    ```python
    frequencia_produto_regiao = dados_com_regiao.groupby(['Produto', 'Região']).size().unstack(fill_value=0)
    ```

3. **Visualizar a Frequência de Produtos por Região**:
    - Visualize a tabela de frequência para verificar os padrões.

    ```python
    print(frequencia_produto_regiao)
    ```

## 11. Inferir Regiões Ausentes com Base na Frequência dos Produtos
**Objetivo**: Preencher valores ausentes na coluna `Região` com base na frequência dos produtos.

**Passos**:
1. **Criar um Dicionário para Mapear Produtos para Suas Regiões Mais Frequentes**:
    - Use a tabela de frequência para criar um dicionário que mapeia cada produto para a região onde ele é mais vendido.

    ```python
    produto_para_regiao_mais_frequente = frequencia_produto_regiao.idxmax(axis=1).to_dict()
    ```

2. **Preencher as Regiões Ausentes**:
    - Use o dicionário para preencher os valores ausentes na coluna `Região`.

    ```python
    dados_vendas['Região'] = dados_vendas.apply(
        lambda row: produto_para_regiao_mais_frequente[row['Produto']] if pd.isna(row['Região']) else row['Região'],
        axis=1
    )
    ```

3. **Verificar as Alterações**:
    - Verifique se as regiões ausentes foram preenchidas corretamente.

    ```python
    print(dados_vendas.head())
    ```

## 12. Por que foi escolhida a inferência das regiões ausentes?
**Objetivo**: Explicar a razão por trás da escolha de inferir as regiões ausentes.

**Passos**:
1. **Mantém a Integridade dos Dados**:
    - Evita a remoção de informações valiosas.

2. **Baseia-se em Padrões Observados**:
    - Torna a inferência mais lógica e precisa.

3. **Reduz o Viés**:
    - Minimiza o viés que pode ser introduzido por métodos de imputação arbitrários.

4. **Facilita Análises Geográficas**:
    - Permite análises mais completas e significativas.

```markdown
A inferência de regiões ausentes com base na frequência dos produtos foi escolhida porque:

- **Mantém a integridade dos dados** ao evitar a remoção de informações valiosas.
- **Baseia-se em padrões observados**, tornando a inferência mais lógica e precisa.
- **Reduz o viés** que pode ser introduzido por métodos de imputação arbitrários.
- **Facilita análises geográficas mais completas e significativas**.

Essa abordagem melhora a qualidade dos dados e, consequentemente, a precisão e a relevância das análises realizadas.
```

## 13. Análise de Desempenho ao Longo do Tempo por Categoria
**Objetivo**: Verificar como o desempenho das categorias varia ao longo do tempo.

**Passos**:
1. **Agrupar por AnoMes e Categoria**:
    - Agrupe os dados por `AnoMes` e `Categoria` e some a receita.

    ```python
    desempenho_categoria = dados_vendas.groupby(['AnoMes', 'Categoria'])['Receita'].sum().unstack().fillna(0)
    ```

2. **Plotar Desempenho ao Longo do Tempo por Categoria**:
    - Crie um gráfico de linha mostrando o desempenho de cada categoria ao longo do tempo.

    ```python
    plt.figure(figsize=(12, 6))
    desempenho_categoria.plot(kind='line')
    plt.title('Desempenho ao Longo do Tempo por Categoria')
    plt.xlabel('Ano-Mês')
    plt.ylabel('Receita')
    plt.xticks(rotation=45)
    plt.show()
    ```

## 14. Análise de Concentração de Receita
**Objetivo**: Verificar quais produtos ou categorias geram a maior parte da receita.

**Passos**:
1. **Calcular Receita Total por Produto**:
    - Agrupe os dados por produto e some a receita.

    ```python
    receita_produto_total = dados_vendas.groupby('Produto')['Receita'].sum().reset_index().sort_values(by='Receita', ascending=False)
    ```

2. **Plotar Concentração de Receita**:
    - Crie um gráfico de barras mostrando a receita total por produto.

    ```python
    plt.figure(figsize=(12, 6))
    sns.barplot(data=receita_produto_total, x='Produto', y='Receita', palette='viridis', dodge=False)
    plt.title('Concentração de Receita por Produto')
    plt.xlabel('Produto')
    plt.ylabel('Receita')
    plt.legend([],[], frameon=False)
    plt.show()
    ```

## 15. Distribuição de Vendas por Intervalo de Preço
**Objetivo**: Entender como as vendas estão distribuídas por diferentes faixas de preço.

**Passos**:
1. **Criar Faixas de Preço**:
    - Crie uma nova coluna com faixas de preço.

    ```python
    dados_vendas['Faixa de Preço'] = pd.cut(dados_vendas['Preço'], bins=[0, 100, 200, 300, 400, 500], labels=['0-100', '101-200', '201-300', '301-400', '401-500'])
    ```

2. **Agrupar por Faixa de Preço e Somar Receita e Quantidade**:
    - Agrupe os dados por faixa de preço e some a receita e a quantidade vendida.

    ```python
    analise_preco = dados_vendas.groupby('Faixa de Preço').agg({'Receita': 'sum', 'Quantidade': 'sum'}).reset_index()
    ```

3. **Plotar Receita por Faixa de Preço**:
    - Crie um gráfico de barras mostrando a receita por faixa de preço.

    ```python
    plt.figure(figsize=(12, 6))
    sns.barplot(data=analise_preco, x='Faixa de Preço', y='Receita', palette='coolwarm', dodge=False)
    plt.title('Receita por Faixa de Preço')
    plt.xlabel('Faixa de Preço')
    plt.ylabel('Receita')
    plt.legend([],[], frameon=False)
    plt.show()
    ```

4. **Plotar Quantidade Vendida por Faixa de Preço**:
    - Crie um gráfico de barras mostrando a quantidade vendida por faixa de preço.

    ```python
    plt.figure(figsize=(12, 6))
    sns.barplot(data=analise_preco, x='Faixa de Preço', y='Quantidade', palette='coolwarm', dodge=False)
    plt.title('Quantidade Vendida por Faixa de Preço')
    plt.xlabel('Faixa de Preço')
    plt.ylabel('Quantidade Vendida')
    plt.legend([],[], frameon=False)
    plt.show()
    ```

## 16. Resumo das Principais Descobertas
**Objetivo**: Resumir os insights mais importantes da análise de vendas.

**Passos**:
1. **Tendências de Vendas ao Longo do Tempo**:
    - Mostre a tabela de receita mensal.

    ```python
    print("### Tendências de Vendas ao Longo do Tempo")
    print(receita_mensal.to_string(index=False))
    ```

2. **Receita e Quantidade Vendida por Produto**:
    - Mostre a tabela de receita e quantidade vendida por produto.

    ```python
    print("\n### Receita e Quantidade Vendida por Produto")
    print(produto_analise.to_string(index=False))
    ```

3. **Receita e Quantidade Vendida por Categoria**:
    - Mostre a tabela de receita e quantidade vendida por categoria.

    ```python
    print("\n### Receita e Quantidade Vendida por Categoria")
    print(categoria_analise.to_string(index=False))
    ```

4. **Estatísticas Descritivas dos Preços**:
    - Mostre a tabela de estatísticas descritivas para os preços.

    ```python
    print("\n### Estatísticas Descritivas dos Preços")
    print(estatisticas_precos.to_string())
    ```

5. **Estatísticas Descritivas das Quantidades Vendidas**:
    - Mostre a tabela de estatísticas descritivas para as quantidades vendidas.

    ```python
    print("\n### Estatísticas Descritivas das Quantidades Vendidas")
    print(estatisticas_quantidade.to_string())
    ```

6. **Receita por Região (Após Inferência de Regiões Ausentes)**:
    - Mostre a tabela de receita por região após a inferência.

    ```python
    print("\n### Receita por Região (Após Inferência de Regiões Ausentes)")
    print(regiao_analise_inferida.to_string(index=False))
    ```

## 17. Desempenho por Produto e Categoria
**Objetivo**: Fornecer uma visão clara e detalhada dos itens mais importantes para a tomada de decisões.

**Passos**:
1. **Criar Tabela de Desempenho por Produto e Categoria**:
    - Crie uma tabela detalhada de desempenho por produto e categoria.

    ```markdown
    |  Produto  |  Receita Total  | Quantidade Vendida |
    |-----------|-----------------|--------------------|
    | Produto A | \$23,851,095.28 | 90,486             |
    | Produto B | \$25,401,339.21 | 95,716             |
    | Produto C | \$25,021,212.27 | 95,058             |
    | Produto D | \$25,725,838.54 | 96,505             |
    | Produto E | \$25,699,229.50 | 97,297             |
    ```

## 18. Exportação de Resultados
**Objetivo**: Exportar os dados analisados para um novo arquivo CSV para facilitar o compartilhamento e futuras análises.

**Passos**:
1. **Exportar Dados Analisados**:
    - Use `pandas` para exportar os dados analisados para um arquivo CSV.

    ```python
    dados_vendas.to_csv('dados_vendas_analisados.csv', index=False)
    ```
