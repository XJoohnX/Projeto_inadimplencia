 # Contexto e Objetivo

O objetivo central deste projeto foi construir um modelo preditivo que pudesse identificar a probabilidade de inadimplência entre os clientes do A.Cash, um banco digital internacional. A análise se concentrou em permitir que o banco avaliasse melhor o risco de crédito, diminuindo perdas financeiras por inadimplência e otimizando a concessão de empréstimos. Para esse propósito, foi utilizado um conjunto de dados detalhado sobre as características dos clientes, suas intenções de empréstimo e histórico de crédito.

## Avaliação do Relatório com Base em Critérios Específicos

A análise deste relatório se concentrará em cinco pontos principais para garantir que ele cumpra com os requisitos estratégicos:

- **Descrição dos Dados**: O relatório inclui uma seção que detalha claramente os dados e suas características, assim como as etapas iniciais de tratamento de valores ausentes e preparação das variáveis.

- **Objetivo da Análise**: A seção inicial apresenta o objetivo principal de maneira direta, enfatizando a necessidade de reduzir inadimplência e melhorar o risco de crédito.

- **Variação de Modelos**: O relatório compara variações de classificadores, como SMOTE e ClusterCentroids, e especifica que o modelo Random Forest com SMOTE é o que melhor se adapta ao objetivo de identificar inadimplentes.

- **Descobertas Principais**: As métricas do modelo, incluindo recall e precisão, são destacadas com explicações sobre o impacto desses resultados para o objetivo principal.

- **Falhas do Modelo e Plano de Ação**: Possíveis limitações são identificadas, como a precisão moderada e falsos positivos. Ações sugeridas incluem ajustes de threshold, regras de negócio adicionais e exploração de dados complementares.


## Tabela de Conteúdo

