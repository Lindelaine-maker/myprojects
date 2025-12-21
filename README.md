# 🚀 i4DATA - Plataforma de Inteligência Preditiva e Análise Regulatória

![Python](https://img.shields.io/badge/Python-3.11+-blue?logo=python) ![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-yellowgreen) ![Prophet](https://img.shields.io/badge/Prophet-Forecasting-orange) ![Streamlit](https://img.shields.io/badge/Streamlit-Dashboard-red?logo=streamlit) ![Plotly](https://img.shields.io/badge/Plotly-Visualization-blueviolet)

## 🎯 Sobre o Projeto

**i4DATA** é uma plataforma RegTech modular que correlaciona dados de mercado financeiro (ex: ações da B3) com dados setoriais (ex: geração de energia no Ceará) para gerar **insights preditivos** e **análises de risco regulatório**.

O projeto desenvolve um **MVP (Minimum Viable Product)** funcional que:

-   📊 Processa dados públicos de múltiplas fontes (ANEEL, B3, Banco Central)
-   🔮 Aplica modelos preditivos (Prophet e ARIMA) para séries temporais
-   ⚠️ Calcula indicadores de risco regulatório
-   📈 Apresenta tudo em um painel visual interativo

## 🗃️ Quadro Kanban

https://github.com/orgs/capacitabrasil/projects/25

### 🎓 Objetivos Didáticos

1. **Análise Exploratória de Dados (EDA)** com base fictícia inicial
2. **Séries Temporais e Previsão** - Criação de modelos preditivos
3. **Dashboard Interativo** - Visualização e análise de dados
4. **Aplicação em Dados Reais** - Validação e insights práticos

---

## 📁 Estrutura do Repositório

```text
i4data_mvp/
│
├── dados/
│   ├── raw/              # CSVs originais baixados (ANEEL, B3, BCB)
│   └── processed/        # Dados limpos e tratados
│
├── codigos/
│   ├── etl.py           # Script de limpeza e transformação
│   ├── prophet_model.py # Modelo de previsão com Prophet
│   ├── arima_model.py   # Modelo ARIMA alternativo
│   └── risk_score.py    # Cálculo de índice de risco regulatório
│
├── dashboard/
│   └── app.py           # Painel interativo em Streamlit/Dash
│
├── documentos/
│   ├── Guide do Projeto.pdf
│   ├── Escopo dos Projetos V. A. Barbosa.pdf
│   └── README.md
│
├── outputs/             # Gráficos e relatórios gerados
├── notebooks/           # Notebooks de análise exploratória
├── requirements.txt     # Dependências do projeto
└── README.md
```

---

## 🛠️ Tecnologias e Bibliotecas

-   **Python 3.11+** 🐍
    -   `pandas`, `numpy` — Manipulação e análise de dados
    -   `prophet`, `statsmodels` — Modelos de previsão (Prophet e ARIMA)
    -   `matplotlib`, `seaborn`, `plotly` — Visualizações
    -   `streamlit` ou `dash` — Dashboards interativos
    -   `yfinance` — Dados financeiros da B3
    -   `requests` — API do Banco Central

**Recomendação:** Use ambiente virtual (venv/conda) e fixe dependências em `requirements.txt`

```bash
# Criar ambiente virtual
python -m venv .venv

# Ativar (Windows PowerShell)
.\.venv\Scripts\activate

# Instalar dependências
pip install -r requirements.txt
```

---

## 📊 Fontes de Dados

| #   | Fonte              | Descrição                                   | Link                                                             |
| --- | ------------------ | ------------------------------------------- | ---------------------------------------------------------------- |
| 1   | **B3 (Ações)**     | Séries históricas de ações (via `yfinance`) | `yf.download("EGIE3.SA")`                                        |
| 2   | **ANEEL - BIG**    | Dados de geração de energia elétrica        | [Portal ANEEL](https://dadosabertos.aneel.gov.br/dataset)        |
| 3   | **Banco Central**  | Indicadores macroeconômicos (IPCA, Selic)   | [API BCB SGS](https://api.bcb.gov.br)                            |
| 4   | **Porto do Pecém** | Movimentação portuária (CE)                 | [Transparência](https://www.portodopecem.com.br/transparencia)   |
| 5   | **OFAC**           | Lista de sanções internacionais             | [Treasury OFAC](https://www.treasury.gov/ofac/downloads/sdn.csv) |

---

## 🔁 Pipeline de Desenvolvimento (10 Semanas)

| Semana  | Foco Principal          | Entregas                                                      |
| :-----: | ----------------------- | ------------------------------------------------------------- |
|  **1**  | Organização e Estrutura | Criar repo GitHub, estrutura de pastas, baixar 3 datasets     |
|  **2**  | Limpeza de Dados (ETL)  | Padronizar datas, colunas e salvar em `processed/`            |
| **3-4** | Modelagem Preditiva     | Aplicar Prophet/ARIMA, gerar previsões, salvar `forecast.csv` |
|  **5**  | Cálculo de Risco        | Criar `risk_score.py` com métricas de risco regulatório       |
| **6-7** | Dashboard Interativo    | Criar `app.py` com gráficos e tabelas interativas             |
|  **8**  | UX e Estética           | Melhorar visual, cores, logotipo, responsividade              |
|  **9**  | Documentação            | Relatório técnico, README completo, preparar pitch            |
| **10**  | Demonstração            | Deploy local ou Streamlit Cloud, apresentação final           |

---

## 💻 Exemplos de Código

### 📥 ETL - Limpeza de Dados (`etl.py`)

```python
import pandas as pd

# Carregar CSV bruto (exemplo: energia)
df = pd.read_csv('dados/raw/energia_aneel.csv')

# Selecionar e renomear colunas essenciais
df = df[['Data', 'Geração (MWh)']]
df.columns = ['data', 'geracao_MWh']

# Tratar dados nulos e converter datas
df.dropna(inplace=True)
df['data'] = pd.to_datetime(df['data'])

# Salvar processado
df.to_csv('dados/processed/energia.csv', index=False)
print("✅ Dados limpos salvos em dados/processed/energia.csv")
```

### 🔮 Modelo de Previsão (`prophet_model.py`)

```python
from prophet import Prophet
import pandas as pd

# Carregar dados tratados
df = pd.read_csv('dados/processed/energia.csv')
df = df.rename(columns={'data':'ds', 'geracao_MWh':'y'})

# Criar e treinar modelo
model = Prophet()
model.fit(df)

# Prever 6 meses à frente
future = model.make_future_dataframe(periods=180)
forecast = model.predict(future)

# Salvar previsão
forecast[['ds','yhat','yhat_lower','yhat_upper']].to_csv(
    'dados/processed/forecast.csv', index=False
)
print("📈 Previsão salva em dados/processed/forecast.csv")
```

### ⚠️ Índice de Risco (`risk_score.py`)

```python
import pandas as pd

# Exemplo simples: risco = variabilidade + penalidade se valor negativo
df = pd.read_csv('dados/processed/energia.csv')
df['variacao'] = df['geracao_MWh'].pct_change().abs()
score = df['variacao'].mean() * 100

risco = pd.DataFrame({
    'setor': ['Energia'],
    'score_risco': [round(score, 2)]
})

risco.to_csv('dados/processed/risco.csv', index=False)
print("⚠️ Score de risco gerado:", risco)
```

### 📊 Dashboard Interativo (`dashboard/app.py`)

```python
import streamlit as st
import pandas as pd
import plotly.express as px

st.set_page_config(page_title="i4DATA Painel", layout="wide")
st.title("🚀 i4DATA – Inteligência Preditiva Ceará")

# Previsão
df_forecast = pd.read_csv('dados/processed/forecast.csv')
fig = px.line(df_forecast, x='ds', y='yhat',
              title='Previsão de Geração de Energia (MWh)')
st.plotly_chart(fig, use_container_width=True)

# Risco
risco = pd.read_csv('dados/processed/risco.csv')
st.subheader("⚠️ Índice de Risco Regulatório")
st.dataframe(risco)
```

**Executar:**

```bash
streamlit run dashboard/app.py
```

---

## ✅ Critérios de Validação do MVP

| Critério              | Métrica de Validação                                 |
| --------------------- | ---------------------------------------------------- |
| **Funcionalidade**    | Dashboard roda localmente e exibe gráficos + tabelas |
| **Valor Percebido**   | Profissionais/investidores entendem os insights      |
| **Usabilidade**       | Interface simples, limpa e responsiva                |
| **Reprodutibilidade** | Outros podem reproduzir com novos dados              |

---

## 📦 Entregáveis Finais (Dia 70)

✅ **Painel Interativo** rodando localmente (Streamlit/Dash)  
✅ **Previsões** de geração de energia para 6 meses (Prophet)  
✅ **Índice de Risco Regulatório** calculado e visualizado  
✅ **Relatório Técnico** (3 páginas PDF ou notebook)  
✅ **Código Reprodutível** com documentação completa

---

## 🎓 Recursos de Aprendizado

| Tema                   | Recurso Gratuito                                                                  |
| ---------------------- | --------------------------------------------------------------------------------- |
| Prophet (Previsão)     | [Facebook Prophet Docs](https://facebook.github.io/prophet/docs/quick_start.html) |
| Pandas (Dados)         | [10 min to Pandas](https://pandas.pydata.org/docs/user_guide/10min.html)          |
| Streamlit (Dashboards) | [Streamlit Docs](https://docs.streamlit.io/)                                      |
| GitHub Básico          | [GitHub Hello World](https://guides.github.com/activities/hello-world/)           |
| API Banco Central      | [BCB API Docs](https://dadosabertos.bcb.gov.br/)                                  |

---

## 🚀 Como Começar

### 1. Clone o repositório

```bash
git clone https://github.com/capacitabrasil/e3-3-vabarbosa-cr-projeto02.git
cd e3-3-vabarbosa-cr-projeto02
```

### 2. Configure o ambiente

```bash
python -m venv venv
.\venv\Scripts\Activate.ps1  # Windows PowerShell
pip install -r requirements.txt
```

### 3. Baixe os dados

```python
# Execute o script de coleta
python codigos/fetch_data.py
```

### 4. Execute a análise

```bash
# ETL
python codigos/etl.py

# Modelagem
python codigos/prophet_model.py

# Dashboard
streamlit run dashboard/app.py
```

---

## 🎯 Resultado Esperado

Um **painel visual simples e funcional** que demonstra:

-   📈 Previsão de geração de energia no Ceará para os próximos meses
-   ⚠️ Índice de risco regulatório automatizado
-   🔄 Capacidade de replicação para outros setores (Porto, Finanças, etc.)
-   💼 Produto investível e escalável para apresentação a stakeholders

---

## 👥 Equipe

**Equipe 3.3 - V.A. Barbosa**  
Projeto 02 - Aplicações de Modelos Preditivos e Análise Regulatória

---

## 📬 Contato e Contribuições

Dúvidas ou sugestões? Abra uma **issue** no repositório ou entre em contato com os instrutores do C-Jovem Imersão.

**Vamos construir análises que geram impacto real!** 🚀📊💡

---

## 💫 Créditos

Desenvolvido com ❤️ pela **Equipe i4DATA** do programa C-Jovem Imersão

[📊 Dashboard](./dashboard) • [📝 Documentação](./documentos) • [💻 Código](./codigos)
