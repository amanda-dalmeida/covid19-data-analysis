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