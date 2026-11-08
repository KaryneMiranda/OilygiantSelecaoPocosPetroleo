# Seleção de Região para Novos Poços de Petróleo – OilyGiant

Projeto da Sprint 10 em que uso Machine Learning e bootstrapping para ajudar a mineradora OilyGiant a escolher, entre três regiões candidatas, a melhor para abrir 200 novos poços de petróleo.

## Objetivo

Construir um modelo de regressão linear que preveja o volume de reservas de novos poços e, a partir dessas previsões, escolher a região com o **maior lucro esperado** e **risco de prejuízo abaixo de 2,5%**.

## Condições do negócio

- **Orçamento:** US$ 100 milhões para 200 poços
- **Exploração:** em cada região, 500 pontos são estudados e os 200 melhores são escolhidos
- **Receita:** US$ 4.500 por unidade de produto (mil barris)
- **Critério:** só podem ser escolhidas regiões com risco de prejuízo menor que 2,5%

## Dados

| Arquivo | Conteúdo |
|---|---|
| `geo_data_0.csv` | Dados geológicos da Região 0 |
| `geo_data_1.csv` | Dados geológicos da Região 1 |
| `geo_data_2.csv` | Dados geológicos da Região 2 |

Cada arquivo tem 100.000 poços, com três características geológicas (`f0`, `f1` e `f2`) e o volume de reservas (`product`, em milhares de barris).

## Etapas

1. **Preparação dos dados:** verificação de nulos e duplicados, remoção de IDs repetidos e da coluna `id`
2. **Treinamento do modelo:** regressão linear para cada região, com 75% dos dados para treino e 25% para validação
3. **Ponto de equilíbrio:** cálculo do volume mínimo por poço para não ter prejuízo
4. **Função de lucro:** seleção dos 200 poços com maior volume previsto e cálculo do lucro com os volumes reais
5. **Bootstrapping:** 1.000 amostras de 500 poços por região para estimar o lucro médio, o intervalo de confiança de 95% e o risco de prejuízo

## Resultados

### Desempenho do modelo

| Região | Volume médio previsto | REQM |
|---|---|---|
| Região 0 | 92,79 | 37,85 |
| Região 1 | 69,18 | **0,89** |
| Região 2 | 94,87 | 40,08 |

A Região 1 tem o menor volume médio, mas o modelo prevê os poços dela quase perfeitamente. Nas Regiões 0 e 2, o volume médio é maior, mas as previsões são bem menos precisas.

### Ponto de equilíbrio

Cada poço precisa ter pelo menos **111,11 mil barris** para cobrir o investimento. Nenhuma região atinge esse valor na média, por isso escolher bem os 200 melhores poços é essencial.

### Risco e lucro (bootstrapping)

| Região | Lucro médio | IC 95% | Risco de prejuízo |
|---|---|---|---|
| Região 0 | US$ 5,86 mi | -US$ 0,06 mi a US$ 12,79 mi | 2,8% |
| **Região 1** | **US$ 6,82 mi** | **US$ 1,86 mi a US$ 12,48 mi** | **0,3%** |
| Região 2 | US$ 5,38 mi | -US$ 0,70 mi a US$ 11,68 mi | 4,1% |

## Conclusão

A **Região 1** é a recomendada. É a única com risco de prejuízo abaixo de 2,5% e também tem o maior lucro médio. Como o modelo é muito preciso nessa região, ele consegue identificar os melhores poços com mais segurança, mesmo que o volume médio de reservas seja menor.

## Tecnologias

- Python
- Pandas
- NumPy
- Scikit-learn

## Como executar

1. Clone o repositório
2. Instale as dependências:
   ```bash
   pip install pandas numpy scikit-learn jupyter
   ```
3. Abra o notebook:
   ```bash
   jupyter notebook projetosprint10.ipynb
   ```

> Observação: os dados são carregados diretamente de URLs públicas, então não é preciso baixar os arquivos.