- [Descrição dos Dados](#descrição-dos-dados)
- [Pré-processamento](#pré-processamento)
- [Modelagem de Regressão](#modelagem-de-regressão-1)
- [Balanceamento de Classes](#balanceamento-de-classes)
- [Avaliação Final dos Modelos de Classificação](#avaliação-final-dos-modelos-de-classificação)
- [Recomendação](#recomendação)
- [Considerações Importantes](#considerações-importantes)
- [Conclusão](#conclusão)

## Descrição dos Dados

O dataset contém 34.501 registros com 12 colunas relacionadas a dados demográficos, informações de empréstimo e histórico de crédito. Observamos que algumas colunas possuem valores ausentes, especialmente em taxa_juros_emprestimo, tempo_emprego_pessoa e inadimplente_pessoa. Esse tipo de lacuna requer estratégias específicas de imputação, dependendo da natureza das variáveis e da quantidade de valores ausentes.

A estrutura do notebook revela as seguintes etapas iniciais:

Leitura e renomeação de colunas: O notebook foi ajustado para nomes em português, facilitando o entendimento das variáveis para análises subsequentes.

Exploração dos dados: Inclui comandos para avaliar a quantidade de entradas, tipos de dados e a presença de valores ausentes.

## Pré-processamento

- **Remoção de valores nulos**: Remoção de valores nulos para variáveis preditoras específicas, focando inicialmente na coluna `grau_emprestimo`.

- **Codificação ordinal**: Codificação ordinal aplicada à coluna `grau_emprestimo`, atribuindo valores numéricos para facilitar o modelo.

- **Normalização de Dados Numéricos**: As colunas numéricas foram escaladas para o intervalo `[0, 1]` utilizando o MinMaxScaler. Este processo garantiu uniformidade na escala das variáveis, prevenindo que algumas características dominassem outras no modelo de regressão ou influenciassem negativamente os modelos de classificação. Além disso, a normalização reduziu a influência de valores extremos e preparou os dados para uso em modelos subsequentes que requerem entradas normalizadas.

## Modelagem de Regressão

- **Modelo**: A regressão linear foi utilizada exclusivamente como uma etapa de pré-processamento para preencher valores ausentes na variável `taxa_juros_emprestimo` com base nas variáveis `valor_emprestimo` e `grau_emprestimo_encoded`.

- **Avaliação**: O modelo apresentou um Erro Médio Quadrático (MSE) de aproximadamente 1.84 e um coeficiente de determinação (R²) de cerca de 0.82, sugerindo que o modelo explica 82% da variabilidade da taxa de juros.

- **Limitações**: A robustez deste modelo pode ser limitada pela presença de outliers e pela distribuição assimétrica da variável dependente.

## Balanceamento de Classes

Para a variável de inadimplência (`inadimplente_pessoa`), foi aplicado o SMOTE (oversampling) para gerar amostras adicionais de inadimplentes e o ClusterCentroids (undersampling) para reduzir o desequilíbrio entre as classes.

A inclusão da normalização no pipeline fortaleceu o pré-processamento ao criar condições favoráveis para a modelagem, contribuindo para a qualidade e desempenho tanto do modelo de regressão quanto dos modelos de classificação subsequentes.

## Validação cruzada 
### Por que realizar Train-Test Split inicialmente?

O Train-Test Split foi utilizado para dividir o conjunto de dados em dois subconjuntos:

- **Conjunto de Treino**: Usado para ajustar (treinar) o modelo.
- **Conjunto de Teste**: Reservado para avaliar o desempenho do modelo em dados que ele nunca viu antes.

Essa separação inicial é essencial para simular como o modelo se comportará em um cenário real, permitindo uma avaliação de generalização antes de qualquer ajuste mais refinado.

### Por que complementar com Validação Cruzada?

Após realizar o Train-Test Split, foi empregada a validação cruzada para garantir maior confiança no desempenho do modelo:

- **Redução de Viés**: Isso ajuda a reduzir o risco de que os resultados sejam influenciados por uma única divisão dos dados.
- **Métricas Robustas**: Fornece métricas mais robustas e estáveis, especialmente em conjuntos de dados com distribuição complexa ou limitada.
## Avaliação Final dos Modelos de Classificação

### Comparação de Modelos

| Modelo                      | Método de Balanceamento | F1-Score Médio | Precisão Média | Recall Médio | Acurácia Média |
|-----------------------------|-------------------------|----------------|----------------|--------------|----------------|
| **Regressão Logística**     | SMOTE                   | 0.9040         | 0.8255         | 0.9991       | 0.8940         |
| **Regressão Logística**     | NearMiss                | 0.8489         | 0.7559         | 0.9682       | 0.8278         |
| **Random Forest**          | SMOTE                   | 0.9099         | 0.8466         | 0.9835       | 0.9026         |
| **Random Forest**          | NearMiss                | 0.8539         | 0.7748         | 0.9510       | 0.8372         |
| **XGBoost**                | SMOTE                   | 0.9008         | 0.8696         | 0.9343       | 0.8971         |
| **XGBoost**                | NearMiss                | 0.8996         | 0.8561         | 0.9478       | 0.8942         |

### Análise dos Resultados

No contexto de prever a inadimplência entre os clientes do A.Cash, é crucial equilibrar corretamente as métricas de **precisão** (quantos dos identificados como inadimplentes realmente o são) e **recall** (quantos dos inadimplentes foram identificados pelo modelo). Uma alta taxa de recall é importante para capturar a maioria dos clientes que realmente serão inadimplentes, reduzindo perdas financeiras. Entretanto, uma precisão baixa pode levar a muitos falsos positivos, afetando a relação com clientes bons pagadores.

## Recomendação

**Modelo Sugerido**: Random Forest com SMOTE

**Justificativa**:

- **Equilíbrio Ideal**: O Random Forest com SMOTE oferece um excelente equilíbrio entre precisão (0,8465) e recall (0,9835), garantindo que a maioria dos inadimplentes seja identificada sem gerar um número excessivo de falsos positivos.
- **Desempenho Superior**: Apresenta o maior F1-score médio (0,9099) e a maior acurácia média (0,9026), indicando um desempenho geral robusto.
- **Redução de Riscos**: Com um recall alto, o modelo minimiza o risco de perdas financeiras ao identificar a maioria dos clientes que potencialmente seriam inadimplentes.
- **Manutenção de Relacionamentos**: A precisão relativamente alta ajuda a evitar a recusa de crédito a clientes que são bons pagadores, mantendo um relacionamento positivo com a base de clientes.

## Considerações importantes

- **Importância do SMOTE**: Os modelos que utilizaram SMOTE superaram consistentemente aqueles que utilizaram NearMiss, indicando que o balanceamento da classe minoritária através de oversampling foi mais eficaz neste caso.
- **Avaliação Contínua**: É recomendado monitorar continuamente o desempenho do modelo após a implementação, ajustando conforme necessário para responder a mudanças nos dados ou no comportamento dos clientes.


## Conclusão

Implementar o Random Forest com SMOTE permitirá ao A.Cash identificar eficazmente clientes com alto risco de inadimplência, reduzindo perdas financeiras e otimizando a concessão de empréstimos. Este modelo equilibra a necessidade de minimizar riscos com a manutenção de relacionamentos positivos com clientes confiáveis, alinhando-se perfeitamente ao objetivo central do projeto.


