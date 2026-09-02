# 📊 Análise Exploratória de Dados da COVID-19: Auditoria, Limpeza e Geração de Insights

Mesmo com o avanço no controle epidemiológico nos últimos anos, a análise histórica dos dados da COVID-19 continua sendo fundamental para entender a dinâmica de disseminação do vírus, a eficácia das respostas globais e a disparidade nos impactos entre diferentes países e continentes.

---

## 📌 Introdução

Em 2026, seis anos após a eclosão inicial do SARS-CoV-2, a COVID-19 deixou de ser tratada como uma emergência sanitária global aguda e passou a ser gerenciada como uma condição endêmica contínua. No entanto, suas consequências estruturais no sistema de saúde, na economia global e no comportamento da sociedade continuam profundas e duradouras.

---

## 🎯 Objetivos do Projeto

O objetivo principal deste projeto é auditá, tratar e analisar um conjunto de dados globais sobre a COVID-19 extraído do [Kaggle](https://www.kaggle.com/datasets/athangpatil/covid19-world-statistics). 

A análise busca garantir a qualidade e a consistência das informações para responder com precisão às seguintes perguntas centrais:

1. **Qual continente registrou o maior volume total de casos?**
2. **Qual país teve a maior proporção de casos em relação à sua população?**
3. **Qual país apresentou a maior taxa de mortalidade?**

---

## 🔍 Diagnóstico e Auditoria Inicial dos Dados (EDA - Fase 1)

Durante a fase inicial de inspeção e auditoria dos dados (`df.info()`, `df.describe()` e verificações de integridade), foram identificadas as seguintes características e inconsistências na base brutos de 237 registros:

### 1. Estrutura e Registros Especiais
* **Totais Agregados na Coluna de Países:** A base contém linhas onde a coluna `country` armazena agregados continentais ou globais (ex: *"North-America"*, *"Asia"*, *"Europe"*, *"All"*). Essas linhas precisam ser removidas da análise individual por país para não duplicar contagias nas métricas globais.
* **Casos Especiais/Embarcações:** Registros como *"Diamond-Princess"* não representam territórios com população fixa e possuem continente ausente (`NaN`).

### 2. Qualidade e Valores Ausentes (`NaN`)
* **Criticos e Recuperados:** A coluna `cases.critical` possui o maior índice de dados ausentes (178 nulos de 237), seguida por `cases.active` (47 nulos) e `cases.recovered` (48 nulos).
* **População e Testes:** `population(k)` possui 8 valores ausentes e `test.total (k)` possui 24.
* **Continente:** Apenas 1 registro com continente ausente (*Diamond-Princess*).

### 3. Validação Regras de Negócio
* **Integridade Numérica:** Não foram encontrados valores negativos de casos, mortes ou população.
* **Consistência Lógica:** Os testes de validação confirmaram que em 100% dos registros:
  * Mortes totais não superam os casos totais (`deaths.total <= cases.total`).
  * Casos recuperados, ativos e críticos não superam os casos totais.

---

## 🧹 Tratamento e Limpeza dos Dados

Com base na auditoria inicial, as seguintes etapas de sanitização foram aplicadas:

* **Remoção de Agregados:** Exclusão de linhas onde a coluna `country` continha nomes de continentes ou o valor `"All"`, garantindo que apenas países reais entrassem nas análises territoriais.
* **Ajuste de Unidades:** Conversão da coluna `population(k)` para a população total (multiplicação por 1.000) para evitar distorções no cálculo *per capita*.
* **Tratamento de Nulos:** Tratamento direcionado nas métricas de óbitos e casos ativos/recuperados para evitar divergências em taxas calculadas.

## 📐 Metodologia

O projeto foi conduzido seguindo uma adaptação da metodologia **CRISP-DM** para análise exploratória de dados epidemiológicos:

1. **Entendimento do Problema:** Definição das perguntas centrais da análise e identificação das métricas necessárias para respondê-las.
2. **Coleta e Auditoria:** Carga do dataset proveniente do Kaggle e verificação da estrutura dos dados, tipos, valores ausentes, duplicatas e regras de consistência lógica.
3. **Tratamento dos Dados (Saneamento):** Investigação e tratamento de inconsistências, exclusão de registros que não representavam países ou que apresentavam informações incompatíveis com os objetivos da análise, além do tratamento dos valores ausentes.
4. **Validação Final:** Verificação da consistência e integridade do dataset após as etapas de limpeza, resultando na criação do `covid19_clean.csv`.
5. **Análise Descritiva Inicial:** Avaliação do comportamento estatístico das principais variáveis e identificação de padrões relevantes para orientar a análise dos objetivos.
6. **Análise Exploratória (EDA) & Visualização:** Etapa a ser desenvolvida para o cálculo dos indicadores, comparação dos resultados e construção de visualizações que respondam às perguntas centrais.
7. **Consolidação dos Resultados:** Documentação e síntese dos principais insights no README.
## 📋 Etapas do Projeto
### 1. 🎯 Entendimento do Problema

O projeto teve início com a definição de três perguntas centrais para orientar a análise dos dados da COVID-19:

- 🌍 **Volume Geral:** Qual continente teve mais casos?
- 👥 **Comparação Proporcional:** Qual país teve mais casos proporcionalmente à população?
- ⚠️ **Mortalidade:** Qual país apresentou a maior taxa de mortalidade?

A partir dessas perguntas, foram identificadas as principais variáveis necessárias para a análise, como número total de casos, mortes, população e continente. Também foi definido que a comparação entre países deveria considerar tanto valores absolutos quanto métricas proporcionais à população.

### 2. 🔎 Coleta e Auditoria

Foi utilizado um dataset de estatísticas da COVID-19 disponibilizado pelo **Kaggle**, contendo informações sobre casos, mortes, população, testes e localização geográfica.

Inicialmente, foi realizada uma auditoria da estrutura e qualidade dos dados, incluindo:

- Identificação das dimensões do dataset;
- Verificação dos nomes e tipos das variáveis;
- Análise da distribuição dos registros por país e continente;
- Identificação de valores ausentes;
- Investigação de possíveis registros duplicados ou inconsistentes;
- Verificação da coerência entre país e continente;
- Identificação de registros que não representavam países, como `Diamond-Princess`;
- Análise de registros agregados, como `All`;
- Investigação de valores potencialmente incorretos ou incompatíveis com a realidade.

Essa auditoria permitiu identificar problemas de qualidade que precisavam ser investigados antes da análise dos indicadores.

### 3. 📝 Análise Descritiva Inicial 
A análise descritiva inicial revelou uma grande heterogeneidade entre os países analisados, principalmente em relação à população, ao número de casos e às mortes registradas:

* **Assimetria das Variáveis:** As colunas `cases.total` e `deaths.total` apresentaram distribuições fortemente assimétricas, com médias significativamente superiores às medianas. Esse comportamento evidencia a influência de países com valores extremamente elevados.

* **Necessidade de Comparações Proporcionais:** A grande variação populacional observada no dataset reforça que comparações entre países não devem considerar apenas os números absolutos. Por esse motivo, será calculada uma métrica que relacione o número de casos à população de cada país.

* **Distribuição Geográfica Inicial:** Na agregação preliminar por continente, a **Europa** apresentou o maior volume absoluto de casos, seguida pela **Ásia** e pela **América do Norte**.

Essas constatações orientaram as etapas de preparação dos dados e a definição das métricas que serão utilizadas para responder às três perguntas centrais da análise.
## 🛠️ Tecnologias e Ferramentas

* **Linguagem:** Python 3.12.4
* **Manipulação de Dados:** Pandas & NumPy
* **Ambiente de Desenvolvimento:** Jupyter Notebooks & VS Code
* **Controle de Versão:** Git & GitHub

---

## 📂 Estrutura do Repositório

```text
├── data/
│   └── covid19_statistics       # Base de dados bruta
├── notebooks/
│   └── covid19_analysis.ipynb   # Notebook de auditoria, limpeza e EDA
├── README.md                    # Documentação do projeto
└── requirements.txt             # Dependências do projeto