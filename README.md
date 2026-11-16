# PredGuard 4.0 — Pipeline Preditivo de Manutenção Industrial

![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python&logoColor=white)
![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-1.4%2B-orange?logo=scikit-learn&logoColor=white)
![Status](https://img.shields.io/badge/Status-Conclu%C3%ADdo-brightgreen)

## 📋 Sobre o Projeto

O **PredGuard 4.0** é um pipeline ponta a ponta de Ciência de Dados e Machine Learning desenvolvido para o contexto da **Indústria 4.0**. O sistema atua na previsão de falhas mecânicas em equipamentos fabris monitorados por sensores IoT, permitindo que equipes de manutenção ajam preventivamente antes que ocorram paradas não programadas na linha de produção.

### O Problema Resolvido
Paradas inesperadas em linhas de montagem geram prejuízos financeiros elevados e gargalos operacionais. A partir de uma base histórica de 10.000 registros de telemetria industrial (`manutencao_preditiva.csv`), o modelo classifica o estado operacional do equipamento em duas classes binárias:
* **`0` (Operação Normal):** Equipamento operando dentro dos parâmetros esperados (96,61% da base).
* **`1` (Falha Mecânica):** Ocorrência de avaria detectada (3,39% da base).

---

## 🛠️ Tecnologias e Técnicas Utilizadas

* **Linguagem:** Python 3.10+
* **Manipulação e Análise de Dados:** `pandas`, `numpy`
* **Visualização de Dados (EDA):** `matplotlib`, `seaborn`
* **Pré-processamento e Balanceamento:** `scikit-learn` (`train_test_split` estratificado, `StandardScaler`), `imbalanced-learn` (`SMOTE`)
* **Modelagem Preditiva:** `KNeighborsClassifier` (KNN) e `DecisionTreeClassifier` (Árvore de Decisão)
* **Ambiente de Desenvolvimento:** Jupyter Notebook (`.ipynb`) e Git/GitHub (Gitflow)

### Arquitetura do Pipeline (7 Fases)
1. **Análise Exploratória de Dados (EDA):** Inspeção dimensional, estatística descritiva, distribuição de classes, histogramas e matriz de correlação de Pearson.
2. **Limpeza e Tratamento de Dados (Data Prep):** Verificação de duplicatas, análise de outliers via boxplots e imputação estatística pela **mediana** (evitando distorções causadas pela assimetria na velocidade de rotação).
3. **Feature Engineering:** Criação da variável física `potencia` ($\text{Velocidade de Rotação [RPM]} \times \text{Torque [Nm]}$) e remoção rigorosa das colunas de diagnóstico pós-falha (`falha_twf`, `falha_hdf`, `falha_pwf`, `falha_osf`, `falha_rnf`) para evitar *Data Leakage*.
4. **Divisão e Balanceamento:** Split estratificado (80% treino / 20% teste) seguido da aplicação exclusiva do **SMOTE** no conjunto de treino.
5. **Escalonamento de Variáveis:** Aplicação do `StandardScaler` (`fit_transform` no treino, `transform` no teste) apenas para o algoritmo baseado em distância (KNN), preservando a escala original para a Árvore de Decisão.
6. **Ajuste de Hiperparâmetros e Combate ao Overfitting:** Avaliação sistemática de $K \in \{3, 5, 7, 9\}$ no KNN e `max_depth` $\in \{3, 5, 7, \text{None}\}$ na Árvore de Decisão.
7. **Avaliação Final e Veredito:** Comparação das métricas de acurácia em dados inéditos de teste e seleção do modelo vencedor.

---

## 📁 Estrutura do Repositório

```text
predguard-4.0/
├── data/
│   └── manutencao_preditiva.csv    # Base de dados histórica dos sensores
├── notebooks/
│   └── pipeline_preditivo.ipynb    # Notebook principal documentado em 7 fases
├── README.md                       # Documentação oficial do projeto
└── requirements.txt                # Dependências e versões para reprodutibilidade
```

---

## 🚀 Como Executar o Projeto

1. **Clone o repositório:**
   ```bash
   git clone https://github.com/seu-usuario/predguard-4.0.git
   cd predguard-4.0
   ```

2. **Crie e ative um ambiente virtual (recomendado):**
   ```bash
   python -m venv venv
   # Windows:
   venv\Scripts\activate
   # Linux/macOS:
   source venv/bin/activate
   ```

3. **Instale as dependências:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Execute o Jupyter Notebook:**
   ```bash
   jupyter notebook notebooks/pipeline_preditivo.ipynb
   ```
   Execute todas as células sequencialmente (`Run All`). Todos os caminhos de leitura utilizam referências relativas (`../data/manutencao_preditiva.csv` ou `data/manutencao_preditiva.csv`).

---

## 🎥 Demonstração em Vídeo

* **Link do Vídeo no Google Drive:** https://drive.google.com/file/d/1vn1z2OOtptMBKRKAu3LySE530u2L9Wn3/view?usp=drivesdk

---

## 🔮 Melhorias Futuras

* **Otimização de Métricas de Negócio:** Além da acurácia global, incorporar o monitoramento de **Recall (Sensibilidade)** e **F1-Score** para minimizar falsos negativos (falhas reais não detectadas).
* **Modelos Ensemble:** Testar algoritmos baseados em comitês de árvores, como `RandomForestClassifier` e `XGBoost`, para elevar o poder preditivo.
* **Validação Cruzada Estratificada (`StratifiedKFold`):** Aplicar *Cross-Validation* integrada a um `imblearn.pipeline.Pipeline` para validar a estabilidade dos hiperparâmetros em múltiplas partições.
* **Deploy em Tempo Real:** Disponibilizar o modelo treinado via API REST (FastAPI) integrada a um dashboard de monitoramento contínuo no chão de fábrica.
