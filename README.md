# 🔍 FraudShield — Detecção de Fraudes em Transações Financeiras

> Pipeline completo de Machine Learning para identificação de transações fraudulentas, com pré-processamento, modelagem, avaliação de métricas e geração de relatórios.

---

## 📌 Sobre o Projeto

O **FraudShield** é um projeto de ciência de dados voltado à **detecção automática de fraudes em transações financeiras**. Utilizando técnicas de aprendizado de máquina supervisionado, o pipeline processa dados históricos de transações, treina modelos classificadores e avalia seu desempenho com métricas adequadas para problemas de **classes desbalanceadas** — característica central em datasets de fraude.

O projeto foi desenvolvido com foco em **reprodutibilidade**, **clareza do código** e **boas práticas de ML**, cobrindo desde a análise exploratória até a geração de relatório final com métricas e visualizações.

---

## ✨ Funcionalidades

- 📥 **Ingestão e validação de dados** — carregamento e checagem de integridade do dataset
- 🔎 **Análise Exploratória (EDA)** — distribuição de classes, correlações, outliers
- ⚙️ **Pré-processamento** — normalização, encoding, tratamento de nulos e balanceamento com SMOTE
- 🤖 **Treinamento de modelos** — Random Forest, Logistic Regression e XGBoost
- 📊 **Avaliação com métricas focadas em fraude** — Precision, Recall, F1-Score, AUC-ROC, Confusion Matrix
- 📄 **Relatório automático** — exportação de métricas e gráficos em `.txt` e `.png`
- 🔁 **Pipeline reprodutível** — execução end-to-end com um único comando

---

## 🛠️ Tecnologias Utilizadas

| Categoria | Biblioteca |
|---|---|
| Linguagem | Python 3.11+ |
| Manipulação de dados | Pandas, NumPy |
| Machine Learning | Scikit-learn |
| Balanceamento | Imbalanced-learn (SMOTE) |
| Visualização | Matplotlib, Seaborn |
| Serialização de modelos | Joblib |
| Utilitários | tqdm, python-dotenv |

---

## 🗂️ Estrutura do Projeto

```
fraudshield/
├── data/
│   ├── raw/                    # Dados originais (não versionados)
│   └── processed/              # Dados pré-processados
│
├── models/
│   └── trained/                # Modelos serializados (.joblib)
│
├── outputs/
│   ├── figures/                # Gráficos gerados
│   └── reports/                # Relatórios de métricas
│
├── src/
│   ├── __init__.py
│   ├── config.py               # Caminhos, parâmetros e seed global
│   ├── data_loader.py          # Carregamento e validação dos dados
│   ├── eda.py                  # Análise exploratória e visualizações
│   ├── preprocessor.py         # Pipeline de pré-processamento
│   ├── trainer.py              # Treinamento e seleção de modelos
│   ├── evaluator.py            # Métricas, curvas ROC e relatório
│   └── pipeline.py             # Orquestrador end-to-end
│
├── main.py                     # Ponto de entrada
├── requirements.txt
├── .env.example
└── README.md
```

---

## 🚀 Como Executar

### Pré-requisitos

- Python 3.11+
- Dataset de transações (ver seção [Dataset](#-dataset))

### Instalação

```bash
# Clone o repositório
git clone https://github.com/seu-usuario/fraudshield.git
cd fraudshield

# Crie e ative o ambiente virtual
python -m venv venv
source venv/bin/activate      # Linux/Mac
venv\Scripts\activate         # Windows

# Instale as dependências
pip install -r requirements.txt
```

### Configuração

```bash
cp .env.example .env
```

Edite o `.env` com o caminho do seu dataset:

```env
DATA_PATH=data/raw/transactions.csv
RANDOM_SEED=42
TEST_SIZE=0.2
```

### Execução

```bash
# Pipeline completo
python main.py

# Apenas EDA
python main.py --step eda

# Apenas treinamento
python main.py --step train

# Apenas avaliação
python main.py --step evaluate
```

---

## 📊 Dataset

O projeto é compatível com o dataset público **[Credit Card Fraud Detection](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud)** disponível no Kaggle.

Coloque o arquivo em `data/raw/creditcard.csv` antes de executar.

> O dataset contém 284.807 transações, sendo apenas 492 (0,17%) fraudulentas — um exemplo clássico de desbalanceamento extremo de classes.

**Estrutura esperada do CSV:**

| Coluna | Descrição |
|---|---|
| `Time` | Segundos desde a primeira transação |
| `V1` a `V28` | Features anonimizadas (PCA) |
| `Amount` | Valor da transação |
| `Class` | 0 = legítima, 1 = fraude |

---

## 📈 Resultados Esperados

Com o pipeline padrão (Random Forest + SMOTE), resultados típicos no dataset do Kaggle:

| Métrica | Valor |
|---|---|
| AUC-ROC | ~0.98 |
| Precision (fraude) | ~0.92 |
| Recall (fraude) | ~0.85 |
| F1-Score (fraude) | ~0.88 |

> ⚠️ Métricas de **Recall** e **AUC-ROC** são priorizadas neste problema: é preferível um falso positivo a deixar uma fraude passar.

---

## 🧠 Decisões Técnicas

**Por que SMOTE e não apenas class_weight?**
O SMOTE gera amostras sintéticas da classe minoritária no espaço de features, permitindo que o modelo aprenda fronteiras de decisão mais ricas. `class_weight='balanced'` é mais simples, mas menos eficaz em desbalanceamentos extremos como este.

**Por que Random Forest como modelo principal?**
Robusto a outliers, não exige normalização das features e produz feature importances interpretáveis — essencial para explicar ao negócio quais variáveis mais indicam fraude.

**Por que AUC-ROC como métrica principal?**
A acurácia é enganosa em datasets desbalanceados. Um modelo que classifica tudo como legítimo teria 99,8% de acurácia — mas seria inútil. AUC-ROC mede a capacidade discriminatória real do modelo.

---

## 🧪 Testes

```bash
pytest tests/ -v --tb=short
```

---

## 📄 Licença

Este projeto está sob a licença MIT. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.

---

<div align="center">
  Desenvolvido com 🛡️ para fins educacionais e de portfólio
</div>
