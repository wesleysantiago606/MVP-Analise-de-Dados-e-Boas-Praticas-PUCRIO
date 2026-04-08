# MVP – Análise Exploratória e Pré-processamento de Dados
## Risco de Crédito – Estimação de Probabilidade de Default (PD)

Projeto desenvolvido como MVP da Pós-Graduação em Ciência de Dados e Analytics da PUC-Rio, contemplando as disciplinas de Análise Exploratória e Pré-processamento de Dados, Visualização da Informação e Engenharia de Software para Ciência de Dados.

**Autor:** Wesley Ramos Neres Santiago
**Matricula:** 4052025002507

---

## Objetivo

Explorar e preparar um dataset de crédito ao consumidor pessoa física para um problema de classificação binária: prever se um tomador irá ou não entrar em **default** (inadimplência), com base em seus atributos cadastrais e características do empréstimo solicitado.

---

## Dataset

- **Nome:** Credit Risk Dataset
- **Fonte:** [Kaggle — laotse/credit-risk-dataset](https://www.kaggle.com/datasets/laotse/credit-risk-dataset)
- **Instâncias:** 32.581
- **Atributos:** 12 (11 preditores + 1 variável-alvo: `loan_status`)

| Atributo | Tipo | Descrição |
|----------|------|-----------|
| `person_age` | Numérico | Idade do solicitante em anos |
| `person_income` | Numérico | Renda anual declarada (USD) |
| `person_home_ownership` | Categórico | Situação de moradia: RENT, OWN, MORTGAGE, OTHER |
| `person_emp_length` | Numérico | Tempo de emprego em anos |
| `loan_intent` | Categórico | Finalidade do empréstimo |
| `loan_grade` | Ordinal | Classificação de risco: A (menor) → G (maior) |
| `loan_amnt` | Numérico | Valor do empréstimo solicitado (USD) |
| `loan_int_rate` | Numérico | Taxa de juros anual (%) |
| `loan_status` | Binário | **Variável-alvo**: 0 = adimplente, 1 = inadimplente |
| `loan_percent_income` | Numérico | Proporção do empréstimo sobre a renda (DTI) |
| `cb_person_default_on_file` | Binário | Histórico de default anterior: Y/N |
| `cb_person_cred_hist_length` | Numérico | Tempo de histórico de crédito em anos |

---

## Estrutura do Repositório

```
├── MVP_Analise_Dados_Risco_Credito_Wesley_Santiago.ipynb 
└── README.md                   
```

> O dataset é carregado automaticamente via `kagglehub` — não é necessário baixar o arquivo manualmente.

---

## Etapas Realizadas no Notebook

1. **Definição do Problema** — descrição, tipo de aprendizado, hipóteses, premissas e definição dos atributos
2. **Análise de Dados**
   - Visão geral (shape, tipos, primeiras linhas)
   - Verificação de valores faltantes
   - Resumo estatístico completo (mín, máx, média, mediana, desvio-padrão, skewness, kurtosis)
   - Visualizações: histogramas, boxplots por classe e matriz de correlação
3. **Tratamento de Valores Nulos**
   - Imputação pela mediana em `loan_int_rate` e `person_emp_length`
4. **Pré-processamento**
   - Remoção de outliers impossíveis (`person_age` > 90 e `person_emp_length` > 60)
   - Transformação logarítmica em `person_income`
   - Encoding ordinal em `loan_grade`
   - One-Hot Encoding nas variáveis categóricas nominais
   - Normalização Min-Max
   - Padronização Z-score
5. **Análise Pós-processamento** — verificação do impacto das transformações
6. **Respondendo as Hipóteses** — validação das 6 hipóteses levantadas com gráficos e dados reais
7. **Conclusão** — síntese dos achados e recomendações para modelagem

---

## Como Executar

1. Acesse o notebook no Google Colab
2. Execute todas as células em ordem: `Runtime → Run all`
3. Na célula de carga de dados, o `kagglehub` irá baixar o dataset automaticamente — nenhum arquivo local é necessário
4. Nenhuma instalação adicional é necessária — todas as bibliotecas estão disponíveis por padrão no Colab

---

## Bibliotecas Utilizadas

| Biblioteca | Versão recomendada | Uso |
|------------|--------------------|-----|
| `pandas` | ≥ 1.5 | Manipulação de dados |
| `numpy` | ≥ 1.23 | Operações numéricas |
| `matplotlib` | ≥ 3.6 | Visualizações |
| `seaborn` | ≥ 0.12 | Visualizações estatísticas |
| `scipy` | ≥ 1.9 | Estatísticas descritivas (skewness, kurtosis) |
| `scikit-learn` | ≥ 1.1 | Pré-processamento (MinMaxScaler, StandardScaler, LabelEncoder) |
| `kagglehub` | ≥ 0.1 | Download automático do dataset |

---

## Resultados Principais

- Dois atributos com valores faltantes tratados: `loan_int_rate` (~9,6%) e `person_emp_length` (~2,7%)
- Outliers impossíveis removidos em `person_age` e `person_emp_length`
- Atributos com maior correlação com o target: `loan_int_rate` (r = +0,34) e `loan_percent_income` (r = +0,38)
- Desbalanceamento de classes confirmado: 78,2% adimplentes vs 21,8% inadimplentes (ratio 3,6:1)
- Três versões do dataset geradas: escala original, normalizado (Min-Max) e padronizado (Z-score)

### Hipóteses Validadas

| Hipótese | Resultado | Destaque |
|----------|-----------|----------|
| H1 — DTI prediz default | ✅ Confirmada | Salto crítico na faixa de 30% de DTI — taxa passa de 22% para 68,7% |
| H2 — Histórico anterior prediz default | ✅ Confirmada | Com histórico: 37,8% vs sem histórico: 18,4% |
| H3 — Default cresce grade A → G | ✅ Confirmada | `loan_grade` G atinge 98,4% de default |
| H4 — Finalidade como proxy de estresse | ⚠️ Parcial | DEBTCONSOLIDATION lidera (28,6%), mas diferença entre categorias é moderada |
| H5 — Jovens têm maior risco | ⚠️ Parcial | Confirmada para 18-25 anos (23%), mas faixa 56-65 surpreende com 26% |
| H6 — Desbalanceamento de classes | ✅ Confirmada | Ratio 3,6:1 — requer tratamento antes da modelagem |

---

## Contexto Acadêmico

Este trabalho aborda o problema de estimação da **Probabilidade de Default (PD)**, um dos três componentes do modelo IRB (*Internal Ratings-Based*) dos Acordos de Basileia II/III, ao lado da LGD (*Loss Given Default*) e da EAD (*Exposure at Default*). O escopo deste MVP é restrito à etapa de análise exploratória e pré-processamento — a modelagem preditiva constitui etapa futura.
